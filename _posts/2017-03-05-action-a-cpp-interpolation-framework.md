---
title: "Action: A C++11 interpolation framework"
description: "Easing, tweening, interpolating. Whatever you know it as – it holds a special place in my heart."
---

##### Before I begin, the repo for Action can be found [here.](http://github.com/tyskwo/action) #####
{: .notice--primary }

Since [Flip]({{ site.url }}{{ site.baseurl }}/work/96_flip/), almost three years ago now, I've used interpolation in every game that I've created – starting with [Starling](http://gamua.com/starling/), and then moving to [iTween in Unity](http://www.pixelplacement.com/itween/index.php), and [SKActions in SpriteKit](https://developer.apple.com/reference/spritekit/skaction). Interpolation is used everywhere in games, from UI animations, to AI actions, animations, and even world building. It truly could be looked at as a universal tool in a game developer's toolbox.


## What is interpolation?

Interpolation is a method of constructing new data within the range of a discrete set of known data. Put simply, it's the translating of one piece of data into another piece of data.

[Casey Muratori](https://mollyrocket.com/casey/stream_0018.html), who worked extensively on _The Witness_, has a great video on the basics of interpolation, which I highly, _highly_ suggest watching. He explains bezier curves in 40 seconds better than other's have done in half an hour.

{% include video id="S2fz4BS2J3Y" provider="youtube" %}


## Introducing Action.

I've been wanting to roll my own solution for a while now. Not necessarily because I think I can do it better, or include more features, but so I have it in my arsenal if I ever need it, and gain experience in creating the backend of such a system.

Thus, Action was created.

##### What it is.

It is a time-based, self-contained interpolation framework. It contains the [common list of easing functions](http://easings.net), and the ideology of a `Single` action. `Single` contains a value to interpolate, start/end values of that interpolation, a duration of how long the interpolation will take, a start-delay, a loop type (currently `None, Normal, or Yoyo`), an easing type, and an optional callback to be called upon completion of the interpolation.

There are also `Group` and `Sequence` actions: the former is a list of `Singles` that are run in concurrency, with an optional callback when the entire group is finished, and the latter is similar, except the `Singles` run sequentially.

The value to be interpolated is a reference to memory, and thus can be applied to any data of any object: volume of a sound clip, scale of a transform, rotation of a quaternion, etc.

##### What it isn't.

The easing functions (called `TimingFunctions` in the code base) are exposed, so more can be added, but currently there is no support for bezier/custom curves. Because they're stored as function pointers, there's no way to include more parameters in the call without casting another `typedef`. I'm experimenting to find the best way to implement this.

There currently is also no option to pause actions, but that is due to the fact of me focusing on the interpolation framework more than the timer it runs on.

## How to use it.

First, include `Action.h`.

Let's create a value:

{% highlight cpp %}
// imagine t is some object,
// with testInt being some int in that class.

//                 variable to change, 'start' value, 'end' value
Value<int> value { &t->testInt,         0,             100 };
{% endhighlight %}

There are common interpolations generally referred to as `To`, `From`, and `FromTo`. The above is an example of `FromTo`. `To` and `From` could be modeled by doing:

{% highlight cpp %}
// To:
Value<int> value { &t->testInt, t->testInt, 100 };

// From:
Value<int> value { &t->testInt, 0, t->testInt };
{% endhighlight %}

### Singles

Now let's take that value to interpolate, and create a `Single` from it:

{% highlight cpp %}
Action::addSingle(&value,
                  3.0f,   // duration  
                  0.5f,   // delay
                  TimingFunctions::ExponentialEaseIn);
{% endhighlight %}

There's a lot of customization in `addSingle`:

{% highlight cpp %}
>(Value<T>* value)
>(Value<T>* value, float duration)
>(Value<T>* value, float duration, Callback cb)
>(Value<T>* value, float duration, TimingFunction tf)
>(Value<T>* value, float duration, TimingFunction tf, Callback cb)
>(Value<T>* value, float duration, TimingFunction tf, Callback cb, LoopTypes::Type loop)
>(Value<T>* value, float duration, float delay, TimingFunction tf)
>(Value<T>* value, float duration, float delay, TimingFunction tf, Callback cb)
>(Value<T>* value, float duration, float delay, TimingFunction tf, Callback cb, LoopTypes::Type loop)
{% endhighlight %}

**Note:** For any variable that is not set in the method call, the default is constructed: 0 seconds for delay, 1 second for duration, no looping, no callback, and `Linear` for the timing function.
{: .notice--info }

Rather than calling one of the predefined methods, you can create a `Single` on your own:

{% highlight cpp %}
Single<T>* s = new Single<T>(&value);

s->setDuration(3.0f);
s->setTimingFunction(TimingFunctions::BounceEaseOut);
s->setDelay(1.5f);
s->setLooptype(LoopTypes::Yoyo);
s->setCallback(cb); // more on callbacks at the end of this post

Action::addSingle(s);
{% endhighlight %}

`Action::addSingle` automatically starts the interpolation (including its delay).

### Groups and Sequences

Now what about groups and sequences?

Let's first create a list of `Singles`:

{% highlight cpp %}
std::vector<SingleBase*> singleList = std::vector<SingleBase*>
{
    Action::createSingle(&value1,
                         3.0f,
                         2.5f,
                         TimingFunctions::ExponentialEaseIn),

    Action::createSingle(&value2,
                         3.0f,        // no delay on this one.
                         TimingFunctions::ExponentialEaseOut)
};
{% endhighlight %}

You'll notice here that we call `Action::createSingle` as opposed to `Action::addSingle`. The former simply returns a `Single` object without adding it to the pool of actions, whereas the latter creates the `Single`, adds it to the pool, but doesn't return anything.
{: .notice--warning }

You can also create `Singles` like above on your own and create a list of them that way.
{: .notice }

After the list of singles is constructed, creating a `Group` or `Sequence` is very straightforward:

{% highlight cpp %}
Callback groupCB    = std::bind([&](){ std::cout <<"GROUP COMPLETE\n";    }),
         sequenceCB = std::bind([&](){ std::cout <<"SEQUENCE COMPLETE\n"; }),

Action::addGroupFrom   (singleList, // list of singles     
                        1.0f,       // additional delay on top of singles' delays     
                        groupCB);   // callback for when the entire group is done

Action::addSequenceFrom(singleList,      
                        1.0f,             
                        sequenceCB);
{% endhighlight %}

Like `Singles`, `Action::addGroupFrom` and `addSequenceFrom` automatically starts the interpolations.

&nbsp; All that's left is to call `Action::update();` in your main game loop (otherwise none of the actions will run).

### A word on callbacks.

Callbacks are typedefs of `std::function<void()>`. Thus, we can use `std::bind` and lambda function notation to create callbacks:

{% highlight cpp %}
Callback exampleCB = std::bind([&](){ std::cout <<"SINGLE COMPLETE\n"; });
{% endhighlight %}

The beauty of them being lambdas means any code can be executed as a callback. Going back to our example `t` class from the start of this post:

{% highlight cpp %}
Callback cb = std::bind([&]()
{
    int temp = 0;

    while(temp < 100)
    {
        t->someRandomFunction(temp);
        example += 5;
    }
});
{% endhighlight %}

Creating a `Single` with a callback is as simple as:

{% highlight cpp %}
Action::addSingle(&value,
                  3.0f,   // duration  
                  0.5f,   // delay
                  TimingFunctions::ExponentialEaseIn,
                  std::bind([&](){ std::cout << "SINGLE COMPLETE"; }));
{% endhighlight %}

Or, using the above callback:

{% highlight cpp %}
Callback cb = std::bind([&]()
{
    int temp = 0;

    while(temp < 100)
    {
        t->someRandomFunction(temp);
        example += 5;
    }
});


Action::addSingle(&value,
                  3.0f,   // duration  
                  0.5f,   // delay
                  TimingFunctions::ExponentialEaseIn,
                  cb);
{% endhighlight %}
