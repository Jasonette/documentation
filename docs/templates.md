# ■ What is a template?

Sometimes we may want to dynamically generate the JSON from the client side instead of directly using a JSON returned from the server.

It may be for rendering a local user input, it may be for rendering a result from a 3rd party API. There are several cases where templating makes sense. [Learn more here]()

To achieve this we use templates.

1. Templates have slots (`{{ }}`) to be filled in.
2. All expressions inside `{{ }}` are evaluated with the data in memory and substituted in.
3. Templates handle not only evaluation but also support **looping (`#each`) and conditionals (`#if/#elseif/#else`)**.
4. Templates are always declared under `$jason.head.templates`.
5. Most of the times we just use the `body` template (`$jason.head.templates.body`).

---

## valid expressions
Jasonette template engine takes advantage of the native javascript engine. This means:

### 1. Any expression that evaluates to a value.

    {
      "items": [
        {
          "type": "image",
          "url": "{{$jason.image}}"
        },
        {
          "type": "label",
          "text": "{{$jason.username}}"
        }
      ]
    }

### 2. Any javascript expression.
Jasonette implements the native javascript engine to evaluate expressions. So you can use any javascript expression inside `{{ }}`.

    {
      "items": [
        {
          "type": "label",
          "url": "Full JSON string"
        },
        {
          "type": "label",
          "text": "{{JSON.stringify($jason)}}"
        }
      ]
    }

### 3. A full fledged javascript function.
If it involves multiple instructions, you can even write a full fledged function inside the template expression.

Just make sure to end with a `return` statement.

    {
      "items": [
        {
          "type": "label",
          "url": "Reversed Fullname"
        },
        {
          "type": "label",
          "text": "{{var sorted_posts = $jason.posts.sort(function(a,b){ return new Date(b.created_at) - new Date(a.created_at); }); return sorted_posts[0];}}"
        }
      ]
    }


## example
Here's an example where we use the `$geo.get` action to get the current location, and render it dynamically using the template.

    {
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
                    "text": "Latitude: {{ $jason.coord.split(',')[0] }}"
                  },
                  {
                    "type": "label",
                    "text": "Longitude: {{ $jason.coord.split(',')[1] }}"
                  }           
                ]
              }]
            }
          }
        }
      }
    }

In above JSON markup, the `$jason.body` part is missing, because we will dynamically generate it via `$render`.

The `$render` action will render the data with the template, and insert it into where `$jason.body` should be. The result would be:

    {
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
                    "text": "Latitude: {{ $jason.coord.split(',')[0] }}"
                  },
                  {
                    "type": "label",
                    "text": "Longitude: {{ $jason.coord.split(',')[1] }}"
                  }           
                ]
              }]
            }
          }
        },
        "body": {
          "sections": [{
            "items": [
              {
                "type": "label",
                "text": "Latitude: 12.1234"
              },
              {
                "type": "label",
                "text": "Longitude: 23.2345"
              }           
            ]
          }]
        }
      }
    }

Notice how we now have the `$jason.body` filled out.

---

# ■ Syntax

## JSON
Let's take a look at how JSON templating works:

---

### 1. Loop (#each)

To demonstrate looping, let's look at an example. We have a static JSON that looks like this:

    {
      "body": {
        "sections": [{
          "items": [
            {
              "type": "label",
              "text": "Homer"
            },
            {
              "type": "label",
              "text": "Marge"
            },
            {
              "type": "label",
              "text": "Lisa"
            },
            {
              "type": "label",
              "text": "Bart"
            },
            {
              "type": "label",
              "text": "Maggie"
            }
          ]
        }]
      }
    }

IF you look at each item, the only part that's custom is the name ("Homer", "Marge", "Lisa", "Bart", "Maggie"). We want to shorten this so that we don't have to rewrite `"type": "label"` for every item.

First, we need to declare a data attribute ([Learn more about `head.data`](#1-inline-data)):

    {
      "$jason": {
        "head": {
          "data": {
            "members": [
              { "name": "Homer" },
              { "name": "Marge" },
              { "name": "Lisa" },
              { "name": "Bart" },
              { "name": "Maggie" }
            ]
          }
        }
      }
    }

Then we will declare a `body` template that will iterate through this `members` array and turn each into renderable item.

    {
      "$jason": {
        "head": {
          "data": {
            "members": [
              { "name": "Homer" },
              { "name": "Marge" },
              { "name": "Lisa" },
              { "name": "Bart" },
              { "name": "Maggie" }
            ]
          },
          "templates": {
            "body": {
              "sections": [{
                "items": {
                  "{{#each members}}": {
                    "type": "label",
                    "text": "{{name}}"
                  }
                }
              }]
            }
          }
        }
      }
    }

The `#each` keyword will iterate through the expression that comes after it (`members`) and generate a JSON array from the result, ending up with the final JSON markup we saw at the beginning.

### How to access variables from inside nested #each

When there's only a single `#each` expression it's simple. But when we have multiple `#each` expressions, dealing with context becomes a bit tricky.

Here's an example:

    {
      "{{#each families}}": {
        "{{#each members}}": {
          "type": "label",
          "text": "{{name}}"
        }
      }
    }
    
What if we want to access `families` object from inside the nested `{{#each members}}` loop? 

We can't just do:

    {
      "{{#each families}}": {
        "{{#each members}}": {
          "type": "label",
          "text": "{{families.length}}"
        }
      }
    }

because inside the `{{#each members}}` loop, the context is each family member. Above expression will result in the parser trying to access for example `members[0].families.length` instead of `families.length`. It will throw an error because a `member` object doesn't contain a `families` attribute.

We need a way to access the root context. This is where `$root` comes in.

Whenever you're inside a loop, you can refer to the root context using `$root`. So above example will be:

    {
      "{{#each families}}": {
        "{{#each members}}": {
          "type": "label",
          "text": "{{$root.families.length}}"
        }
      }
    }

You can use the `$root` object to access everything at the root level, such as `$get` (through `$root.$get`), `$cache` (through `$root.$cache`), `$global` (through `$root.$global`), etc.


---

### 2. Conditional (#if/#elseif/#else)

Conditionals are used to conditionally render their children only when the expression evaluates to `true`.

Conditionals take the form of an `array`.

  1. The parser walks through the array sequentially
  2. Executes each conditional expression
  3. And Renders the child JSON of the first conditional expression that evaluates to `true`

#### syntax
Conditionals take the following format.

    [
      {
        "{{#if (EXPRESSION A)}}": (JSON)
      },
      {
        "{{#elseif (EXPRESSION B)}}": (JSON)
      }
      {
        "{{#else (EXPRESSION C)}}": (JSON)
      }
    ]

The template will walk through the items in the array sequentially until it encounters an conditional expression that's true. Then it will only render its child JSON.

#### Note

- `#elseif` and `#else` are optional.
- if no conditional expression evaluates to `true`, nothing gets rendered.


#### Example

Let's say we are are trying to render the following return value (`$jason`):

    {
      "data": {
        "name": "Homer"
      }
    }

What happens when we run above data through the following template?

    {
    	"type": "label",
    	"text": [
    		{
    			"{{#if $jason.data.name=='Bart'}}": "Ay Caramba!"
    		},
    		{
    			"{{#elseif $jason.data.name=='Homer'}}": "Donuts..."
    		}
    	]
    }

It will render the following result:

    {
      "type": "label",
      "text": "Donuts..."
    }

### 3. "this"
`this` is a javascript keyword used to refer to the current context. Let's look at what that means:

For example we want to generate this JSON using `#each`:

    [
      {
        "type": "label",
        "text": "Homer"
      },
      {
        "type": "label",
        "text": "Marge"
      },
      {
        "type": "label",
        "text": "Lisa"
      },
      {
        "type": "label",
        "text": "Bart"
      },
      {
        "type": "label",
        "text": "Maggie"
      }
    ]

If our data looks like this:

    {
      "members": [{"name": "Homer"}, {"name": "Marge"}, {"name": "Lisa"}, {"name": "Bart"}, {"name": "Maggie"}]
    }

We can write the following template:

    {
      "{{#each members}}": {
        "type": "label",
        "text": "{{name}}"
      }
    }

But what if it looked like this:

    {
      "members": ["Homer", "Marge", "Lisa", "Bart", "Maggie"]
    }

Now we're lost. Since each individual element in the `members` array is just a string instead of an object, **we need some way to refer to the object itself**.

This is where `this` comes in. To handle this situation we can write the following template:

    {
      "{{#each members}}": {
        "type": "label",
        "text": "{{this}}"
      }
    }

Keep in mind that the change in context makes global objects such as `$get`, and `$cache` inaccessible. You can use the `$root` object to get at them, e.g. `$root.$get`.

### 4. Advanced

Since inception, the template engine has added a lot of more useful features, such as

- [$index](https://selecttransform.github.io/site/transform.html#9-index) for keeping track of the current item's index within a loop
- [#let](https://selecttransform.github.io/site/transform.html#10-local-variables) API to define local variables
- [#concat](https://selecttransform.github.io/site/transform.html#5-concat) for merging two arrays
- [#merge](https://selecttransform.github.io/site/transform.html#6-merge) for merging two objects
- [#?](https://selecttransform.github.io/site/transform.html#4-existential-operator): Existential operator for including or excluding a key/value pair altogether based on the parsed result

The template itself has become too much of a sophisticated beast that it's been spun out to a separate project.

You can view the full documentation here: https://selecttransform.github.io/site/transform.html


## Non-JSON

Let's take a look at how non-JSON (CSV, RSS, HTML) templating works:

### CSV
When you have a raw CSV content, you can parse it into JSON format before feeding it into a template.

To do this, use [$convert.csv](actions.md#convertcsv)

**[Here's a functional example](http://www.jasonbase.com/things/B1m/edit)**

---

### RSS
When you have an RSS content, you can parse it into JSON format before feeding it into a template.

To do this, use [$convert.rss](actions.md#convertrss)

**[Here's a functional example](http://www.jasonbase.com/things/2dL/edit)**

---

### HTML
Unlike other formats like CSV and RSS, Jasonette implements a separate HTML template engine, so we don't need to parse HTML into JSON.

Instead, we convert HTML DOM elements into JSON, using the built-in **HTML to JSON parser**, which is built on top of [Cheerio library](https://github.com/cheeriojs/cheerio), which has similar syntax to [jQuery](#http://www.jquery.com)

#### How to use
##### Step 1. Make a `$network.request`
It starts with an HTML content. You can fetch HTML content by making `$network.request` calls with `data_type` of `html`, like this:

    {
      "$jason": {
        "head": {
          "actions": {
            "type": "$network.request",
            "options": {
              "url": "http://www.techmeme.com/river",
              "data_type": "html"
            }
          }
        }
      }
    }

##### Step 2. `$render` as html
In order to render it using the html parser, you need to call `$render` with `data_type` of `html`:

    {
      "$jason": {
        "head": {
          "actions": {
            "type": "$network.request",
            "options": {
              "url": "http://www.techmeme.com/river",
              "data_type": "html"
            },
            "success": {
              "type": "$render",
              "options": {
                "type": "html"
              }
            }
          }
        }
      }
    }

##### Step 3. Use jQuery syntax to parse and render

The HTML template engine automatically sets the `<body>` element as `$jason`.

From there we can use the [jQuery](http://www.jquery.com) syntax to parse and render content:

    {
      "$jason": {
        "head": {
          "actions": {
            "type": "$network.request",
            "options": {
              "url": "http://www.techmeme.com/river",
              "data_type": "html"
            },
            "success": {
              "type": "$render",
              "options": {
                "type": "html"
              }
            }
          },
          "templates": {
            "body": {
              "sections": [
                {
                  "items": {
                    "{{#each $jason.find('tr.ritem')}}": {
                      "type": "vertical",
                      "components": [
                        {
                          "type": "label",
                          "text": "{{$(this).find('td > a').text()}}"
                        },
                        {
                          "type": "label",
                          "text": "{{$(this).find('cite').text()}}"
                        },
                        {
                          "type": "label",
                          "text": "{{$(this).find('td').first().text() + '  ' + $(this).closest('table').prev().text()}}"
                        }
                      ],
                      "href": {
                        "view": "web",
                        "url": "{{$(this).find('td > a').attr('href')}}"
                      }
                    }
                  }
                }
              ]
            }
          }
        }
      }
    }

**[Here's a functional example](http://www.jasonbase.com/things/5yp/edit)**

---

# ■ When to use templates
Normally you can just return a static JSON document from the server and Jason would do its job to render it. However sometimes you may want to dynamically render the view.

Here are some cases where using a template makes sense:

1. Make a separate network request for data, then render the response
2. Dynamically render local data
3. Dynamically render data generated from device sensors
4. Separate data from template for less redundancy

Let's take a look at each:

---

### 1. Separate data from view
Make a separate network request for data, then render the response
For example here's a JSON markup that renders a list of labels:

    {
      "$jason": {
        "head": {
          ...
        },
        "body": {
          "sections": [{
            "items": [{
              "type": "label",
              "text": "This is row 1"
            }, {
              "type": "label",
              "text": "This is row 2"
            }, {
              "type": "label",
              "text": "This is row 3"
            }, {
              "type": "label",
              "text": "This is row 4"
            }, {
              "type": "label",
              "text": "This is row 5"
            }, {
              "type": "label",
              "text": "This is row 6"
            }, {
              "type": "label",
              "text": "This is row 7"
            }, {
              "type": "label",
              "text": "This is row 8"
            }]
          }]
        }
      }
    }


As you can see, the `"type": "label"` part is repeated for each item.

Instead of this static JSON, we can use a template/data approach:

1. Return a `body template`.
2. Make a separate network request on `$load` just to fetch the data.
3. Render the fetched data using the body template.

Here's what it looks like:

    {
      "$jason": {
        "head": {
          ...
          "actions": {
            "$load": {
              "type": "$network.request",
              "options": {
                "url": "https://jasonclient.org/rownames.json"
              },
              "success": {
                "type": "$render"
              }
            }
          },
          "templates": {
            "body": {
              "sections": [{
                "items": {
                  "{{#each $jason.result}}": {
                    "type": "label",
                    "text": "This is row {{row_name}}"
                  }
                }
              }]
            }
          }
        }
      }
    }

1. Notice there's no `body` under `$jason` here (`$jason.body`). It's because we're going to dynamically generate the body using the body template inside `templates` (`$jason.head.templates.body`).

2. Also notice the [`actions` attribute contains a $load attribute](actions.md#1-load), so this will be triggered as soon as the view loads.

So putting all these together, here's what's going on:

  1. The Jason app loads the JSON shown above, from our server.
  2. There's no `body` attribute so nothing is rendered on the screen by default. However, notice there's a `body` attribute under `templates`. This is the template that will be rendered as the `body` later.
  3. Immediately after the view loads, the `$load` action gets automatically triggered by the system.
  4. `$load` makes a `$network.request` call with the url specified. We will need to return the following data from the API:

      {
        "result": [{
          "row_name": "1"
        }, {
          "row_name": "2"
        }, {
          "row_name": "3"
        }, {
          "row_name": "4"
        }, {
          "row_name": "5"
        }, {
          "row_name": "6"
        }, {
          "row_name": "7"
        }, {
          "row_name": "8"
        }]
      }

  5. Once `$network.request` succeeds, its `success` action gets executed next. In this case it's `$render`.
  6. `$render` draws the view [using the `body` template and the data from the network request](actions.md#render).
  7. The result is the same as the [original static JSON](#1-separate-data-from-view)

- **Network request to a 3rd party API:** Let's say we want to build a Twitter client. What we want to do is:

  1. Fetch the data by making a network request to Twitter API.
  2. Render the data using our own template.

- **Instant plug and play:** Most web development frameworks nowadays come with JSON API right out of the box. This means you can simply write a template and render your own existing API.


### 2. Local user input

Dynamically render local data:

You can render templates using any type of data, which includes **local variables** you can set using form components such as:

- textfield
- textarea
- [search](document.md#search)
- etc.


**Example:** Below, we render the label using a local variable named `message`, which is automatically set whenever the `textfield` value changes. **Note that there is no top level `body` element after `head`.** Instead we have a `body` template, which will be rendered into body whenever we call the `$render` action.

    {
      "$jason": {
        "head": {
          ...
          "actions": {
            "$load": {
              "type": "$render"
            },
            "$pull": {
              "type": "$render"
            }
          },
          "templates": {
            "body": {
              ...
              "items": [
                {
                  "type": "textfield",
                  "name": "message"
                },
                {
                  "type": "label"
                  "text": "{{$get.message}}"
                }
              ]
              ...
            }
          }
        }
      }
    }

### 3. Device API generated data

Dynamically render data generated from device APIs

- geolocation
- addressbook
- camera
- timer
- etc.

**Example:** Below, we access the geolocation device sensor and render its result. Since our server has no knowledge of the device sensor data, templates are the only way to go in this case.


    {
      "$jason": {
        "head": {
          ...
          "actions": {
            "$load": {
              "type": "$geo.get",
              "success": {
                "type": "$render"
              }
            }
          },
          "templates": {
            "body": {
              ...
              "items": [
                {
                  "type": "label"
                  "text": {{$jason.coord}}"
                }
              ]
              ...
            }
          }
        }
      }
    }

### 4. Reduce redundancy
Separate data from template for less redundancy

Sometimes you simply want to separate view from model to avoid lots of code redundancy. See the below [data](#1-inline-data) section for details.

---

# ■ What can be rendered

## 1. Inline data

The `head.data` attribute is used to automatically fill in the `body` template if one exists.

When a view loads,

1. Jasonette looks at `$jason.head.data` and `$jason.head.templates.body`.
2. If both exist, it dynamically generates the view using the data and the template, and inserts it into `$jason.body`.

Here's a Jason markup **without** a template/data. As you can see, the label items mostly repeat, except for the `text` attribute.

    {
      ...
      "body": {
        "sections": [
          "items": [
            {
              "type": "label",
              "text": "Ethan",
              "style": {
                "color": "#000000",
                "size": "14",
                "font": "HelveticaNeue-Bold",
                "padding": "10",
                "background": "rgba(0,0,0,0.5)",
                "width": "300",
                "height": "100"
              }
            }
            ...
            {
              "type": "label",
              "text": "John",
              "style": {
                "color": "#000000",
                "size": "14",
                "font": "HelveticaNeue-Bold",
                "padding": "10",
                "background": "rgba(0,0,0,0.5)",
                "width": "300",
                "height": "100"
              }
            }
            {
              "type": "label",
              "text": "Samantha",
              "style": {
                "color": "#000000",
                "size": "14",
                "font": "HelveticaNeue-Bold",
                "padding": "10",
                "background": "rgba(0,0,0,0.5)",
                "width": "300",
                "height": "100"
              }
            }
          ]
        ]
      }
    }

Using template/data, we can reduce it down to:

    {
      "head": {
        "data": {
          "names": ["Ethan", ..., "John", "Samantha"]
        },
        "templates": {
          "body": {
            "sections": [{
              "items": {
                "{{#each names}}": {
                  "type": "label",
                  "text": "{{this}}",
                  "style": {
                    "color": "#000000",
                    "size": "14",
                    "font": "HelveticaNeue-Bold",
                    "padding": "10",
                    "background": "rgba(0,0,0,0.5)",
                    "width": "300",
                    "height": "100"
                  }
                }
              }]
            }
          }
        }
      }
    }

## 2. Return value of an action

But templates really shine when you use it to render dynamic data, produced by running some action on the device.

This is essential, since your server has no knowledge of what it should render if the data to render is a result of user interaction.

[See here](actions.md#rendering-return-value-from-the-previous-action) for details.

## 3. Manually specify data

This is rarely needed, but sometimes we need a way to manually specify data for the `$render` action.

[See here](actions.md#specifying-data-when-rendering) for details.
