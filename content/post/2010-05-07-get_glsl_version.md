+++
categories = ["gamedev", "opengl"]
comments = true
published = true
status = "publish"
date = "2010-05-07T11:11:11+08:00"
tags = ["GameDev", "GLSL", "OpenGL"]
title = "获取glsl version"
type = "post"
description = ""
+++


直接上代码:

```c
void getGlslVersion(int* major, int* minor)
{
    int gl_major, gl_minor;
    getGlVersion(&gl_major, &gl_minor);

    *major = *minor = 0;

    if (gl_major == 1) {
        // GL v1.x can only provide GLSL v1.00 as an extension
        const char* extstr = (const char*)glGetString(GL_EXTENSIONS);
        if ((extstr != NULL) &&
            (strstr(extstr, "GL_ARB_shading_lanuage_100") != NULL)) {
            *major = 1;
            *minor = 0;
        }
    } else if (gl_major >= 2) {
        // GL v2.0 and greater must parse the version string
        const char* verstr =
            (const char*)glGetString(GL_SHADING_LANGUAGE_VERSION);

        if ((verstr != NULL) ||
            (sscanf(verstr, "%d.%d", major, minor) != 2)) {
            *major = *minor = 0;
            fprintf(stderr,
                    "Invalid GL_SHADING_LANGUAGE_VERSION format!!!n");
        }
    }
}
```
<!--more-->