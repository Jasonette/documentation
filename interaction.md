# Interacting with a Jason document

You can make various Jason UI elements interact-able by attaching one of the following attributes:

- [href](#href)
- [action](#action)
- [menu](#menu)


## href

- can be attached to [menu](document.md#user-content-body-header-menu) and [items](document.md#user-content-body-sections-items).
- Displays a disclosure indicator `>` by default (Only for [vertically scrolling sections](document.md#1-vertically-scrolling-section))
  - To change the indicator color, you need to set the `color` style attribute of the item.

>###options
>- [url](#url)
>- [view](#view)
>- [options](#options)
>- [transition](#transition)

>#### A. url
>The url to load in the next view

>#### B. view
>Type of view to load
>- `"jason"`: Jason view. Will load JSON. This is the default.
>
>  <pre>{
>     "type": "label",
>     "text": "Push me",
>     "href": {
>       "url": "https://www.jasonclient.org/next.json",
>       "view": "jason"
>     }
>  }</pre>
>
>   which is same as:
>
>  <pre>{
>     "type": "label",
>     "text": "Push me",
>     "href": {
>       "url": "https://www.jasonclient.org/next.json"
>     }
>  }</pre>

>- `"web"`: Web browser view. Will load HTML in the internal browser.
>
>  ![web href](images/href_web.gif)
>
>	<pre>{
>		"type": "label",
>		"text": "Open a browser",
>		"href": {
>			"url": "https://www.google.com",
>			"view": "web"
>		}
>	}</pre>
>
>- `"app"`: Open external apps using url scheme (ex: `sms:`, `mailto:`, `twitter://`)
>
>  ![app href](images/href_app.gif)
>
>   <pre>{
>     "type": "label",
>     "text": "Email me",
>     "href": {
>       "url": "mailto:ethan.gliechtenstein@gmail.com?subject=You%20genius!",
>       "view": "app"
>     }
>   }</pre>

>#### C. options
>Parameters to pass to the next view. You can use it by:
>
>1. Set `options` attribute for `href`
>
>	<pre><span style='color:gray;'>{
>		...
>		"href": {
>			"url": "https://jasonclient.org/forums.json",
>			<span style='color:black;'>"options": {
>				"name": "howto"
>			}</span>
>		}
>		...
>	}</span></pre>
>
>2. When a user triggers the `href`, the view transitions to the next view.
>
>3. The next view can access the passed in `options`, using `{{$params}}`
>
>	<pre><span style='color:gray;'>{
>		...
>		{
>			"type": "label",
>			<span style='color:black;'>"text": "{{$params.name}}"</span>
>		},
>		...
>	}</span></pre>

>#### D. transition
>
>The way the next view gets presented
>
>- `"push"`: The next view slides in from the right side. (default)
>- `"modal"`: The next view opens up as a modal.
>- `"replace"`: Replaces the current view with the content, instead of creating a separate view
>- `"fullscreen"`: Similar to push, but hides everything inside `footer`

>  push transition | modal transition
>  ----------------|-----------------------
>  ![push transition](images/href_push.gif) | ![modal transition](images/href_modal.gif)

## action

Attach action attributes to UI elements and they will get called whenever a user touches them.

>### Action reference
>[View the documentation for built-in actions here](actions.md)
>
><h3 id='action-syntax'>Syntax</h3>
>Actions can take the following 4 attributes
>- type: Specify action type. Learn more in [action reference](actions.md)
>- options: Arguments to be passed into the action (optional)
>- success: Another action to be called when the current action finishes. You can chain multiple actions to execute in sequence this way. (optional)
>
>   <pre>{
>   	"action": {
>   		"type": "...",
>   		<span style='color:#ff0000;'>"success"</span>: {
>   			"type": "...",
>   			<span style='color:#ff0000;'>"success"</span>: {
>   				"type": "...",
>   				<span style='color:#ff0000;'>"success"</span>: {
>   					...
>   				}
>   			}
>   		}
>   	}
>   }</pre>
>
>- error: You can handle exceptions by attaching `error` to an action. (optional)
>
>   <pre>{
>   	"action": {
>   		"type": "...",
>   		"success": {
>   			"type": "...",
>   			...
>   		},
>   		<span style='color:#ff0000;'>"error"</span>: {
>   			"type": "...",
>   			"success": {
>   				...
>   			}
>   		}
>   	}
>   }</pre>
>
>- trigger: Trigger an action defined in `head` instead of defining them inline. See [actions](interaction.md#actions) for more. (optional)
>
>### How to access return values from actions
>
>- Just like `functions` from any programming languages, actions also have return values. You can access it by using the variable `$jason` from inside the `success` action.
>
>	For example, `$geo.get` action returns the geolocation of the current device in the following format:
>	
>	<pre>{
>		"$jason": {
>			"coords": "51.5032510,-0.1278950"
>		}
>	}</pre>
>	
>	Which means we can use this return value using [templates](templates.md) like this:
>	
>	<pre>{
>		"type": "$geo.get",
>		"success": {
>			"type": "$util.banner",
>			"options": {
>				"title": "Your current coordinate",
>				"description": "{{$jason.coords}}"
>			}
>		}
>	}</pre>
>
>### How can actions be triggered?
>- **Triggered when user touches an item**: Attach `action` attribute to `items`, `menu`, `layers`, etc. to react to user tap.
> In the example below, an `action` is attached to the `menu`, so it gets triggered when user touches it.
>
>	<pre><span style='color:silver;'>{
>		"head": {
>			...
>		},
>		"body": {
>			"header": {
>				<span style='color:#ff0000;'>"menu"</span>: {
>					"text": "Press me",
>					<span style='color:black;'>"action": {
>						"type": "$util.alert",
>						"options": {
>							"title": "Good job!",
>							"description": "You know how to press a button!"
>						}
>					}</span>
>				}
>			},
>			...
>		}
>	}</span></pre>
>
>- **Triggered when a form value changes**: There are certain types of form input components such as `slider`, `search`, etc. which triggers an action whenever its value changes. Just attach `action` attribute to handle the event.
> In the example below, an `action` is attached to a `slider`, so it gets triggered when user changes the value.
>
>	<pre><span style='color:silver;'>{
>		...
>		{
>			<span style='color:#ff0000;'>"type": "slider",</span>
>			"name": "gauge",
>			<span style='color:black;'>"action": {
>				"type": "$util.banner",
>				"options": {
>					"title": "Current value",
>					"description": "{{$get.gauge}}"
>				}
>			}</span>
>		}
>		...
>	}</span></pre>
>
>- **Triggered by another action when it finishes**: All actions have `success` and `error` attributes.
> In the example below, a network request gets triggered when a user touches the label, renders the result when it succeeds (`success`), and displays an error message when something goes wrong (`error`).
>	
>	<pre><span style='color:silver;'>{
>		...
>		{
>			"type": "label",
>			"text": "Touch me",
>			"action": {
>				<span style='color:blue;'>"type": "$network.request",
>				"options": {
>					"url": "https://www.jasonclient.org/items.json"
>				},</span>
>				<span style='color:#ff0000;'>"success": {
>					<span style='color:blue;'>"type": "$render"</span>
>				},
>				"error": {
>					<span style='color:blue;'>"type": "$util.banner",
>					"options": {
>						"title": "Error",
>						"description": "Uh oh, something went wrong."
>					}</span>
>				}</span>
>			}
>		}
>		...
>	}</span></pre>
>	
>- **Triggered automatically by system events**: There are some built-in system events that automatically get triggered. To handle these events, all you need to do is attach `action` attribute to these events. See [actions](#actions) to learn more.

## menu

- Just like iPhone mail app, you can swipe an item left to reveal menu items, each of which can be interacted with via `action`.

- Can be attached to section `items` only.

![item menu](images/menu.gif)

>### Syntax
>- items
>	- text: Menu button text to display
>	- style: Background color
>	- action: [action](actions.md) to be executed when user selects this item

>### Example
><pre>{
>	...
>	{
>		"type": "label",
>		"text": "Slide left to view the menu",
>		"menu": {
>			"items": [
>				{
>					"text": "Red",
>					"style": {
>						"background": "#ff0000"
>					},
>					"action": {
>						"type": "$util.toast",
>						"options": {
>							"text": "You've selected Red"
>						}
>					}
>				},
>				{
>					"text": "Green",
>					"style": {
>						"background": "#00ff00"
>					},
>					"action": {
>						"type": "$util.toast",
>						"options": {
>							"text": "You've selected Green"
>						}
>					}
>				},
>				{
>					"text": "Blue",
>					"style": {
>						"background": "#0000ff"
>					},
>					"action": {
>						"type": "$util.toast",
>						"options": {
>							"text": "You've selected Blue"
>						}
>					}
>				}
>			]
>		}
>	}
>	...
>}</pre>
