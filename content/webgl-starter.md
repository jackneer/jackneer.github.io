Title: WebGL Starter
Date: 2019-03-21T20:37:03+08:00
Category: programming
Tags: webgl
Author: Jack

Create index.htnl file with following content

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="utf-8">
        <title>WebGl Test</title>
        <script>
            function initWebGL() {
                var canvas = document.getElementById("gl")
                var gl = canvas.getContext("webgl")
                // check for webgl support
                if (gl) {
                    console.log("WebGL ready")
                    // set viewport
                    gl.viewportWidth = canvas.width
                    gl.viewportHeight = canvas.height
                    // set clear color
                    gl.clearColor(0.0, 0.0, 0.0, 1.0)
                    // clear buffer
                    gl.clear(gl.COLOR_BUFFER_BIT)
                } else {
                    console.log("WebGL not supported")
                }
            }            
        </script>
    </head>
    <body onload="initWebGL()">
        <canvas id="gl" width="640" height="480"></canvas>
    </body>
</html>
```

Open index.html with your browser, a black box (webgl viewport) should appear.