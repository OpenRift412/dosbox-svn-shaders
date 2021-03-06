# A few GLSL shaders for DOSBox SVN

I have been working on writing shaders for the new version of DOSBox SVN (r4319). There are currently 6 shaders included in this repo:

- interpolation/pixellate.glsl - a port of the Pixellate shader by Fez and included in [libretro](https://github.com/libretro/glsl-shaders/blob/master/interpolation/shaders/pixellate.glsl).
- interpolation/pp.glsl - a pixel-perfect shader with aspect ratio correction, based on [Marat Tanalin's algorithm](https://tanalin.com/en/articles/integer-scaling/). A [Shadertoy implementation](https://www.shadertoy.com/view/3dsyW7) is also available.
- crt/CRT-EasyMode_tweaked.glsl - duganchen's CRT-EasyMode port, adapted by liPillON with some parameters slightly tweaked.
- crt/CRT-Lottes_tweaked.glsl - MartyShepard's adaptation of duganchen's CRT-Lottes port, adapted for DOSBox SVN, and by liPillON with some parameters slightly tweaked.
- crt/ScanLine.glsl - duganchen's ScanLine shader, adapted by liPillON
- template.glsl - a shader that doesn't change anything and can be used as a template for other shaders.

In order to get these shaders working, you will need to make the following changes to the `dosbox-SVN.conf` configuration file:

    [sdl]
    fullresolution = desktop
    output = openglnb

    [render]
    scaler = none
    glshader = [name of .glsl file]

The window size will not increase automatically, so you will need to manually set the window resolution or switch to full screen. If the full screen resolution option is not set to `desktop`, the image will be stretched and filtered.

Setting `output` to `openglnb` enables OpenGL output and disables bilinear filtering. To enable bilinear filtering set the output to `opengl` instead.

Shaders can be copied to the DOSBox root directory, or alternatively to the `glshaders` subdirectory in the DOSBox configuration directory. The shader can be referenced by the filename, without the .glsl extension. If the shader fails to load, or does not compile correctly, an error/warning message can be seen in the DOSBox output window.

I have tested the pixel-perfect shader code against [Marat Tanalin's reference implementation](https://tanalin.com/en/projects/integer-scaling/) and the original DOSBox [Pixel perfect patch](http://www.vogons.org/viewtopic.php?f=32&t=49160) originally included in [DOSBox ECE](https://dosboxece.yesterplay.net/en/). This implementation provides the identical level of scaling for the majority of the input/output resolutions I tested. Testing was conducted using DOSBox SVN r4334, which does not appear to be using the full resolution of the monitor in full screen mode, and therefore some results will be different to the pixel perfect scaling seen in DOSBox ECE. This is probably something that needs to be changed in the DOSBox OpenGL rendering code, but is not something I have looked at yet.
