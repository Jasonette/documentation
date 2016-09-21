# Templates

- Syntax
	- [{{ }} notation](#user-content-brackets)
	- [Loop](#user-content-loop)
	- [Conditional](#user-content-conditional)
	- ["this"](#user-content-this)
- Parsing non-JSON content
	- [CSV](#csv)
	- [RSS](#rss)
	- [HTML](#html)

## JSON

<h3 id='brackets'>1. `{{   }}` notation</h3>
>- The expression inside `{{ }}` is evaluated via javascript engine and substituted in.
>- The expression inside the brackets must be:
>	1. a javascript function call that returns a value.
>	2. a value

>#### Example
><pre>{
  "$jason": {
    "head": {
      "title": "Display location",
      "actions": {
      	"type": "$geo.get",
      	"success": {
      		"type": "$render"
      	}
      },
      "templates": {
        "body": {
          "sections": [{
            "items": [
              {
                "type": "label",
                "text": "Latitude: <span style='color:#ff0000;'>{{ $jason.coords.split(',')[0] }}</span>"
              },
              {
                "type": "label",
                "text": "Longitude: <span style='color:#ff0000;'>{{ $jason.coords.split(',')[1] }}</span>"
              }           
            ]
          }]
        }
      }
    }
  }
}</pre>

> Once the `$render` is complete, the client will have the following rendered body markup

><pre>{
>	"body": {
>		"sections": [{
>			"items": [
>				{
>					"type": "label",
>					"text": "Latitude: 12.1234"
>				},
>				{
>					"type": "label",
>					"text": "Longitude: 23.2345"
>				}           
>			]
>		}]
>	}
>}</pre>


<h3 id='loop'>2. Loop (#each)</h3>

>Let's imagine we have the following data:
>#### Data
><pre>{
>  "data": {
>    "family": "Simpson",
>    "<span style='color:#ff0000;'>members</span>": [
>      { "name": "Homer" },
>      { "name": "Marge" },
>      { "name": "Lisa" },
>      { "name": "Bart" },
>      { "name": "Maggie" }
>    ]
>  }
>}</pre>

> To build a Jason view out of this, we can iterate through each member and render labels using `#each`. Here's what the template may look like:
>####Template
><pre><span style='color:silver;'>{
>	...
>	"templates": {
>		<span style='color:black;'>"body": {
>			"sections": [{
>				"items": {
>					"<span style='color:#ff0000;'>{{#each members}}</span>": {
>						"type": "label",
>						"text": "{{name}}"
>					}
>				}
>		}</span>
>	}
>}</span></pre>

> - The `#each` keyword is used to iterate through the expression that comes after it (`members`)
> - `#each` expects an array value, so it would fail if `members` was not an array.

>####Result
> The template will render the data to create the following JSON, which now can be safely rendered as a Jason view.
><pre>{
>	"body": {
>		"sections": [{
>			"items": [
>				{
>					"type": "label",
>					"text": "Homer"
>				},
>				{
>					"type": "label",
>					"text": "Marge"
>				},
>				{
>					"type": "label",
>					"text": "Lisa"
>				},
>				{
>					"type": "label",
>					"text": "Bart"
>				},
>				{
>					"type": "label",
>					"text": "Maggie"
>				}
>			]
>		}]
>	}
>}</pre>

<h3 id='conditional'>3. Conditional (#if/#elseif/#else)</h3>

>- Use conditionals to render child expressions conditionally.
>- Conditionals take the form of an `array`
>	1. The parser walks through the array sequentially
>	2. Executes each conditional expression
>	3. And Renders the child JSON of the first conditional expression that evaluates to `true`

>####Syntax
>- Conditionals take the following format.
>- `#elseif` and `#else` are optional.
>- if no conditional expression evaluates to `true`, nothing gets rendered.
>
>	<pre>[
>		{
>			"{{#if (EXPRESSION A)}}": (Jason markup A)
>		},
>		{
>			"{{#elseif (EXPRESSION B)}}": (Jason markup B)
>		}
>		{
>			"{{#else (EXPRESSION C)}}": (Jason markup C)
>		}
>	]</pre>

> Here, the template will go through the items in the array sequentially until it encounters an expression that's true. Then it will only render the corresponding markup.

>####Example
>#####Data
>Let's say we have the following data
><pre>{
>	"$jason": {
>		"data": {
>			"name": "Homer"
>		}
>	}
>}</pre>


>#####Template
>What happens when we run above data through the following template? 
><pre>{
>	"type": "label",
>	"text": <span style='color:#ff0000;'>[
>		{
>			"{{#if $jason.data.name=='Bart'}}": "Ay Caramba!"
>		},
>		{
>			"{{#elseif $jason.data.name=='Homer'}}": "Donuts..."
>		}
>	]</span>
>}</pre>

>#####Result
>It will render the following result:
><pre>{
>	"type": "label",
>	"text": <span style='color:#ff0000;'>"Donuts..."</span>
>}</pre>


<h3 id='this'>4. "this"</h3>
>- Since everything inside the brackets gets evaluated using the javascript engine, we can use the javascript keyword `this` to refer to the current context.

>####Example
- For example let's try to iterate through `memebers` in the following json:
>	<pre>{
>		"family": "Simpson",
>		"members": ["Homer", "Marge", "Lisa", "Bart", "Maggie"]
>	}</pre>

>- We use the `#each` keyword to iterate through `members`.
>- However each item in the `members` array is not an object, so there's no way to refer to them.
>- This is where `this` comes in. Here, `this` refers to each array item.

>	<pre>{
>		"{{#each members}}": {
>			"type": "label",
>			"text": "{{this}}"
>		}
>	}</pre>


##Parsing non-JSON content

###CSV
>- You can convert CSV (Comma Separated Values) content into JSON format before feeding into a template.
>- You can use [$convert.csv](#csv) action to do that.

>####Example
>- [Here's a functional example](http://www.jasonbase.com/things/B1m/edit)

###RSS
>- You can convert RSS content into JSON format before feeding into a template.
>- You can to use [$convert.rss](#rss) action to do that.

>####Example
>- [Here's a functional example](http://www.jasonbase.com/things/2dL/edit)


###HTML
>- Unlike other formats like CSV and RSS, HTML parser is built into the template engine.
>- You can convert HTML DOM elements into JSON, using the built-in **HTML to JSON parser**, which is built on top of [Cheerio library](https://github.com/cheeriojs/cheerio), which has similar syntax to [jQuery](#http://www.jquery.com)
>
>####How to use
>1. It starts with an HTML content. You can fetch HTML content by making `$network.request` calls with `data_type` of `html`, like this:
>	<pre>{
>		"type": "$network.request",
>		"options": {
>			"url": "http://www.techmeme.com/river",
>			"data_type": "html"
>		},
>		"success": {
>			..
>		}
>	}</pre>
>
>2. In order to render it using the html parser, you need to call `$render` with `type` of `html` as well. Extending the above example, we will have the following action call chain:
>	<pre>{
>		"type": "$network.request",
>		"options": {
>			"url": "http://www.techmeme.com/river",
>			"data_type": "html"
>		},
>		"success": {
>			"type": "$render",
>			"options": {
>				"type": "html"
>			}
>		}
>	}</pre>
>3. Here's what an HTML type template looks like (Notice the [jQuery](http://www.jquery.com)-like syntax):
>
>	<pre>"templates": {
>		"body": {
>			...
>			"sections": [
>				{
>					"items": {
>						"{{#each $jason.find('tr.ritem')}}": {
>							"type": "horizontal",
>							"href": {
>								"view": "web",
>								"url": "{{$(this).find('td > a').attr('href')}}"
>							},
>							"components": [
>								{
>									"type": "vertical",
>									"components": [
>										{
>											"type": "label",
>											"text": "{{$(this).find('td > a').text()}}"
>										},
>										{
>											"type": "label",
>											"text": "{{$(this).find('cite').text()}}"
>										},
>										{
>											"type": "label",
>											"text": "{{$(this).find('td').first().text() + '  ' + $(this).closest('table').prev().text()}}"
>										}
>									]
>								}
>							]
>						}
>					}
>				}
>			]
>		}
>	}</pre>
>
>####Example
>- [Here's a functional example](http://www.jasonbase.com/things/5yp/edit)

