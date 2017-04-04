---
title: "Action"
excerpt: "A C++11 interpolation framework."

header:
  image: /assets/images/portfolio/2017/action/feature.png
  teaser: /assets/images/portfolio/2017/action/feature.png

language: "C++"
year: "2017"
portfolio: "true"

sidebar:
  - text: "## 2017, C++ ##"


  - title: "Description"

    text: "A C++11 interpolation framework."


  - title: "Outcomes"

    text: "Lambda functions.


    C++11 features: std::function, std::bind.


    Library linking.


    Interpolators."


  - title: "Links"

  - button: Repository
    link: https://github.com/tyskwo/Action
    icon: github
---

_Action_ is a time-based, self-contained interpolation framework. It has the capability of interpolating values over a duration with delays, ease types, and completion callbacks. Because it uses templates and pointers to the values it is interpolating, _Action_ is flexible in what it can do: lower the volume of a sound clip, scale a transform, change the color of a particle, etc.

See [this blog post for an introduction on how to use Action.]({{ site.url }}{{ site.baseurl }}/2017/action-a-cpp-interpolation-framework/) This page is more about the architecture of it.


### Architecture

Because _Action_ interpolates values within the codebase of the project, it needs to store references to these values as well as the interpolation values. Commonly referred to as `To`, `From`, and `FromTo` tweens, _Action_ uses a simple templated struct to keep track of each value:

{% highlight cpp %}
template <typename T>
struct Value
{
    // values for the start and end of the interpolation
    // reference to the value we're interpolating,
    // which also acts as the 'current' value
    T start, end, *current;


    Value<T> (T *current, T start, T end)
    {
        // set the reference to current
        this->current = current;

        // set the value of current and start
        *this->current = (this->start = start);

        this->end = end;
    }

    // more overloded constructors...
}
{% endhighlight %}

At its root, _Action's_ interpolator runs off of the concept of a `Single` – a single interpolation. This is split into two layers: a base layer that handles the majority of the logic and a top layer that contains a `Value` and handles the templated nature of the system:

{% highlight cpp %}
class SingleBase : public ActionBase
{

protected:

    float          duration, elapsed;
    TimingFunction timingFunction;
    Timer          timer;


public:

    // getters and setters...

    void start()  { this->timer.start();  }
    void pause()  { this->timer.pause();  }
    void resume() { this->timer.resume(); }
    void stop()   { delete this;          }
};
{% endhighlight %}

And here's an overview of the top level:

{% highlight cpp %}
template <typename T>
class Single : public SingleBase
{

private:

   Value<T>* value;


public:

   bool update()
   {
       // get elapsed time
       elapsed = timer.getElapsedMS() / 1000.0f;

       if(elapsed > delay)
       {
           // get the current completion percentage
           float percent = (elapsed - delay) / duration;

           // we're finished
           if(percent >= 1.0f)
           {
               // make sure we don't over-pass the end value
               *value->current = value->end;

               callback();

               // update our values based on if we're looping
               OnComplete();

               return true;
           }

           // otherwise interpolate our value
           else *value->current = (value->start + ((timingFunction)(percent))*(value->end - value->start));
       }

       return false;
   }
}
{% endhighlight %}

If you're eagle-eyed, you'll notice that `SingleBase` inherits from `ActionBase` – this is a class that holds basic info shared across all types of actions, like `Groups` and `Sequences` (more on those in a bit).

***

`Singles` are stored in a vector, and are updated individually:

{% highlight cpp %}
namespace Action
{
    static std::vector<SingleBase*> singles   {};

    static void update()
    {
       for(int i = 0; i < singles.size(); i++)
       {
           singles.at(i)->update();

           // if the Action is completed, delete it
           if(singles.at(i)->isFinished() &&
              singles.at(i)->getLooptype() == LoopTypes::None)
           {
               delete singles.at(i); singles.at(i) = NULL;

               singles.erase(singles.begin() + i);
               ++i;
           }
       }
   }
}
{% endhighlight %}

Thus, the only setup needed from an external party is to `#include 'Action.h'` and to call `Action::update()` in its main update loop.

[The blog post I linked earlier goes more into how to use _Action_.]({{ site.url }}{{ site.baseurl }}/2017/action-a-cpp-interpolation-framework/)


### Features

In addition to interpolating single values, _Action_ supports interpolating groups of values in unison and in sequence. For `Groups`, if specified by the user, a group-wide delay is added to each of the `Singles` within a group, and then the group handles updating each single in unison. For `Sequences`, determining the delay for each of the `Singles` is a little more involved:

{% highlight cpp %}
// in Sequence.h:
void start()
{
    for(int i = (int)singles.size() - 1; i >= 0; i--)
    {
        auto&& s = singles.at(i);

        float delayForS = delay + s->getDelay();

        for(int j = i - 1; j >= 0; j--)
        {
            delayForS += singles.at(j)->getDelay() + singles.at(j)->getDuration();
        }

        delays.push_back(delayForS);
        s->setDelay(delayForS);

        if(looptype > LoopTypes::None) s->setLooptype(looptype);

        s->start();
    }
}
{% endhighlight %}

Depending on the looptype of the `Sequence,` the delay for the `Singles` has to be recalculated:

{% highlight cpp %}
// in Sequence.h:
if(looptype == LoopTypes::Yoyo)
{
    float delay = this->getDelay();

    if(heads)
    {
        for(int j = i + 1; j < singles.size(); j++)
        {
            delay += (singles.at(j)->getDuration() * 2.0f);
        }
    }
    else
    {
        for(int j = i - 1; j >= 0; j--)
        {
            delay += (singles.at(j)->getDuration() * 2.0f);
        }
    }
    singles.at(i)->setDelay(delay);
}
{% endhighlight %}

`heads` is a boolean that keeps track if the `Sequence` is in the 'downwards' or 'upwards' motion of the yo-yo. To get the correct delay, we take twice the length of the entire sequence, and add the current delay of the opposite `Single`: so if the entire sequence is 10 seconds long, and the current `Single` is second from the end, the delay is 20 seconds plus the delay of the second from the start.

##### Callbacks

Callbacks in _Action_ are `std::function<void()>`, which means that they can be used with `std::bind` and lambda function notation:

{% highlight cpp %}
Callback cb = std::bind([&](){ std::cout << "ACTION COMPLETE\n"; });

Callback cb2 = std::bind([&](){ t->someRandomFunction(temp); });
{% endhighlight %}

This allows the callbacks to be extremely flexible for the user.

##### Ease types

_Action_ has [all of the basic easing types built in](http://easings.net), using `std::function` pointers:

{% highlight cpp %}
std::function<float(float t)> CubicEaseIn = [](float t)
{
    return t * t * t;
};
{% endhighlight %}

In addition, cubic and quartic bezier curves are also supported:

{% highlight cpp %}
typedef std::function<float(float t)> TimingFunction;

template <typename T>
TimingFunction GetCurveFromPoints(T x1, T y1, T x2, T y2)
{
    // clamp x values to [0,1]
    if(x1 > 1) x1 = 1; if(x1 < 0) x1 = 0;
    if(x2 > 1) x2 = 1; if(x2 < 0) x2 = 0;

    TimingFunction Curve = [x1, y1, x2, y2](float t) -> float
    {
        // get the x value for the given time
        float x = (3 * (1 - t) * (1 - t) * t * x1) + (3 * (1 - t) * t * t * x2) + t * t * t;

        // get the y value for the given x value
        float y = (3 * (1 - x) * (1 - x) * x * y1) + (3 * (1 - x) * x * x * y2) + x * x * x;

        return y;
    };

    return Curve;
}
{% endhighlight %}

These clamp the points from 0 - 1 along the x-axis to make sure the integrity of the mathematical function isn't compromised.

### Retrospection and Postmortem

WIP.
