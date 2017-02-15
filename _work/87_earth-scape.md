---
title: "Earth-scape"
excerpt: "A modern OpenGL-rendered Earth scene."

header:
  image: /assets/images/portfolio/2016/earth-scape/feature.png
  teaser: /assets/images/portfolio/2016/earth-scape/feature.png

language: "C++"
year: "2016"
portfolio: "true"

sidebar:
  - text: "## 2016, C++ ##


### Description ###


A modern OpenGL-rendered Earth scene. Includes texture blending, bloom, godrays, and shadows.


### Outcomes ###


CMake.


Modern OpenGL.


Post-processing pipeline.


Third-party frameworks.


### Links ###


[Repository](https://github.com/tyskwo/OpenGL_Earth){: .btn}
[Executable](https://www.dropbox.com/s/hmibt5j0t8pevpj/OpenGL_Earth.zip?dl=0){: .btn}
"
---

This project was the culmination of Graphics II, which used core-profile OpenGL to create a simulation of the Earth, moon, and sun. The demo features bloom, 'godrays', shadow-casting, and a system for altering the related parameters to achieve the desired effect.

![earth-intro]({{ site.url }}{{ site.baseurl }}/assets/images/portfolio/2016/earth-scape/intro.gif){: .align-center}


### Architecture

The demo uses the TLoC engine which contains an entity-component system for handling game objects. I abstracted this system to better handle 3D objects, using a struct composed of its mesh, materials, and transform. This allowed for easier composition of game objects and a more readable implementation of rendering them. The moon and Earth are obviously spheres with textures. The sun is a billboard with a light attached to it.

This project was the first where I used CMake. The engine itself wasn't multi-platform, but I gained experience in using CMake, makefiles, and setting up a project architecture that is platform agnostic.

### Shaders

The Earth and the moon each contain a shader to render its textures, as well as shaders for bloom, HDR, shadow mapping, and godrays. To achieve this effect, I render to multiple textures and then combine them after all of the passes. I then apply gamma correction and render back into LDR.

{% highlight cpp %}
#version 330 core

in  vec2 v_texCoord;          //texture coordinate
out vec4 o_color;             //output color

uniform sampler2D s_texture;  //normal texture before post-processing
uniform sampler2D s_bright;   //the blooms and blur pass
uniform sampler2D s_godray;   //the godray pass
uniform float     u_exposure; //constant for exposure

uniform int       u_flag;     //flag for rendering only bloom or godrays

void main()
{
  //get the texture coordinate to sample from each texture
  vec2 tex_offset = 1.0 / textureSize(s_bright, 0);

  float texCoordX = tex_offset[0];
  float texCoordY = tex_offset[1];

  //get color from each texture, ignoring alpha
  vec3 hdr    = texture2D(s_texture, vec2(v_texCoord[0], v_texCoord[1])).rgb;
  vec3 bright = texture2D(s_bright,  vec2(v_texCoord[0], v_texCoord[1])).rgb;
  vec3 godray = texture2D(s_godray,  vec2(v_texCoord[0], v_texCoord[1])).rgb;

  //gamma correct with additive blending & tone mapping
  const float gamma = 1.0;
  hdr += bright;
  hdr += godray;

  vec3 result = vec3(1.0) - exp(-hdr * u_exposure);

  result = pow(result, vec3(1.0 / gamma));
  o_color = vec4(result, 1.0);


  if(u_flag == 1) { o_color = vec4(bright, 1.0); } //check for bloom render flag
  if(u_flag == 2) { o_color = vec4(godray, 1.0); } //check for godray render flag
}
{% endhighlight %}

<figure class="half">
    <img src="/assets/images/portfolio/2016/earth-scape/bloom.gif">
    <img src="/assets/images/portfolio/2016/earth-scape/depth.gif">
    <figcaption>The bloom and godray parameters can be altered during runtime to achieve the desired effect, and the stencils can be rendered on their own.</figcaption>
</figure>


### Retrospection and Postmortem

This project allowed me to gain a better understanding of the ins-and-outs of low-level graphics programming. I know more of OpenGL, and I have a much fonder respect for graphics programmers in the industry. The shader pipeline doesn't seem as scary anymore, and post-processing effects don't seem like voodoo. Shader logic, though, is still magic: I still have lots to learn there. This was also a good exercise in learning an already-existing framework, and creating an architecture around it.
