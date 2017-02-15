---
title: "Shadow Shader"
excerpt: "My first foray into graphics programming."

header:
  image: /assets/images/portfolio/2014/shadow-shader/feature.png
  teaser: /assets/images/portfolio/2014/shadow-shader/feature.png

language: "C#"
year: "2014"

sidebar:
  - text: "## 2014, C# ##


### Description ###


A 2D planet scene with an HLSL shader to render the shadows.


### Outcomes ###


XNA.


HLSL and the shader pipeline.

"
---

![shader-intro]({{ site.url }}{{ site.baseurl }}/assets/images/portfolio/2014/shadow-shader/example.gif){: .align-center}


This shader was my first. Like a first-time dad holding his newborn, I cherish this little guy. The project itself was to create a unique shader, which is why there's no game or anything attached to the project. I took the name a little too seriously (shadow shader – shade, haha get it?), but I feel it was good application and got my feet wet in HLSL and pixel shaders. Shadow simply simulates the sun's rays being cast on planets, which then produce shadows behind them as they orbit the sun. This gives rise to 'eclipse' like situations and shadows that double-up.

Along with the shader, this project also includes a robust particle system that takes care of the planet trails and the sun's bursting. Plus, there's some pretty relaxing space-y background music, making this truly fit for a screensaver.

### The Shader

The shader is its own file, and is included into the file like an object. The shader is a scene shader – meaning that the shader goes through all of the pixels on the screen, rather than it being attached to a sprite and it only going through the sprites visible pixels. Here's the code that incorporates the shader into the project:

{% highlight csharp %}
//shader and its parameters
Effect shadowEffect;
EffectParameter lightCenter, screenSize,		//center of sun and size of screen
                planet1Atop, planet1Abottom, 	
                planet2Atop, planet2Abottom, 	//left and right edge of planet
                planet3Atop, planet3Abottom,	//tangent to the sun
                planet4Atop, planet4Abottom;


//load shader and matchup parameters
shadowEffect = Content.Load<Effect>("shadow");
lightCenter = shadowEffect.Parameters["center"];
screenSize = shadowEffect.Parameters["size"];
planet1Atop = shadowEffect.Parameters["planet1AngleTop"];
planet2Atop = shadowEffect.Parameters["planet2AngleTop"]; . . . etc.


//set center of sun and screen size
lightCenter.SetValue(sun.position);
screenSize.SetValue(new Vector2(graphics.PreferredBackBufferWidth,
                                graphics.PreferredBackBufferHeight));


//for each of the planets, calculate its angle, and set the two sides
//of the planet with hard-coded values
float angle = (float)Math.Atan2(planet1.location.Y - 825.0f,
                                planet1.location.X - 400.0f);

planet1Abottom.SetValue(angle - 0.097f);
planet1Atop.SetValue(angle + 0.097f);
. . . etc.
{% endhighlight %}

### The Shadows

The shadows, obviously are the the meat of the shader. To keep the actual shader as lightweight as possible, I pass in the edge points of the planets each frame. These could very easily be calculated in the shader. Given these points, here is how the shader calculates the shadows:

{% highlight csharp %}
uniform extern texture ScreenTexture;

sampler ScreenS = sampler_state
{
	Texture = <ScreenTexture>;
};


/*
    center: position of sun
    size:   size of screen
    top:    left side of the edge of the planet
    bottom: right side of the edge of the planet
*/

float2 center;
float2 size;
float  planet1AngleTop, planet1AngleBottom;
float  planet2AngleTop, planet2AngleBottom;
float  planet3AngleTop, planet3AngleBottom;
float  planet4AngleTop, planet4AngleBottom;


float4 PixelShaderFunction(float2 currentCoord: TEXCOORD0) : COLOR
{
	//get the pixel, its color, and its distance from the sun
	float2 pixelCoord = currentCoord * size;
	float4 color      = tex2D(ScreenS, currentCoord);
	float  deltaX     = pixelCoord.x - center.x;
	float  deltaY     = pixelCoord.y - center.y;


	//calculate the angle and distance
	float angle    = atan2((deltaY) , (deltaX));
	float distance = sqrt((deltaY) * (deltaY) + (deltaX) * (deltaX));


	//if the calculated angle falls between the planet’s edges and is beyond
	//the planets orbit, darken it
	//
	//ifs (rather than else ifs) allow the pixel to be darkened multiple times,
	//allowing for the ellipse effect

	if ((angle < planet1AngleTop && angle > planet1AngleBottom) && distance > 250)
	{
		color.xyz = 0.65f * color.xyz;
	}
	if ((angle < planet2AngleTop && angle > planet2AngleBottom) && distance > 400)
	{
		color.xyz = 0.65f * color.xyz;
	}
	if ((angle < planet3AngleTop && angle > planet3AngleBottom) && distance > 500)
	{
		color.xyz = 0.65f * color.xyz;
	}
	if ((angle < planet4AngleTop && angle > planet4AngleBottom) && distance > 725)
	{
		color.xyz = 0.65f * color.xyz;
	}
	return color;
}


technique
{
	pass P0
	{
		PixelShader = compile ps_2_0 PixelShaderFunction();
	}
}
{% endhighlight %}

### Retrospection and Postmortem

I'm proud of what I was able to accomplish with this assignment, but looking back there is much that can be streamlined to make the code not only nicer-looking, but more modular as well, especially with the increase in GPU power over the past couple of years. For one, I would pass in the radius and center of each planet. While still passing in two values per planet, this would allow the shader to be more independent of the project and be able to calculate the edge points on its own. Secondly, I would get rid of the hard-coded values to determine the edge points, and calculate these based on the given values. This refactoring would allow the shader to loop through the planets, be more removed from the project, and just be neater overall.

![shader-intro]({{ site.url }}{{ site.baseurl }}/assets/images/portfolio/2014/shadow-shader/overlap.gif){: .align-center}

But that's not all, because there is one glaring, but hidden bug. The sun is at the center of the universe, but why is it at the bottom of the screen? When the angle is within the planet's radius' max angle (~2 degrees or so) from plus/minus 0 (so right around zero on the unit circle), the shader freaks out because the angle of the pixel isn't between the edge points based on the angle. The easiest way to fix this would be to introduce an edge-case and check for this for each pixel, but a better way would be to use quaternions. I looked into it for this project, and the math was way above my head, and even now too. Maybe a summer project is on the horizon.
