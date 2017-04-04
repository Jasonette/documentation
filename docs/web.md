# JSON Web Container

What if you could take any number of web elements, plug them straight into a native app layout, resize, scroll, interact with, and pretty much treat them just like any other [native components](components.md)?

<img src='../images/c.jpg' class='large'>

<br>

Web Containers are self-contained web views that render into native app layout as native components.

Of course, the great part is you can describe them all in JSON, just like the rest of [Jasonette components](components.md).

<br>

<img src='../images/header.jpg' class='large'>

<br>

Web containers are designed to be plugged into a native app layout alongside other [native components](components.md).

<br>

As you can see in above example, it's a combination of JSON and HTML.

First, use HTML/CSS/Javascript to describe the internals of the container view.

Then, [plug it into your native app using JSON](#usage) as just another [component](components.md).

<br>
<div class = 'vid'>
  <div class='content'>
    <div class='video-container'>
			<iframe width='1280' height='720' src="https://www.youtube.com/embed/Ww7siLEbJ0M" frameborder="0" allowfullscreen></iframe>
    </div>
  </div>
</div>
<br>

---

# Table of Contents

1. [Notice](#notice)
1. [Web containers in action](#web-containers-in-action)
1. [Features](#features)
1. [What is possible?](#what-is-possible)
1. [Usage](#usage)

<br>

---

# Notice

Before we jump in, just a few things:

1. The web container performs pretty nicely on modern mobile devices, in fact so nicely that sometimes you forget that you're using web containers. But don't go overboard with it. Native will always be more performant and you should use native whenever possible. Try experimenting and you'll reach the sweet spot.

2. A lot of design decisions around Web containers were made specifically for displaying rich dynamic content, which used to be tricky using just native components. Try to think of this as just another [component](components.md) and try not to use the web container to implement app logic. It will most likely result in bad user experience.

3. You can do a lot of cool things with this, such as implementing little animations here and there without having to invent a whole new JSON syntax because you can now just use CSS or Javascript animation. Also you can do things like text overlay over image simply by using CSS positioning. [Please share cool use cases you find](https://forum.jasonette.com).

4. This is just the first draft and intentionally minimal. We may add more features depending on how it goes. Feedback appreciated.

---

# Web Containers in Action

Web containers can be used both as a [fullscreen background](#or-use-as-fullscreen-background) and as an [inline component](#scroll-just-like-other-components). Here are some examples:

<br>

&shy; | &shy; 
--|--
Anime.js (Mix and match with native) | Native feed view + partial web
<img src='../images/anime.gif'> | <img src='../images/feed.gif'>
D3.js (Native slider controller + Web container output) | Canvas + JS Demo (Background type)
<img src='../images/gauge.gif'> | <img src='../images/particles.gif'> 
Anime.js (Animated HTML element inside native scroll view) | Phaser.js (Background type, with user interaction)
<img src='../images/rotate.gif'>| <img src='../images/phaserjs.gif'>
Three.js (background type) | Three.js (background type)
<img src='../images/threejs.gif'>| <img src='../images/threejstwo.gif'>


---

# Features

<br>
## Use alongside native components
Web containers plug into native layout alongside other native components so seamlessly that you won't even be able to tell the difference in most cases. It's just like any other [component](components.md)! Style and use them together with other native Jasonette components.

<img src='../images/aa.png' class='dark large'>

---

## Dissolve right into native layout
Same native [layout system](layout.md) you already know. 

<img src='../images/bb.png' class='dark large'>

---

## Scroll just like other components
Since it's just like any other component, you can use it in a scroll view, and scroll it just like label, button, image, etc.

**[Note] This is possible because of the [design decision](#1-web-container-content-scrolls-only-when-used-as-a-background) made to make the contents inside a web container NOT scrollable when contained within a scroll view (for obvious UX reasons).**

<img src='../images/cc.png' class='dark large'>

---

## Or use as fullscreen background
Not just for scrolling components. you can use it as a background by setting "html" type for background.

<img src='../images/dd.png' class='dark large'>

---

## Write once, use anywhere
You can use [mix-ins](mixin.md) to write a single HTML container inside JSON and mix them into any context such as the background or a layout.

<img src='../images/ee.png' class='dark large'>

---


# What is possible?
Since each Web container is basically a self-contained web view, you can do anything you can do with a browser, which includes any HTML markup, Javascript, and CSS styling.

<br>

## Canvas
<img src='../images/ff.png' class='dark large'>

---

## SVG & CSS
<img src='../images/svgclock.gif' class='dark large'>

---

## iframe
<img src='../images/gg.png' class='dark large'>

---

## Javascript
<img src='../images/hh.png' class='dark large'>

<img src='../images/ii.png' class='dark large'>

---

## Interaction between native actions and Web container
[Templates](templates.md) work exactly the same as the rest of Jasonette. So you can use native UI elements as input, set [local variables](http://localhost:8000/actions/#variable), and re-render accordingly using the local variable to dynamically display output onto the web container. 

<img src='../images/jj.png' class='dark large'>

---

## Mix and match
Jasonette already has most of what you need to build sophisticated native apps, even without the web container.

But web containers can be powerful when seamlessly blended into the rest of the native app to perform things that were hard to do with existing Jasonette api.

<img src='../images/kk.png' class='dark large'>

---


# Usage

- [Syntax](#syntax)
- [Examples](#examples)
- [Important Tips (MUST READ)](#tips)

<br>

## Syntax

<br>

### 1. As Component

Just like rest of the Jasonette [components](components.md), you can plug it into [layouts](layout.md) and [scroll views(sections)](document.md#bodysections). 

web containers plugged in everywhere | using web container to animate text via JS
------|-----------------
<img src='../images/feed.gif'> | <img src='../images/rotate.gif'>

* `type`: `"html"`
* `text`: A self contained HTML text (`<html>...</html>`), which means everything from `script` tag to `style` tag.
* `style`: **Required. Especially the height**. Web containers should NOT scroll, therefore everything outside of the `style.height` you specify will get cut off. This attribute is about how the web container looks from the outside. To style the HTML elements inside, just include the CSS inside the HTML markup. 
* `action`: (optional) Connect with any [action](actions.md) or a web container specific action: `"type": "$default"`.
    * **[IMPORTANT]** By default, web container content DOES NOT respond to user touch. This means if you click on a link inside the HTML, it won't do anything.
    * In order to override this behavior, attach a `"type": "$default"` action, so the web container knows.
    
```
{
  "items": [{
    "type": "html",
    "text": "<html>....</html>",
		"style": {
			"height": "100"
		},
    "action": {
      "type": "$default"
    }
  }]
}
```
<br>

### 2. As Background 

Web containers can be plugged into `body.style.background` too, which will take up the full screen.

Use any JS library | Interacting with HTML content via touch
------|-----------------
<img src='../images/threejstwo.gif'> | <img src='../images/phaserjs.gif'>

Same as component usage, except there's no need for `style`, and you can't attache an `action` other than `"type": "$default"`.

* `type`: `"html"`
* `text`: A self contained HTML text (`<html>...</html>`), which means everything from `script` tag to `style` tag.
* `action`: (optional) Connect with `"type": "$default"` in order to let the web container handle the touch event. Otherwise not needed.
    * **[IMPORTANT]** By default, web container content DOES NOT respond to user touch. This means if you click on a link inside the HTML, it won't do anything.
    * In order to override this behavior, attach a `"type": "$default"` action, so the web container knows.
    

```
{
  "body": {
    "style": {
      "background": {
        "type": "html",
        "text": "<html>....</html>",
        "action": {
          "type": "$default"
        }
      }
    }
  }
}
```


---

## Example

<br>

### Display scrolling H1 elements

```
{
	"$jason": {
		"head": {
			"title": "Scrolling Web Container"
			"data": {
				"colors": [{
					"code": "#ff0000"
				}, {
					"code": "rgba(0,0,0,0.3)"
				}, {
					"code": "#00ff00"
				}, {
					"code": "rgb(130,43,32)"
				}]
			},
			"templates": {
				"body": {
					"sections": [{
						"items": {
							"{{#each colors}}": {
								"type": "html",
								"style": {
									"height": "50"
								},
								"text": "<html><head><style>h1{font-size: 30px; color: {{code}};}</style></head><body><h1>Hello World</h1></body></html>"
							}
						}
					}]
				}
			}
		}
	}
}
```

<br>

### iframe as background

```
{
  "$jason": {
    "head": {
      "title": "pdf.js"
    },
    "body": {
      "style": {
        "background": {
          "type": "html",
          "text": "<html><head><style>html{padding:0;margin:0;margin-top: 64px;} body,iframe{width:100%;height:100%;padding:0;margin:0;border:0;}</style><body><iframe src='https://mozilla.github.io/pdf.js/web/viewer.html'/></body></html>"
        }
      }
    }
  }
}
```

<br>

### three.js as background

```
{
  "$jason": {
	"head": {
		"title": "pdf.js"
	},
	"body": {
		"style": {
			"border": "none",
			"color": "rgba(0,0,0,0)",
			"background": {
				"type": "html",
				"text": "<html> <head> <style> body { margin: 0; padding: 0; background: #111; overflow: hidden; } .dg{visibility: hidden;}</style> </head> <body> <script src='https://cdnjs.cloudflare.com/ajax/libs/dat-gui/0.6.2/dat.gui.min.js'></script><script src='https://cdnjs.cloudflare.com/ajax/libs/jquery/2.1.3/jquery.min.js'></script><script src='https://cdnjs.cloudflare.com/ajax/libs/three.js/r50/three.min.js'></script><script> window.onload = function(){ var $container = $('#container'); var renderer = new THREE.WebGLRenderer({antialias: true}); var camera = new THREE.PerspectiveCamera(80,window.innerWidth/window.innerHeight,0.1,10000); var scene = new THREE.Scene(); var mouseX = 0, mouseY = 0; scene.add(camera); renderer.setSize(window.innerWidth, window.innerHeight); $container.append(renderer.domElement); window.addEventListener( 'resize', onWindowResize, false ); var Controls = function() { this.speed = 2; this.rotation = 0; }; var text = new Controls(), gui = new dat.GUI(); gui.add(text, 'speed', 0, 10); gui.add(text, 'rotation',0,15); var normalMaterial = new THREE.MeshNormalMaterial({}); function Torus(f){ this.b = new THREE.Mesh(new THREE.TorusGeometry( 160, 75, 2, 13),normalMaterial); this.b.position.x = 57*Math.cos(f); this.b.position.y = 57*Math.sin(f); this.b.position.z = f*1.25; this.b.rotation.z = f*0.03; } var numTorus = 80; var tabTorus = []; for(var i=0; i<numTorus; i++){ tabTorus.push(new Torus(-i*13)); scene.add(tabTorus[i].b); }  function update(){ for(var i=0; i<numTorus; i++){ tabTorus[i].b.position.z+=text.speed; tabTorus[i].b.rotation.z+=i*text.rotation/10000; if(tabTorus[i].b.position.z>0) { tabTorus[i].b.position.z=-1000; } } } function onWindowResize() { windowHalfX = window.innerWidth / 2; windowHalfY = window.innerHeight / 2; camera.aspect = window.innerWidth / window.innerHeight; camera.updateProjectionMatrix(); renderer.setSize( window.innerWidth, window.innerHeight ); } function onDocumentMouseMove(event) { mouseX = ( event.clientX - windowHalfX ); mouseY = ( event.clientY - windowHalfY ); } function render() { requestAnimationFrame( render); camera.position.x += ( mouseX - camera.position.x ) * .05; camera.position.y += ( - mouseY - camera.position.y ) * .05; renderer.render(scene, camera); update(); } render(); }; </script> <div id='container'></div> </body> </html>"
  }
}
```

---


## Tips

** ⚠️  ⚠️  ⚠️  ⚠️  ⚠️  IMPORTANT. READ ALL!!!!     ⚠️  ⚠️  ⚠️  ⚠️  ⚠️  **

<br>

### 1. Web container content scrolls ONLY when used as a background

When you use a web container [as a background](#2-as-background), it scrolls just fine, and behaves like you would expect.

However when you use a web container [as a component inside a scrolling view](#1-as-component), the content inside **does not scroll** because the container itself is supposed to scroll. 



Which brings us to:

<br>

### 2. Must specify height when in scroll view
Every web container must have a fixed height when inside a scroll view (sections).

This is so that there exists no scrolling view within a scroll view, which is a bad user experience.

```
{
	...
	"sections": [{
		"items": [{
			"type": "html",
			"text": "<html>....</html>",
			"style": {
				"height": "100"
			},
			"action": {
				"type": "$default"
			}
		}, {
			...
		}],
		...
	}]
}
```

<br>

### 3. Use single quotes
Obviously, an entire HTML markup can be a long text. Sometimes it will contain double quotes and some characters that need to be escaped.

Since we're trying to package an entire HTML markup into a text attribute, you'll either need to escape these characters or use single quotes instead of double quotes.

<br>

### 4. Wrap all script content inside `window.onload = function() { ... }`

Because of the way native scroll views recycle their child items, you need to make sure your javascript lines are run AFTER the container has loaded.

To do this, wrap your Javascript inside `window.onload`. Here's an example:


```
{
  "type": "html",
  "text": "<html><head><style>.square{background: red; width: 50px; height: 50px;}</style><script src='https://cdnjs.cloudflare.com/ajax/libs/animejs/2.0.0/anime.min.js'></script><script>var x=200; var move = function() {var el = document.querySelector('#domNode .el'); var domNode = anime({ targets: el, translateX: x, translateY: x}); x=200-x;}; document.addEventListener('DOMContentLoaded', function(event) { setInterval(move, 1000); }); </script><body><div id='domNode'><div class='large square el'></div></div></body></html>"
}
```


<br>

### 5. By default, the HTML content does NOT respond to user input

HTML content inside a web container is NOT interactive by default. This means NOTHING will happen when you touch the contents inside, even if it's a link. (Especially if it's a link, because it would be weird to have a small portion of a native app redirect to a completely different web page)

<br>

The interaction is disabled by default because the web container is designed as a **dynamic output method** and **NOT as an input method**.

<br>

Web containers are very useful when it comes to **displaying rich dynamic content**, and this is the primary purpose of the web container. Also, in most cases we want to trigger native actions instead of letting the HTML content handle events. If we allowed HTML to handle user touch for example, then we wouldn't have a way to take advantage of Jasonette's [native actions](actions.md).

<br>

So whenever possible, use native components such as [textarea](components.md#textarea) or [textfield](components.md#textfield) to receive user input instead of trying to use the HTML`<form>` element. 

Also, keep in mind pure native components generally perform better too. Just use it wisely.

<br>

That said, there are some cases where it does make sense to let the HTML handle events. In which case you could:

<br>

### 6. Use the `"type": "$default"` action to force Jasonette to let HTML handle events.

If you know what you're doing, you can override the block and let the HTML handle user events.

```
{
	"type": "html",
	"text": "<html><body><script src='https://cdnjs.cloudflare.com/ajax/libs/jquery/2.1.3/jquery.min.js'><script>$(function(){$(document).on('click', function(){ $('body').css('background', '#ff0000'); }); });</script></body></html>",
	"action": {
		"type": "$default"
	}
}
```

<br>

## Technical Details

Under the hood, each web container is a containerized instance of web view ([UIWebView for iOS](https://developer.apple.com/reference/uikit/uiwebview) and [WebView for Android](https://developer.android.com/reference/android/webkit/WebView.html)).

It gets recycled when you scroll just like rest of the native components so it's pretty efficient.

