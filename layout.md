# Item layout

You can construct all kinds of sophisticated UIs for [section items/headers](document.md#user-content-body-sections) by laying out multiple [components](components.md) vertically, horizontally, or through combination of the two.

- Basic layout types
	1. [vertical](#vertical)
	2. [horizontal](#horizontal)
- [Build a nested layout](#build-a-nested-layout)
- [Style a layout](#style-a-layout)

## vertical
>In a vertical layout, the item's components flow vertically, from top to bottom.

>### Example
>
><pre><span style='color:silver;'>{
>	"$jason": {
>		"head": {
>		},
>		"body": {
>			...
>			"items": [
>				<span style='color:black;'>{
>					<span style='color:#ff0000;'>"type": "vertical",</span>
>					"components": [{
>						"type": "label",
>						"text": "John Doe"
>					}, {
>						"type": "label",
>						"text": "33"
>					}, {
>						"type": "label",
>						"text": "A normal guy with a normal name"
>					}]
>				}</span>
>			]
>		}
>	}
>}</span></pre>

## horizontal
>In a horizontal layout, the item's components flow horizontally, from left to right.
>
>### Example
>
><pre><span style='color:silver;'>{
>	"$jason": {
>		...
>		"body": {
>			...
>			"items": [
>				<span style='color:black;'>{
>					<span style='color:#ff0000;'>"type": "horizontal",</span>
>					"components": [{
>						"type": "image",
>						"text": "https://jasonclient.org/img/john.png"
>					}, {
>						"type": "text",
>						"text": "John Doe"
>					}]
>				}</span>
>			]
>		}
>	}
>}</span></pre>


## Build a nested layout
>A layout can also contain another layout as one of its components. This way you can create any type of sophisticated layout you want by nesting one layout inside another.
>
>Here's an example:
>
><pre><span style='color:silver;'>{
>	"$jason": {
>		"head": {
>		},
>		"body": {
>			...
>			"items": [{
>				<span style='color:#000000;'><span style='color:#ff0000;'>"type": "horizontal",
>				"components":</span> [
>					{
>						"type": "image",
>						"url": "https://jasonclient.org/img/john.png"
>					},
>					{
>						<span style='color:#ff0000;'>"type": "vertical",
>						"components":</span> [
>							{
>								"type": "label",
>								"text": "John Doe"
>							},
>							{
>								"type": "label",
>								"text": "Male"
>							}
>						]
>					}
>				]</span>
>			}]
>		}
>	}
>}</span></pre>

## Style a layout

### Available attributes
#### 1. padding
the space surrounding the layout itself, in pixels.
#### 2. spacing
the space among each immediate children components, in pexels.
#### 3. background
background color code (Example: `{"background": "#ff0000"}`, `{"background": "rgba(0,0,0,0.4)"}`)
#### 4. z_index
specifies the stack order of the layout, defining whether the layout is displayed on top of another or below. Default is 0. (Example: `{"z_index": "-1"}`
#### 5. align
How the children components will be aligned perpendicular to the layout's direction.

- **In case of vertical layout**, it describes how the children components should be aligned **horizontally**.

<pre><span style='color:silver;'>{
	"type": "vertical",
	"style": {
		<span style='color:#ff0000;'>"align": "center"</span>
	},
	<span style='color:black;'>"components": [
		{
			"type": "image",
			"url": "https://jasonclient.org/img/john.png"
		},
		{
			"type": "label",
			"text": "John Doe"
		},
		{
			"type": "button",
			"text": "Follow"
		}
	]</span>
}</span></pre>

- **In case of horizontal layout**, it describes how the children components should be aligned **vertically**.

<pre><span style='color:silver;'>{
	"type": "horizontal",
	"style": {
		<span style='color:#ff0000;'>"align": "top"</span>
	},
	<span style='color:black;'>"components": [
		{
			"type": "image",
			"url": "https://jasonclient.org/img/john.png"
		},
		{
			"type": "label",
			"text": "John Doe"
		},
		{
			"type": "button",
			"text": "Follow"
		}
	]</span>
}</span></pre>

- **Possible values**
	- For vertical layout
		- `left`
		- `center`
		- `right`
		- `fill`: stretch all children horizontally to fit the layout width
	- For horizontal layout
		- `top`
		- `center`
		- `bottom`
		- `fill`: stretch all children vertically to fit the layout height equally
