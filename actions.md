# Action reference

- Drawing and Transitioning views
	- [$reload](#reload)
	- [$render](#render)
  - [$snapshot](#snapshot)
  - [$href](#href)
  - [$close](#close)
  - [$back](#back)
- Network request
  - [$network.request](#networkrequest)
  - [$network.upload](#networkupload)
  - [$network.auth](#networkauth)
  - [$network.unauth](#networkunauth)
- Local variable
	- [setting](#set)
	- [getting](#get)
- Cache
	- [setting](#cacheset)
	- [getting](#cacheget)
- Util (widgets and convenience methods)
	- [$util.banner](#utilbanner)
	- [$util.toast](#utiltoast)
	- [$util.alert](#utilalert)
	- [$util.share](#utilshare)
	- [$util.picker](#utilpicker)
	- [$util.datepicker](#utildatepicker)
	- [$util.addressbook](#utiladdressbook)
- Media (video/photo)
	- [$media.camera](#mediacamera)
	- [$media.picker](#mediapicker)
	- [$media.play](#mediaplay)
- Audio
	- [$audio.play](#audioplay)
	- [$audio.pause](#audiopause)
	- [$audio.stop](#audiostop)
	- [$audio.seek](#audioseek)
	- [$audio.position](#audioposition)
	- [$audio.duration](#audioduration)
	- [$audio.record](#audiorecord)	
- Geolocation
	- [$geo.get](#geoget)
- Timer
	- [$timer.start](#timerstart)
	- [$timer.stop](#timerstop)
- Convert data to JSON
	- [$convert.csv](#convertcsv)
	- [$convert.rss](#convertrss)	

## View related
actions related to drawing views

### $reload

Refreshes the view completely by re-fetching content from the current URL.

<pre>{
  "items": [{
    "type": "label",
    "text": "Refresh",
    "action": {
      "type": "$reload"
    }
  }]
}</pre>

> This will reload the page when a user taps on the [item](document.md#items).

### $render

- Renders a [template](templates.md) with data

>####options
>1. data: Data to render. 
>	- **If not specified, it uses the `$jason` value at the time of rendering.**
>
>		<pre>{
>			"type": "$network.request",
>			"options": {
>				"url": "https://jasonclient.com/req.json"
>			},
>			"success": {
>				"type": "$render",
>				"options": {
>					"data": "{{$jason}}"
>				}
>			}
>		}</pre>
>
>		is same as
>
>		<pre>{
>			"type": "$network.request",
>			"options": {
>				"url": "https://jasonclient.com/req.json"
>			},
>			"success": {
>				"type": "$render"
>			}
>		}</pre>

>2. template: [Template](templates.md) name to render.
>	- **If not specified, it uses the `body` template**
>
>		<pre>{
>			"type": "$network.request",
>			"options": {
>				"url": "https://jasonclient.com/req.json"
>			},
>			"success": {
>				"type": "$render"
>			}
>		}</pre>
>		is same as
>		<pre>{
>			"type": "$network.request",
>			"options": {
>				"url": "https://jasonclient.com/req.json"
>			},
>			"success": {
>				"type": "$render",
>				"options": {
>					"template": "body"
>				}
>			}
>		}</pre>

>3. type: `"html"` | `"json"` (Default is `json`. See [html templating](templates.md#html) to learn more about how to render an HTML string natively using Jason)

>####note
>- In most cases, you call `$render` at the end of an [action call chain](https://github.com/Jasonette/documentation/blob/master/interaction.md#action).
>- Also, in most cases `$render` is used without `template` or `data` options.
>	- Simply having a `body` template under `templates` will make sure that template gets rendered whenever you call a `$render` action without a `template` argument.
>	- Also `$render` automatically passes the `$jason` return value it received from its parent action (The one that called `$render` as a `success` action) to the template, so you don't need to specify `data`.

>####How is `$render` different from `$reload`?
>`$render` redraws a template, using dynamic data. `$reload` completely refreshes the current URL. Here's an example scenario:
>
>1. Jason view loads and fetches the JSON markup from the server (`main.json`)
>2. `main.json` makes an API request to Twitter to fetch Tweets. (`https://api.twitter.com/1.1/statuses/user_timeline.json
`)
>3. Then it renders the result using its `body` template, which is included under `head > templates` in `main.json`.
>4. From this point on, calling `$render` simply takes the `body` template we have in memory, and renders updated data.
>5. However calling `$reload` will go back to step 1, completely refreshing everything.

### $snapshot

Takes a snapshot of the currently visible screen

>####options
> - none

>####return value
> - Returns the raw data under `data` attribute.
> - Also returns `data_uri` attribute, which contains the [data-uri](https://en.wikipedia.org/wiki/Data_URI_scheme)
>
>   <pre>{
>     "$jason": {
>       "data": "....",
>       "data_uri": "....",
>       "metadata": "....",
>       "content_type": "....",
>     }
>   }</pre>
>
> - You can utilize the `data` by passing it to another action using [success](interaction.md#user-content-action-syntax)

>####Example 1: Take a snapshot and upload
> [see here for example](#user-content-network-upload-example)

>####Example 2: Take a snapshot and share
>
>   <pre>{
>     "type": "$snapshot",
>     "success": {
>       "type": "$util.share",
>       "options": {
>         "items": [
>           {
>             "type": "image",
>             "data": "{{$jason.data}}"
>           }
>         ]
>       }
>     }
>   }</pre>
>
> Share sheet | SMS example
> ------------|------------
> ![snapshot share sheet example](images/util_share_sheet.jpeg) | ![snapshot sms example](images/util_share_sms.jpeg)

### $href

- An action version of [href](interaction.md#href). Works the same way, but just another way to invoke href.
- Sometimes used to invoke href on user taps without displaying a disclosure indicator.

>####options

> It's the same as [href](interaction.md#href) since it simply invokes the href when triggered.

> - url
> - view: `"web"` | `"app"` | `"jason"` (default)
> - transition: `"modal"` | `"fullscreen"` | `"push"` (default)


>####example
> <pre>{
>   "type": "label",
>   "text": "trigger href",
>   "action": {
>     "type": "$href",
>     "options": {
>       "url": "...",
>       "transition": "...",
>       "view": "..."
>     }
>   }
> }</pre>
>
> is same as:
>
> <pre>{
>   "type": "label",
>   "text": "trigger href",
>   "href": {
>     "url": "...",
>     "transition": "...",
>     "view": "..."
>   }
> }</pre>
>
> The only difference is: in case of [items](document.md#user-content-body-sections-items) the second option displays a disclosure indicator since we're directly using [href](interaction.md#href)

###$close

Close a modal (works when the currently view is a modal)

>####options
>
> - none

###$back

- Transition one step back
  - In case of a modal, it closes the current view
  - Otherwise it slides back to the previous view

>####options
>
> - none

## $network
Make GET/POST/PUT/DELETE network requests

### $network.request

>####options
>- url: The url to access.
>- method: `"get"` | `"post"` | `"put"` | `"delete"`.
>- data: Parameters to send along with the url (optional)
>- headers: Headers to attach to every request if any (optional)
>- data_type: Specifies how the fetched response will be processed. Can be `json`, `html`, `rss`, or `raw`. `json` assumes that the return value will be in JSON format, whereas `raw` expects a plain text. You can use `raw` type when fetching a plain text or CSV. `html` is for fetching HTML content and especially required when you need to utilize HTML requests associated with cookies/sessions. `rss` is used to fetch RSS. The default is `json`.
>- content_type: Specifies which format the parameters will be sent as. By default it's sent as a form object, but in case you specify `{"content_type": "json"}` the data will be submitted as a JSON string.

> Here's an example:
>	<pre>{
>		"type": "$network.request",
>		"options": {
>			<span style='color:#ff0000;'>"url": "http://www.jasonbase.com/messages.json",
>			"method": "post",
>			"data": {
>				"user_id": "fI9",
>				"message": "Hello there"
>			},
>			"headers": {
>				"auth_token": "fnekfla98dls9sNFK0nf3"
>			}</span>
>		},
>		"success": {
>			"type": "$render"
>		},
>		"error": {
>			"type": "$util.alert",
>			"options": {
>				"title": "Error",
>				"description": "Uh oh, something went wrong"
>			}
>		}
>}</pre>


> #### Example 1. JSON GET request
> 👇 Here's a simple example of GET request, fetching JSON. We don't need to specify `{"type": "get"}` here since the default type is `"get"`

><pre>{
>	"type": "$network.request",
>	"options": {
>		"url": "http://plasticfm.herokuapp.com/things/3.json"
>	},
>	"success": {
>		"type": "$render"
>	}
>}</pre>

> #### Example 2. HTML GET request
> 👇 Here's an HTML data_type request example. It fetches the url as `html` type, and then renders it using the `html` type parser.
>
><pre>{
>	"type": "$network.request",
>	"options": {
>		"url": "https://news.ycombinator.com/newest",
>		"dataType": "raw"
>	},
>	"success": {
>		"type": "$render",
>		"options": {
>			"type": "html"
>		}
>	}
>}</pre>


>#### Example 3. HTML POST request with cookies
>- Dealing with cookies is simple. All you need to do is make the request to create a session. Jason will automatically store the returned cookie, and then attach it to all subsequent requests.
>- Here's an example of signing into a website by making an HTML data_type `$network.request` call.  👇
> 	1. It first signs into the site by making a `POST` request of `{"data_type": "html"}`. Then it stores the cookie returned from the server.
> 	2. Once the login succeeds, it makes a `GET` request of `{"data_type": "html"}` to an actual content API. The cookie from the previous step is automatically applied to the request.
>
>    <pre>{
>      "type": "$network.request",
>      "options": {
>        "url": "https://news.ycombinator.com/login",
>        "method": "post",
>        "data": {
>          "acct": "{{$get.username}}",
>          "pw": "{{$get.password}}"
>        },
>        "data_type": "html"
>      },
>      "success": {
>        "type": "$network.request",
>        "options": {
>          "url": "https://news.ycombinator.com/saved?id={{$get.username}}",
>          "data_type": "html"
>        },
>        "success": {
>          "type": "$render",
>          "options": {
>            "type": "html"
>          }
>        }
>      }
>    }</pre>

### $network.upload

>####options
>- type: `s3` (Currently only s3 is supported)
>- bucket: s3 bucket
>- path: the s3 path to upload the file
>- sign_url:
>
>   To upload files to s3, you need to acquire a signed url from S3 first, and then upload it to that URL.
>
>   The `sign_url` attribute is the URL to your server which returns a signed url in the following format:
>
>   <pre>{
>     "$jason": "https://s3.amazonaws.com/...../...?AWSAccessKeyId=.....&Expires=.....&Signature=....."
>   }</pre>
>
><h4 id='network-upload-example'>example</h4>
>
> Here's an example of taking a photo and uploading to S3.
>
>   <pre>{
>     "type": "$media.camera",
>     "options": {
>       "quality": "0.4"
>     },
>     "success": {
>       "type": "$network.upload",
>       "options": {
>         "type": "s3",
>         "bucket": "fm.ethan.jason",
>         "data": "{{$jason.data}}",
>         "path": "",
>         "sign_url": "https://imagejason.herokuapp.com/sign_url"
>       },
>       "success": {
>         "type": "$network.request",
>         "options": {
>           "url": "https://imagejason.herokuapp.com/post",
>           "method": "post",
>           "data": {
>             "bucket": "fm.ethan.jason",
>             "path": "/",
>             "filename": "{{$jason.filename}}"
>           }
>         },
>         "success": {
>           "type": "$reload"
>         }
>       }
>     }
>   }</pre>
>
> Check out the full code here: [s3-upload-examle](https://github.com/Jasonette/s3-upload-example)
>
>####How it works
>
> 1. The client makes a request to `sign_url` (your server)
> 2. The server should return the corresponding response, mentioned above.
> 3. The client then uploads the content passed in as `data` to the just generated signed url, using a randomly generated filename.
> 4. Once the upload finishes, the `$network.upload` returns the filename generated from step 3.
> 5. You can take that filename and store it to your server by making another `$network.request`

### $network.auth
- `$network.auth` takes care of `token authentication`, which is used by most mobile APIs.
- For cookie based HTML authentication, see [Example 3 from `$network.request` above](#example-3-html-post-request-with-cookies).

>####options
>- url: The url for signing in.
>- method: `"get"` | `"post"` | `"put"` | `"delete"`.
>- data: Parameters to send along with the url (optional)
>- headers: Headers to attach to every request if any (optional)

> Here's an example:
>	<pre>{
>		"type": "$network.auth",
>		"options": {
>			<span style='color:#ff0000;'>"url": "http://www.jasonbase.com/messages.json",
>			"method": "post",
>			"data": {
>				"user_id": "fI9",
>				"message": "Hello there"
>			},
>			"headers": {
>				"auth_token": "fnekfla98dls9sNFK0nf3"
>			}</span>
>		},
>		"success": {
>			"type": "$render"
>		},
>		"error": {
>			"type": "$util.alert",
>			"options": {
>				"title": "Error",
>				"description": "Uh oh, something went wrong"
>			}
>		}
>}</pre>


>Similar to $network.request, except for a couple of things:
>
>	- Jason/Jasonette client expects the server to return a session object as a response to `$network.auth`, in the following format:

>	<pre>{
>		"$session": {
>			"headers": (SESSION TO APPEND TO EVERY REQUEST HEADER),
>			"body": (SESSION TO APPEND TO EVERY REQUEST BODY)
>		}
>	}</pre>

> 	Upon receiving this response from the server, the client will store the session object locally, and will automatically attach the session object to all subsequent `$network.request` actions.
> 
> 	Here's an example of what you need to return from the server as a response to `$network.auth`:

>	<pre>{
>		"$session": {
>			"headers": {
>				"username": "ethan",
>				"auth_token": "39fj3lsf9djfjs"
>			}
>		}
>	}</pre>

>	Once you do this, all subsequent `$network.request` actions to this domain will automatically have `{"username": "ethan", "auth_token": "39fj3lsf9djfjs"}` attached to their **header**, so that the server understands it's you.

### $network.unauth
Lets you clear sessions for specified domains. Can be used for both [token authentication](#tokenauthentication) and [web authentication via cookies](#example-3-html-post-request-with-cookies)

1. For APIs, it clears your session object.
2. For web requests (html), it clears your cookie.

>#### options
>- url: The url from which to sign out.
>- domain: The domain from which to sign out (used for APIs)
>- data: Parameters to send along with the url (optional)
>- type: `"html"` | `"json"` (Default is "json")


> #### Example 1. Signing out of token authentication
> No need to specify `type`, since it's `json` by default.
><pre>{
>	"type": "$network.unauth",
>	"options": {
>		"domain": "http://jasonclient.org"
>	},
>	"success": {
>		"type": "$reload"
>	}
>}</pre>

> #### Example 2. Signing out of a website by clearing cookies
>  Set `{"type": "html"}`
><pre>{
>	"type": "$network.unauth",
>	"options": {
>		"url": "http://news.ycombinator.com/newest",
>		<span style='color:#ff0000;'>"type": "html"</span>
>	},
>	"success": {
>		"type": "$reload"
>	}
>}</pre>



## Local variable
Use $set and $get to set and get local variables. Local variables are valid only within the current view and only stays on the memory.

### $set
Set local variables.

>#### options
>- key:value pairs. The key is the variable name, and the value is the variable's value.
>
>	In the following example, the `$set` action sets the value of the two local variables `firstname` and `lastname` as `ethan` and `gliechtenstein`, respectively.
>
>	<pre>{
>		"type": "$set",
>		"options": {
>			"firstname": "ethan",
>			"lastname": "gliechtenstein"
>		}
>	}</pre>

> This is how you set a variable. We are setting local variables `firstname` and `lastname` to `ethan` and `gliechtenstein` respectively.

>#### Note
>
>-  Setting the variable on its own won't update the view in realtime. Everytime you update a local variable and wish to update the view to reflect the update, you need to call `$render`. 
>- For example you can add a `success` attribute to above action to redraw the view after update, like this:

> <pre>{
>   "type": "label",
>   "text": "{{$get.firstname}} {{$get.lastname}}",
>   "action": {
>     "type": "$set",
>     "options": {
>       "firstname": "ethan",
>       "lastname": "gliechtenstein"
>     },
>     "success": {
>       "type": "$render"
>     }
>   }
> }</pre>

### $get
You can access the local variables you set by using a template expression `{{$get.VARIABLE_NAME}}`

>####Example
><pre>[
>  {
>    "type": "label",
>    "type": "{{$get.firstname}}"
>  },
>  {
>    "type": "label",
>    "type": "{{$get.lastname}}"
>  }
>]</pre>

>####Note
>- `$get` is NOT an action.
>- Instead, you directly access the local variable from `$get` object. When you're calling `$set` action to set local variables, you're actually setting the attributes of `$get` object. For example, to get the value of a local variable `username`, you simply refer to it as `{{$get.username}}`
>- Normally, the usage flow is:
>	1. Set a variable using `$set` action
>	2. Use the variable from templates by using the `{{$get.VARIABLE_NAME}}` expression.
>- Here's a full usage example using both `$set` action and `$get` expression.
>	1. When the view loads (`$load`), it sets the local variable `bar`'s value as "#", then renders the template with a label that displays the variable `bar` (`{{$get.bar}}`).
>	2. When the user makes a pull to refresh gesture (`$pull`), it appends another "#" to `bar` and then renders again.
>
>	<pre>{
>		"$jason": {
>			"head": {
>				"actions": {
>					"$load": {
>						"type": "$set",
>						"options": {
>							"bar": "#"
>						},
>						"success": {
>							"type": "$render"
>						}
>					},
>					"$pull": {
>						"type": "$set",
>						"options": {
>							"bar": "{{$get.bar+'#'}}"
>						},
>						"success": {
>							"type": "$render"
>						}
>					}
>				},
>				"templates": {
>					"body": {
>						"sections": [{
>							"items": [{
>								"type": "label",
>								"text": "{{$get.bar}}"
>							}]
>						}]
>					}
>				}
>			}
>		}
>	}</pre>


## $cache

Persist and retrieve content

- `$cache` is sandboxed per url. Therefore anything you store to `$cache` is stored just for that document.
- whereas local variables set by `$set` gets cleared out when the view disappears, `$cache` is persisted and can be reused next time.
- You can use cache to store network content after `$network.request`. This way you will be able to render directly from the cache next time.

<h3 id='cacheset'>1. Storing to $cache</h3>

`$cache.set` action is used to store any content.

>#### options
>- key:value pairs. The key is the cache variable name, and the value is the variable's value.
>
>	In the following example, it first makes a `$network.request`, and then takes its return value `{{$jason}}` and stores it to cache using the `$cache.set` action.
>
>	<pre>{
>		"type": "$network.request",
>		"options": {
>			"url": "http://jasonclient.org/api/items.json"
>		},
>		"success": {
>			"type": "$cache.set",
>			"options": {
>				"items": "{{$jason}}"
>			}
>		}
>	}</pre>


>####return value
>- returns the updated `$cache` object, which looks like this:
>
>	<pre>{
>		"$jason": {
>			"items": [...]
>		}
>	}</pre>


>	In the example below, the `{{$jason.items.length}}` is same as `{{$cache.items.length}}` since `$cache.set` returns the cache itself as `$jason`. 
>	<pre>{
>		"type": "$network.request",
>		"options": {
>			"url": "http://jasonclient.org/api/items.json"
>		},
>		"success": {
>			"type": "$cache.set",
>			"options": {
>				"items": "{{$jason}}"
>			},
>			"success": {
>				"type": "$util.alert",
>				"options": {
>					"title": "Items fetched",
>					"description": "{{$jason.items.length}}"
>				}
>			}
>		}
>	}</pre>


<h3 id='cacheget'>2. Retrieving from cache</h3>
Directly access `$cache` variable to access cache.

>####Note
>- `$cache.VARIABLE_NAME` is NOT an action (similar to how you retrive local variables using `$get`).
>- Instead, you access the `$cache` object directly. For example `{{$cache.items}}`


>####Example
>For example, you could store a `tracking_keyword` value locally and automatically perform a search whenever the document loads.
>
><pre>{
>  "type": "$oauth.request",
>  "options": {
>    "url": SEARCH_URL,
>    "data": {
>      "search_query": "{{$cache.tracking_keyword}}"
>    }
>  }
>}</pre>


## $util

### $util.banner

Displays a banner notification with title and description.

![$util.banner](images/util_banner.jpeg)

>####Options syntax
>- title
>- description
>- type: `"error"` | `"success"` | `"info"` (default)
>
>#### return value
> none
>
>#### Example
><pre>{
>	"type": "$util.banner",
>	<span style='color:black;'>"options": {
>		"title": "Hello World",
>		"description": "I'm a banner. I display a title and a description",
>		"type": "info"
>	}</span>
>}</pre>

### $util.toast

Displays a toast notification with a simple text

![$util.toast](images/util_toast.jpeg)

>####Options syntax
>- text: The text to display
>- type: `"error"` | `"info"` | `"warning"` | `"dark"` | `"default"` | `"success"` (default)
>
>#### return value
> none
>
>####Example
><pre>{
>	"type": "$util.toast",
>	"options": {
>		"text": "I'm a toast. I display a simple text.",
>		"type": "warning"
>	}
>}</pre>

### $util.alert
- Displays an alert.
- Alerts can also have form input fields users can fill in.
  - When you use the form input, `$util.alert` returns the resulting key/value pairs wrapped with `$jason`.

Basic Alert   |   Form Alert
--------------|-----------------------------
![$util.alert](images/util_alert_basic.jpeg) | ![$util.alert](images/util_alert_form.jpeg)

>####Options syntax
>- title: title of the alert
>- description: description caption
>- form (optional)
>	- name: name of the field. Use this name to retrieve the value filled out by the user
>	- value: set this attribute to preset the value inside the input field.
> - placeholder: placeholder text
>	- type (optional) : `secure` to hide keystrokes with *
>
>#### return value
> - if `form` attribute is used, returns the filled out `$jason` object
> - if `form` attribute is NOT used, no return value
>
>####Example 1
> Simple notice alert
><pre>{
>	"type": "$util.alert",
>	"options": {
>		"title": "Basic Alert",
>		"description": "I'm a basic alert. I simply display an alert that needs to be dismissed before moving forward"
>	},
>	"success": {
>		"type": "$render"
>	}
>}</pre>



>####Example 2
> Here's an example of an alert that lets users fill out a form and return the value.
><pre>{
>	"type": "$util.alert",
>	"options": {
>		"title": "Demo alert with input",
>		"description": "Try entering values and press OK",
>		"form": [{
>			"name": "username",
>     "placeholder": "Enter username"
>		}, {
>			"name": "password",
>     "placeholder": "Enter password"
>			"secure": "true"
>		}]
>	}
>}</pre>
>
>In this case, the alert finishes with a return value that takes the following form:

>```
>{
>	"$jason": {
>		"username": "ethan",
>		"password": "sdn3Uef2!"
>	}
>}
>```

> To use this return value, you can chain another action as a `success` callback and use the attributes, like this:
>
><pre>{
>  "type": "$util.alert",
>  "options": {
>    "title": "Sign in",
>    "description": "Please enter username and password",
>    <span style='color:#ff0000;'>"form": [{
>      "name": "username"
>    }, {
>      "type": "secure",
>      "name": "password"
>    }]</span>
>  },
>  "success": {
>    "type": "$network.request",
>    "options": {
>      "url": "https://www.jasonclient.org/users/sign_in.json",
>      "method": "post",
>      "data": {
>        <span style='color:#ff0000;'>"username": "{{$jason.username}}",
>        "password": "{{$jason.password}}"</span>
>      }
>    }
>  }
>}</pre>

### $util.share

Share a text, image, video, or combination of them.

> Share sheet | SMS example
> ------------|------------
> ![snapshot share sheet example](images/util_share_sheet.jpeg) | ![snapshot sms example](images/util_share_sms.jpeg)


>####Options syntax
>
> - items (array):
>   - type: `"text"` | `"image"` | `"video"`
>   - text: text
>   - data: raw data to be shared (only for `image` type)
>   - url: image url (only for `image` type)
>   - file_url: video url (only for `video` type)

>#### Example 1. Sharing simple text
>
> <pre>{
>   "type": "$util.share",
>   "options": {
>     "type": "text",
>     "text": "This is an automated message"
>   }
> }</pre>

>#### Example 2. Sharing an image captured from `$snapshot`
>
> <pre>{
>   "type": "$snapshot",
>   "success": {
>     "type": "$util.share",
>     "options": {
>       "items": [
>         {
>           "type": "image",
>           "data": "{{$jason.data}}"
>         }
>       ]
>     }
>   }
> }</pre>

>#### Example 3. Sharing a video captured from `$media.camera`
>
> <pre>{
>   "type": "$media.camera",
>   "success": {
>     "type": "$util.share",
>     "options": {
>       "items": [
>         {
>           "type": "video",
>           "file_url": "{{$jason.file_url}}"
>         }
>       ]
>     }
>   }
> }</pre>

>#### Example 4. Sharing an image from a URL, and a text
>
> <pre>{
>   "type": "$util.share",
>   "options": {
>     "items": [
>       {
>         "type": "image",
>         "url": "https://vjs.zencdn.net/v/oceans.png"
>       },
>       {
>         "type": "text",
>         "text": "This is a picture of ocean"
>       }
>     ]
>   }
> }</pre>

### $util.picker

Opens a multiple choice picker menu with each item linking to an [action](interaction.md#action) or an [href](interaction.md#href).

![$util.picker](images/util_picker.jpeg)

>####Options
> - items
> 		- text
> 		- href
> 		- action

>####Return value
>- none (Note that the picker itself doesn't have return values but its children actions can have their own return values

>####Example
><pre>{
>	"$jason": {
>		"head": {
>			...
>		},
>		"body": {
>			"header": {
>				"menu": {
>					"text": "Menu",
>					"action": {
>						<span style='color:#ff0000;'>"type": "$util.picker",</span>
>						"options": {
>							"items": [
>								{
>									"text": "Trigger $util.banner",
>									"action": {
>										"type": "$util.banner",
>										"options": {
>											"title": "Success",
>											"description": "This is a banner"
>										}
>									}
>								},
>								{
>									"text": "Trigger $util.alert",
>									"action": {
>										"type": "$util.alert",
>										"options": {
>                     "title": "Alert",
>                     "description": "This is an alert triggered by $util.picker"
>										}
>									}
>								},
>								{
>									"text": "Trigger $audio.play",
>									"action": {
>										"type": "$audio.play",
>										"options": {
>											"url": "https://s3.amazonaws.com/www.textcast.co/icons/yo.mp3"
>										}
>									}
>								}
>							]
>						}
>					}
>				}
>			}
>			...
>		}
>	}
>}</pre>

### $util.datepicker

Opens a date picker

![$util.datepicker](images/util_datepicker.jpeg)

>#### Options
>- None
>
>#### Return value
>- When a user selects one of the dates it returns the selected date in in unix timestamp format (in string) like this:
>
>	<pre>{
>		"$jason": "1471310358216"
>}	</pre>

>####Example
><pre>{
>  "$jason": {
>    "head": {
>      "title": "Datepicker Demo"
>    },
>    "body": {
>      "sections": [
>        {
>          "items": [
>            {
>              "type": "label",
>              "text": "Pick a date",
>              "action": {
>                <span style='color:#ff0000;'>"type": "$util.datepicker",</span>
>                "options": {
>                  "title": "Pick a date",
>                  "description": "Just pick one"
>                },
>                "success": {
>                  "type": "$util.alert",
>                  "options": {
>                    "title": "Selected date",
>                    "description": "{{(new Date(parseInt(<span style='color:#ff0000;'>$jason</span>) * 1000)).toString()}}"
>                  }
>                }
>              }
>            }
>          ]
>        }
>      ]
>    }
>  }
>}</pre>

### $util.addressbook

Fetches the addressbook to populate them into $jason.

>#### Options
>- None
>
>#### Return value
>- Returns an array of contacts from the addressbook, like this:
>
>	<pre>{
>		"$jason": [
>			{
>				"name": "John",
>				"phone": "9176568890",
>				"email": "john@jasonclient.org"
>			},
>			{
>				"name": "Mary",
>				"phone": "9172562890",
>				"email": "mary@jasonclient.org"
>			},
>			{
>				"name": "Ethan",
>				"phone": "2026468271",
>				"email": "ethan@jasonclient.org"
>			}
>		]
>}	</pre>

>####Example
>In this example, we access the addressbook when the view `$load`s, then `$render` the content using the given template.
><pre>{
>  "$jason": {
>    "head": {
>      "title": "Addressbook demo",
>      "actions": {
>        "$load": {
>          <span style='color:#ff0000;'>"type": "$util.addressbook",
>          "success": {
>            "type": "$render"
>          }</span>
>        }
>      },
>      "templates": {
>        "body": {
>          "sections": [
>            {
>              "items": {
>                "{{#each <span style='color:#ff0000;'>$jason</span>}}": {
>                  "type": "vertical",
>                  "style": {
>                    "padding": "5",
>                    "spacing": "5"
>                  },
>                  "components": [
>                    {
>                      "type": "label",
>                      "text": "{{name}}"
>                    },
>                    {
>                      "type": "label",
>                      "text": "{{JSON.stringify(phone)}}"
>                    },
>                    {
>                      "type": "label",
>                      "text": "{{JSON.stringify(email)}}"
>                    }
>                  ]
>                }
>              }
>            }
>          ]
>        }
>      }
>    }
>  }
>}</pre>

## $media

### $media.camera

Capture a video or a photo using the device camera

![$media.camera](images/media_camera.jpeg)

>#### Options
>
> - type: `"photo"` | `"video"`
> - quality: `"high"` | `"medium"` | `"low"` (Default is `"medium"`)
> - edit: `"true"` (Don't include to remove the editing step)

><h4 id='mediacamera-return-value'>Return value</h4>
>- Returns an object with multiple attributes which represent the video/photo
>
>>##### attributes
>> - `file_url`: local file url (Used for videos)
>> - `data_uri`: [data-uri string](https://en.wikipedia.org/wiki/Data_URI_scheme)
>> - `data`: raw data (Used for photos)
>> - `content_type`: `"image/png"` | `"image/jpeg"` | `"video/mp4"`

>>##### example return value
>>	<pre>{
>>   "$jason": {
>>     "file_url": "...",
>>     "data_uri": "data:image/png;base64,.....",
>>     "data": "...",
>>     "content_type": "image/png"
>>   }
>> }</pre>

>>- Normally you will want to pass the photo or video to another action such as [$network.upload](#networkupload).


>####Example
>- In the following example, we take a photo using `$media.camera`, and then utilize the `data_url` from the return value to set the `background` image url. 
>
>	<pre>{
>		"$jason": {
>			"head": {
>				"title": "Camera",
>				"description": "Tap to open up camera",
>				"actions": {
>					"$load": {
>						"type": "$media.camera",
>						"options": {
>							"edit": "true",
>							"type": "photo"
>						},
>						"success": {
>							"type": "$render"
>						}
>					}
>				},
>				"templates": {
>					"body": {
>						"style": {
>							"background": "{{$jason.data_url}}"
>						}
>					}
>				}
>			}
>		}
>	}</pre>

### $media.picker
Opens the device camera roll.

![media picker](images/media_picker.png)


>#### Options
>
> - type: `"photo"` | `"video"`
> - quality: `"high"` | `"medium"` | `"low"` (Default is `"medium"`)
> - edit: `"true"` (Don't include to remove the editing step)

>#### Return value
>Same as `$media.camera`. See [`$media.camera return values`](#user-content-mediacamera-return-value)

>#### Example
><pre>{
>	"$jason": {
>		"head": {
>			"title": "Media picker",
>			"description": "Tap to select media"
>		},
>		"body": {
>			"sections": [{
>				"items": [{
>					"type": "label",
>					"text": "Select media",
>					"action": {
>						"type": "$media.picker",
>						"options": {
>							"edit": "true",
>							"type": "video"
>						},
>						"success": {
>							"type": "$util.alert",
>							"options": {
>								"title": "Selected {{$jason['content_type']}} at",
>								"description": "{{$jason.file_url}}"
>							}
>						}
>					}
>				}]
>			}]
>		}
>	}
>}</pre>

### $media.play
Plays a video from remote url.

![$media.play](images/media_play.jpeg)

>#### options
>- url: the video url to play
>- muted: `"true"` to mute the sound

>#### Return value
>- none
 
>#### Example
><pre>{
>	"$jason": {
>		"head": {
>			"title": "Video",
>			"description": "Tap to play the video"
>		},
>		"body": {
>			"sections": [{
>				"items": [{
>					"type": "image",
>					"url": "https://vjs.zencdn.net/v/oceans.png",
>					"action": {
>						"type": "$media.play",
>						"options": {
>							"url": "https://vjs.zencdn.net/v/oceans.mp4"
>						}
>					}
>				}]
>			}]
>		}
>	}
>}</pre>



## $audio

### $audio.play

- Play audio from remote url. 
- Toggles between play and pause state if called multiple times.

> #### options
>- url:  A remote url to stream audio from.
>- title: Title to display on the lock screen while playing in background mode.
>- author: Author name to display on the lock screen while playing in background mode.
>- album: Album name to display on the lock screen while playing in background mode.
>- image: Image url to display on the lock screen while playing in background mode.

> #### return value
> - none

> #### Example
><pre>{
>	"$jason": {
>		"head": {
>			"title": "Play audio"
>		}, 
>		"body": {
>			"sections": [{
>				"items": [{
>					"type": "label",
>					"text": "Yo",
>					"action": {
>						"type": "$audio.play",
>						"options": {
>							"title": "Busdriver - Worlds to Run",
>							"author": "Song Exploder",
>							"image": "http://discover.pocketcasts.com/discover/images/400/fff9ba50-53e1-0131-8293-723c91aeae46.jpg",
>							"url": "http://www.podtrac.com/pts/redirect.mp3/traffic.libsyn.com/songexploder/SongExploder73-Busdriver.mp3"
>						}
>					}
>				}]
>			}]
>		}
>	}
>}</pre>

### $audio.pause

Pauses audio already playing from a remote url.

> #### options
> - url: if specified, pauses ONLY this url. Otherwise, pauses all audios currently playing.

> #### Return value
> - none

### $audio.stop

Stops audio already playing from a remote url.

> #### options
> - url: if specified, stops ONLY this url. Otherwise, stops all audios currently playing.

> #### Return value
> - none

### $audio.seek
- Seeks audio already playing from a remote url.
- The position value must be a value between `0` and `1` (wrapped in double quotes)

> #### options
> - url: The audio url.
> - position: value between 0 and 1 (Must be in string format)

> #### return value
> - none

> #### Example
> Here's an example of seeking an audio clip to 30% position
> 
><pre>{
>	"$jason": {
>		"head": {
>			"title": "Seek example"
>			"actions": {
>				"$load": {
>					"type": "$audio.play",
>					"options": {
>						"url": "http://www.podtrac.com/pts/redirect.mp3/traffic.libsyn.com/songexploder/SongExploder73-Busdriver.mp3"
>					}
>				}
>			}
>		},
>		"body": {
>			"layers": [{
>				"type": "label",
>				"text": "Go to 30% position",
>				"style": {
>					"bottom": "50",
>					"left": "50%-50",
>					"width": "100"
>				},
>				"action": {
>					"type": "$audio.seek",
>					"options": {
>						"url": "http://www.podtrac.com/pts/redirect.mp3/traffic.libsyn.com/songexploder/SongExploder73-Busdriver.mp3",
>						"position": "0.3"
>					}
>				}
>			}]
>		}
>	}
>}</pre>


### $audio.position
Get the position of the specified audio clip

>#### options
>- url: The audio url

>#### return value
>
> - Returns the position between 0 and 1, for example:
>
>	<pre>{
>		"$jason": "0.3"
>	}</pre>

>#### example
>
>- The following example displays a toast with the current position when the user taps the label.
>
>	<pre>{
>		"$jason": {
>			...
>			"body": {
>				"sections": [{
>					"items": [{
>						"type": "label",
>						"text": "How much did I listen so far?",
>						"action": {
>							"type": "$audio.position",
>							"options": {
>								"url": "http://www.podtrac.com/pts/redirect.mp3/traffic.libsyn.com/songexploder/SongExploder73-Busdriver.mp3"
>							},
>							"success": {
>								"type": "$util.toast",
>								"options": {
>									"text": "{{JSON.stringify($jason)}}"
>							}
>						}
>					}]
>				}]
>			}
>		}
>	}</pre>

Returns the current position (value between 0 and 1) while an audio clip is playing.

### $audio.duration
Returns total duration of the specified audio clip

>#### options
>- url: The audio url

>#### return value
>
> - Duration in seconds
>
>	<pre>{
>		"$jason": "300"
>	}</pre>

>#### example
>
>- The following example displays a toast with the total duration of the track when the user taps the label.
>
>	<pre>{
>		"$jason": {
>			...
>			"body": {
>				"sections": [{
>					"items": [{
>						"type": "label",
>						"text": "How long is this track?",
>						"action": {
>							"type": "$audio.duration",
>							"options": {
>								"url": "http://www.podtrac.com/pts/redirect.mp3/traffic.libsyn.com/songexploder/SongExploder73-Busdriver.mp3"
>							},
>							"success": {
>								"type": "$util.toast",
>								"options": {
>									"text": "{{JSON.stringify($jason)}}"
>							}
>						}
>					}]
>				}]
>			}
>		}
>	}</pre>


### $audio.record
> Record audio

> #### options
> - none

> #### return value
> - url: the local url in which the audio was stored
> - content-type: `"audio/m4a"` (it's always this format)

>Normally you will want to pass the result immediately to a `$network.upload` call in order to upload it to a cloud storage.

> ####Example
><pre>{
>  "$jason": {
>    "head": {
>      "title": "Play audio"
>    }, 
>    "body": {
>      "sections": [{
>        "items": [{
>          "type": "label",
>          "text": "Record Now",
>          "action": {
>            "type": "$audio.record",
>            "success": {
>              "type": "$util.alert",
>              "options": {
>                "title": "Audio stored at",
>                "description": "{{$jason.url}}"
>              }
>            }
>          }
>        }]
>      }]
>    }
>  }
>}</pre>

## $geo

### $geo.get

Get user's geolocation

>#### Options
> - none

> #### Return value
> - a `coord` object that contains `(latitude),(longitude)` format string
>
> <pre>{
> 	"$jason": {
> 		"coord": "12.342,22.343"
> 	}
> }</pre>

>#### Example
>- Below example demonstrates various ways of utilizing `$geo.get` return values
>
>	<pre>{
>	  "$jason": {
>	    "head": {
>	      "title": "Right Here",
> 	     "description": "Searching anything nearby, links to yelp, google streetview and foursquare"
>	    },
>	    "body": {
>	      "sections": [
>	        {
>	          "items": [
>	            {
>	              "type": "label",
>	              "text": "Street View",
>	              "style": {
>	                "size": "40",
>	                "font": "HelveticaNeue-CondensedBold",
>	                "color": "#000000"
>	              },
>	              "action": {
>	                "type": "$geo.get",
>	                "success": {
>	                  "type": "$href",
>	                  "options": {
>	                    "url": "http://maps.google.com/maps?q=&layer=c&cbll={{$jason.coord}}&cbp=11,0,0,0,0",
>	                    "view": "App"
>	                  }
>	                }
>	              }
>	            },
>	            {
>	              "type": "label",
>	              "text": "Yelp",
>	              "style": {
>	                "size": "40",
>	                "font": "HelveticaNeue-CondensedBold",
>	                "color": "#000000"
>	              },
>	              "action": {
>	                "type": "$geo.get",
>	                "success": {
>	                  "type": "$href",
>	                  "options": {
>	                    "url": "http://www.yelp.com/search?find_desc=food&cll={{$jason.coord}}&ns=1",
>	                    "view": "App"
>	                  }
>	                }
>	              }
>	            },
>	            {
>	              "type": "label",
>	              "text": "Foursquare",
>	              "style": {
>	                "size": "40",
>	                "font": "HelveticaNeue-CondensedBold",
>	                "color": "#000000"
>	              },
>	              "action": {
>	                "type": "$geo.get",
>	                "success": {
>	                  "type": "$href",
>	                  "options": {
>	                    "url": "https://foursquare.com/explore?ll={{$jason.coord}}&mode=url&q=Food",
>	                    "view": "App"
>	                  }
>	                }
>	              }
>	            }
>	          ]
>	        }
>	      ]
>	    }
>	  }
>	}</pre>

## $timer

### $timer.start
Start a timer

> ####options
> - interval: timer interval in seconds
> - name: name of the timer (used later to stop it)
> - repeats: if set to `"true"`, it's a perpetually repeating timer. Otherwise the timer gets called only once.
> - action: the [action](#action) to execute on every timer interval.

> #### return value
> - none

> #### example
><pre>{
>	"$jason": {
>		"head": {
>			"actions": {
>				"$load": {
>					"type": "$timer.start",
>					"options": {
>						"interval": "1",
>						"name": "timer1",
>						"repeats": "true",
>						"action": {
>							"type": "$render"
>						}
>					}
>				}
>			},
>			"templates": {
>				...
>			}
>		}
>	}
>}</pre>

### $timer.stop
Stops a timer

>####options
>- name: the name of the timer to stop. You need to have [started a timer with a name](#timerstart) first. It will stop all running timers if the name is not specified.

>####return value
>- none

><pre>{
>	"$jason": {
>		...
>		"body": {
>			"sections": [{
>				"items": [{
>					"type": "label",
>					"text": "Stop the timer",
>					"action": {
>						"type": "$timer.stop",
>						"options": {
>							"name": "timer1"
>						}
>					}
>				}]
>			}]
>		}
>	}
>}</pre>

## $convert
Convert other data formats into JSON format

### $convert.csv
Convert CSV to JSON

>####Options
>- data: CSV string

>####return value
>- returns the parsed JSON result.
>- **Expected format:** must have the first row populated with attribute names. (See below for an example)

>####example
>- Here's an example CSV string returned from a network request.
>- Notice how the first line is entirely made up of attribute names, and the rest rows are the actual data.
>
>	<pre>name, descrption, url, icon
>	github, social coding, https://www.github.com, https://assets-cdn.github.com/images/modules/logos_page/GitHub-Mark.png
>	facebook, Best place to build & make an impact., https://www.facebook.com, https://www.facebook.com/images/fb_icon_325x325.png
>	product hunt, Discover your next favorite thing, https://www.producthunt.com, https://pbs.twimg.com/profile_images/699572900643213312/RC2oRewL.jpg</pre>

>- We will try to parse this CSV into JSON by using the following code:
>
>	<pre>{
>		"$jason": {
>			"head": {
>				"actions": {
>					"$load": {
>						"type": "$network.request",
>						"options": {
>							"url": "http://hastebin.com/raw/xiceheroku",
>							"data_type": "raw"
>						},
>						"success": {
>							"type": "$convert.csv",
>							"options": {
>								"data": "{{$jason}}"
>							},
>							"success": {
>								"type": "$render"
>							}
>						}
>					}
>				}
>			}
>		}
>	}</pre>

>- The end result:

>	<pre>[
>		{
>			"name": "github",
>			"description": "social coding",
>			"url": "https://www.github.com",
>			"icon": "https://assets-cdn.github.com/images/modules/logos_page/GitHub-Mark.png"
>		},
>		{
>			"name": "facebook",
>			"description": "Best place to build & make an impact.",
>			"url": "https://www.facebook.com",
>			"icon": "https://www.facebook.com/images/fb_icon_325x325.png"
>		},
>		{
>			"name": "product hunt",
>			"description": "Discover your next favorite thing",
>			"url": "https://www.producthunt.com",
>			"icon": "https://pbs.twimg.com/profile_images/699572900643213312/RC2oRewL.jpg"
>		}
>	]</pre>

### $convert.rss
Convert RSS to JSON. Built on top of [node-feedparser library](https://github.com/danmactough/node-feedparser)

>####options
>- data: RSS string
>
>	<pre>{
>		"actions": {
>			"$load": {
>				"type": "$network.request",
>				"options": {
>					"url": "http://feeds.gawker.com/lifehacker/full",
>					"data_type": "rss"
>				},
>				"success": {
>					"type": "$convert.rss",
>					"options": {
>						"data": "{{$jason}}"
>					},
>					"success": {
>						"type": "$render"
>					}
>				}
>			}
>		}
>	}</pre>

>####return value
>- returns the parsed JSON result in the following format ([You can learn more about the spec here](https://github.com/danmactough/node-feedparser#what-is-the-parsed-output-produced-by-feedparser)):
>
><pre>
>[
>  {
>    "author": "Alan Henry",
>    "rss:pubdate": {
>      "@": {},
>      "#": "Thu, 9 Jun 2016 00:00:00 GMT"
>    },
>    "source": {},
>    "guid": "1780470292",
>    "link": "http://feeds.gawker.com/~r/lifehacker/full/~3/kcJMaJ6Ad7I/the-edge-of-the-world-desktop-1780470292",
>    "title": "The Edge of the World Desktop",
>    "summary": "&lt;p class=\"first-text\"&gt;This OS X desktop is simple, elegant, and combines a few simple widgets with a gorgeous wallpaper to great effect. If you like what you see, here’s how you can set it up, customize your own menubar, and give your Mac the same look. &lt;/p&gt;",
>    "image": {},
>    "rss:category": [
>      {
>        "@": {
>          "domain": ""
>        },
>        "#": "featured desktop"
>      },
>      {
>        "@": {
>          "domain": ""
>        },
>        "#": "desktops"
>      },
>      {
>        "@": {
>          "domain": ""
>        },
>        "#": "wallpapers"
>      },
>      {
>        "@": {
>          "domain": ""
>        },
>        "#": "customization"
>      },
>      {
>        "@": {
>          "domain": ""
>        },
>        "#": "personalization"
>      },
>      {
>        "@": {
>          "domain": ""
>        },
>        "#": "hud"
>      },
>      {
>        "@": {
>          "domain": ""
>        },
>        "#": "rainmeter"
>      },
>      {
>        "@": {
>          "domain": ""
>        },
>        "#": "themes"
>      },
>      {
>        "@": {
>          "domain": ""
>        },
>        "#": "skins"
>      },
>      {
>        "@": {
>          "domain": ""
>        },
>        "#": "windows"
>      },
>      {
>        "@": {
>          "domain": ""
>        },
>        "#": "os x"
>      },
>      {
>        "@": {
>          "domain": ""
>        },
>        "#": "mac"
>      },
>      {
>        "@": {
>          "domain": ""
>        },
>        "#": "linux"
>      }
>    ],
>    "rss:link": {
>      "@": {},
>      "#": "http://feeds.gawker.com/~r/lifehacker/full/~3/kcJMaJ6Ad7I/the-edge-of-the-world-desktop-1780470292"
>    },
>    "feedburner:origlink": {
>      "@": {},
>      "#": "http://lifehacker.com/the-edge-of-the-world-desktop-1780470292"
>    },
>    "enclosures": [],
>    "origlink": "http://lifehacker.com/the-edge-of-the-world-desktop-1780470292",
>    "pubDate": "2016-06-09T00:00:00.000Z",
>    "pubdate": "2016-06-09T00:00:00.000Z",
>    "rss:guid": {
>      "@": {
>        "ispermalink": "false"
>      },
>      "#": "1780470292"
>    },
>    "date": "2016-06-09T00:00:00.000Z",
>    "rss:title": {
>      "@": {},
>      "#": "The Edge of the World Desktop"
>    },
>    "meta": {
>      "#ns": [
>        {
>          "xmlns:itunes": "http://www.itunes.com/dtds/podcast-1.0.dtd"
>        },
>        {
>          "xmlns:dc": "http://purl.org/dc/elements/1.1/"
>        },
>        {
>          "xmlns:taxo": "http://purl.org/rss/1.0/modules/taxonomy/"
>        },
>        {
>          "xmlns:rdf": "http://www.w3.org/1999/02/22-rdf-syntax-ns#"
>        },
>        {
>          "xmlns:atom": "http://www.w3.org/2005/Atom"
>        },
>        {
>          "xmlns:wfw": "http://wellformedweb.org/CommentAPI/"
>        },
>        {
>          "xmlns:feedburner": "http://rssnamespace.org/feedburner/ext/1.0"
>        },
>        {
>          "xmlns:atom10": "http://www.w3.org/2005/Atom"
>        },
>        {
>          "xmlns:atom10": "http://www.w3.org/2005/Atom"
>        }
>      ],
>      "#version": "2.0",
>      "categories": [],
>      "language": "en",
>      "link": "http://lifehacker.com",
>      "title": "Lifehacker",
>      "rss:link": {
>        "@": {},
>        "#": "http://lifehacker.com"
>      },
>      "cloud": {
>        "type": "hub",
>        "href": "http://pubsubhubbub.appspot.com/"
>      },
>      "image": {},
>      "xmlurl": "http://www.lifehacker.com/index.xml",
>      "feedburner:info": {
>        "@": {
>          "uri": "lifehacker/full"
>        }
>      },
>      "feedburner:browserfriendly": {
>        "@": {},
>        "#": "This is an XML content feed. It is intended to be viewed in a newsreader or syndicated to another site."
>      },
>      "#xml": {
>        "version": "1.0",
>        "encoding": "UTF-8"
>      },
>      "@": [
>        {
>          "xmlns:itunes": "http://www.itunes.com/dtds/podcast-1.0.dtd"
>        },
>        {
>          "xmlns:dc": "http://purl.org/dc/elements/1.1/"
>        },
>        {
>          "xmlns:taxo": "http://purl.org/rss/1.0/modules/taxonomy/"
>        },
>        {
>          "xmlns:rdf": "http://www.w3.org/1999/02/22-rdf-syntax-ns#"
>        },
>        {
>          "xmlns:atom": "http://www.w3.org/2005/Atom"
>        },
>        {
>          "xmlns:wfw": "http://wellformedweb.org/CommentAPI/"
>        },
>        {
>          "xmlns:feedburner": "http://rssnamespace.org/feedburner/ext/1.0"
>        }
>      ],
>      "pubDate": null,
>      "pubdate": null,
>      "rss:language": {
>        "@": {},
>        "#": "en"
>      },
>      "date": null,
>      "generator": null,
>      "rss:title": {
>        "@": {},
>        "#": "Lifehacker"
>      },
>      "xmlUrl": "http://www.lifehacker.com/index.xml",
>      "favicon": null,
>      "rss:@": {},
>      "atom10:link": [
>        {
>          "@": {
>            "rel": "hub",
>            "xmlns:atom10": "http://www.w3.org/2005/Atom",
>            "href": "http://pubsubhubbub.appspot.com/"
>          }
>        },
>        {
>          "@": {
>            "href": "http://www.lifehacker.com/index.xml",
>            "rel": "self",
>            "xmlns:atom10": "http://www.w3.org/2005/Atom",
>            "type": "application/rss+xml"
>          }
>        }
>      ],
>      "rss:description": {
>        "@": {},
>        "#": "Tips and downloads for getting things done"
>      },
>      "copyright": null,
>      "#type": "rss",
>      "author": null,
>      "description": "Tips and downloads for getting things done"
>    },
>    "dc:creator": {
>      "@": {},
>      "#": "Alan Henry"
>    },
>    "rss:@": {},
>    "rss:description": {
>      "@": {},
>      "#": "&lt;p class=\"first-text\"&gt;This OS X desktop is simple, elegant, and combines a few simple widgets with a gorgeous wallpaper to great effect. If you like what you see, here’s how you can set it up, customize your own menubar, and give your Mac the same look.&lt;/p&gt;"
>    },
>    "comments": null,
>    "categories": [
>      "featured desktop",
>      "desktops",
>      "wallpapers",
>      "customization",
>      "personalization",
>      "hud",
>      "rainmeter",
>      "themes",
>      "skins",
>      "windows",
>      "os x",
>      "mac",
>      "linux"
>    ],
>    "description": "&lt;p class=\"first-text\"&gt;This OS X desktop is simple, elegant, and combines a few simple widgets with a gorgeous wallpaper to great effect. If you like what you see, here’s how you can set it up, customize your own menubar, and give your Mac the same look.&lt;/p&gt;"
>  },
>  {
>    ...
>  }
>]</pre>

>####Example
[You can check out a full RSS reader example here](http://www.jasonbase.com/things/2dL/edit)
