# About the project
Jasonette is a different way of making native apps.

Instead of programming an app on the device, you simply write a JSON file hosted on a server, and the Jasonette apps fetch and use it to build themselves on-demand whenever you open the app.

---

Before we jump in, <b>here's where you can stay in touch with the project community:</b>

<br>

##■ Follow the project

<table class='equalwidth follow'>
	<tr>
		<th class='span4'>Twitter</th>
		<th class='span4'>Blog</th>
	</tr>
	<tr>
		<td class='span4'><a href='https://www.twitter.com/jasonclient'><h1><i class='icon fa-twitter fa-1x'></i></h1>@jasonclient</a></td>
		<td class='span4'><a href='https://medium.com/@gliechtenstein'><h1><i class='icon fa-rss fa-1x'></i></h1>Follow on Medium</a></td>
	</tr>
</table>
<table class='equalwidth follow'>
  <tr>
		<th class='span4'>Github iOS</th>
		<th class='span4'>Github Android</th>
		<th class='span4'>Github Web</th>
  </tr>
  <tr>
    <td class='span4'>Native Objective-C<br>Implementation</td>
    <td class='span4'>Native Java<br>Implementation</td>
    <td class='span4'>A Web implementation,<br>written in JavaScript</td>
  </tr>
	<tr>
		<td class='span4'><a href='https://www.github.com/Jasonette/JASONETTE-iOS'><h1><i class='icon fa-github fa-1x'></i></h1>JASONETTE-iOS</a></td>
		<td class='span4'><a href='https://www.github.com/Jasonette/JASONETTE-Android'><h1><i class='icon fa-github fa-1x'></i></h1>JASONETTE-Android</a></td>
		<td class='span4'><a href='https://www.github.com/Jasonette/JASONETTE-Web'><h1><i class='icon fa-github fa-1x'></i></h1>JASONETTE-Web</a></td>
	</tr>
</table>

<div class='group'>
	<form action="//textethan.us8.list-manage.com/subscribe/post?u=54e23b3fe61843c19c384fc05&amp;id=76d5746d7b" method="post" id="mc-embedded-subscribe-form" name="mc-embedded-subscribe-form" class="validate" target="_blank" novalidate>
		<h3>subscribe to newsletter</h3>
		<p>Comprehensive newsletter on Important updates, milestones, new projects, and useful tips from the community.</p>
		<input type="email" value="" name="EMAIL" class="required email" id="mce-EMAIL" placeholder="enter email">
		<button type="submit" value="Subscribe" name="subscribe" id="mc-embedded-subscribe" class="button">Subscribe</button>
		<div id="mce-responses" class="clear">
			<div class="response" id="mce-error-response" style="display:none"></div>
			<div class="response" id="mce-success-response" style="display:none"></div>
		</div>
		<div style="position: absolute; left: -5000px;" aria-hidden="true">
			<input type="text" name="b_54e23b3fe61843c19c384fc05_76d5746d7b" tabindex="-1" value="">
		</div>
	</form>
	<script type='text/javascript' src='//s3.amazonaws.com/downloads.mailchimp.com/js/mc-validate.js'></script><script type='text/javascript'>(function($) {window.fnames = new Array(); window.ftypes = new Array();fnames[0]='EMAIL';ftypes[0]='email';fnames[1]='FNAME';ftypes[1]='text';fnames[2]='LNAME';ftypes[2]='text';}(jQuery));var $mcj = jQuery.noConflict(true);</script>
</div>

<br>

##■ Ask the community

For code related questions, ask on the forum to get more comprehensive answers.

For quick chats, use the Slack channel.

<br>

<table class='equalwidth follow'>
	<tr>
		<th class='span6'>Slack</th>
		<th class='span6'>Forum</th>
	</tr>
	<tr>
		<td class='span6'>
			<a href='https://jasonette.now.sh'><img src='images/slack.png'></a>
			<p><script async defer src="https://jasonette.now.sh/slackin.js"></script></p>
		</td>
		<td class='span6'>
			<a href='https://forum.jasonette.com'>
				<img src='images/discourse.png'>
				<h5>Ask questions and share tips.</h5>
				<p>https://forum.jasonette.com</p>
			</a>
		</td>
	</tr>
</table>

<br>

---

<br>

#Quickstart


  1.  Download Jasonette for iOS or Android

  2.  Write and host a JSON recipe that defines your app

  3.  Add the JSON URL to Jasonette to turn it into your app 

<br>


<br>

##Step 1. Download

Go download and come back, we'll wait.
<br><br>

<table class='equalwidth follow'>
	<tr>
		<th class='span6'>iOS</th>
		<th class='span6'>Android</th>
	</tr>
	<tr>
		<td class='span6'>
      <a href="ios">
        <i class='icon fa-apple fa-5x'></i>
        <br><br>
        iOS Setup
      </a>
		</td>
		<td class='span6'>
      <a href="android">
        <i class='icon fa-android fa-5x'></i>
        <br><br>
        Android Setup
      </a>
		</td>
	</tr>
</table>

<br>

---

<br>

##Step 2. Learn

Watch the 2 videos below and you'll have learned everything you need to know to get started.

<b>The videos were shot using an iPhone, but it works exactly the same for Android.</b>

<br>

### A. Do you know JSON?


Before we dive in, do you know JSON? If not, just check out [this tutorial](http://www.w3schools.com/js/js_json_syntax.asp), takes 2 minutes.

<br><br>

### B. Get a JSON server

You'll be serving your entire app from a server, so you will need somewhere to host JSON, just like you need somewhere to host websites. There are many ways to do this:

<div class='well'>
<h3><a href='http://web.jasonette.com'>1. Jasonette Web (web.jasonette.com)</a></h3>
<b>[Recommended]</b> A hosted & social implementation of <a href='https://github.com/Jasonette/Jasonette-Web'>Jasonette-Web</a>.<br><br>Has all the useful features such as folders, markdown-based readme, bookmarking, version control, realtime editing, etc. Write and share as much JSON as you want. <b>It's FREE.</b>
<br><br>
(Note: You DON'T need to use Jasonette Web to use Jasonette. This service is provided for free to help you get started quickly without having to set up your own JSON server. After all, all you need is JSON and the whole point of the framework is the portability of JSON)

<br><br>
<h3>2. Code hosting or pastebin sites</h3>
You can also use <a href='https://www.github.com'>Github</a> or <a href='http://pastebin.com'>Pastebin</a>. Not really recommended for development because these sites are <b>NOT built for this type of usage</b>. They actually discourage you from using them as API endpoint. Furthermore, their content is cached so you'll often keep getting old responses whenever you update your JSON content, which is a pain. <b>However, you can use them AFTER you've finished editing, or use it for open sourcing though. <a href='https://github.com/Jasonette/Instagram-UI-example'>Here's an example.</a></b>

<br><br>

<h3>3. Quick Local Prototyping</h3>
You can also use <a href='https://github.com/Jasonette/Jasonette-Web'>Jasonette-Web</a> along with simple instant open source HTTP servers like the <a href='https://github.com/indexzero/http-server'>http-server project</a> to roll your own Jasonette Web.
<br><br>

<h3>4. Plug into your existing server</h3>
You can skip all this and set up your own web app with a JSON endpoint, or just plug into your existing web app.
<br><br>


</div>
<br><br>

### C. Learn the basics
This video walks you through the basics of Jasonette, such as how it works, how to get started, etc. It was shot using iOS but it basically works the same for Android.
<br><br>
<div class='video-container'>
<iframe width="640" height="360" src="https://www.youtube.com/embed/hfevBAAfCMQ?rel=0" frameborder="0" allowfullscreen></iframe>
</div>

<br><br>

### D. Learn JASON syntax
This video teaches you how to actually write a JSON markup to build sophisticated interactive layouts.
<br><br>
<div class='video-container'>
<iframe width="640" height="360" src="https://www.youtube.com/embed/S7yGejKIH6Q?rel=0" frameborder="0" allowfullscreen></iframe>
</div>

<br>

---

<br>

##Step 3. Learn more (Reference)

<br>

###Anatomy of a Jason view
**[Learn the basic structure of a JASON view](document.md)**<br>
Just like HTML has basic tags such as body, div, span, li, etc, Jasonette has JSON based tags to describe the structure of a view.

<br><br>

###Components
**[Learn component syntax](components.md)**<br>
Components are the most basic units of user interface, such as image, label, textarea, button, slider, etc. 
<br><br>

###Layout
**[Learn layouts](layout.md)**<br>
In many cases we combine multiple components to construct a unit. We use layouts to do this.

<br><br>

###Link multiple views
**[Learn linking](href.md)**<br>
Above three sections are all you need to know to display content in a view. But what if we want multiple views? We can link them using `href`.

<br><br>

###Actions
**[Learn actions](actions.md)**<br>
Actions define a task or a sequence of tasks you wish to run, such as network request, audio play, camera access, geolocation, displaying banners, etc.

<br><br>

###Templates
**[Learn templates](templates.md)**<br>
You can use templates to dynamically render data, such as remote network content, local data, and user input.

---

# In-depth Tutorials

Here are some in-depth articles

- [(Intro) How to build cross-platform mobile apps using nothing more than a JSON markup](https://medium.freecodecamp.org/how-to-build-cross-platform-mobile-apps-using-nothing-more-than-a-json-markup-f493abec1873)
- [Jasonette Offline: Using offline features with Jasonette](https://medium.com/@gliechtenstein/jasonette-offline-7d6ed8d58edb)
- [Building a SlackBot remote control app with Jasonette](http://blog.jasonette.com/2017/01/17/build-a-slackbot-with-jasonette/)
- [TabBar: How the bottom tab bar works](http://blog.jasonette.com/2017/02/07/supercharged-tabbar/)
- [Functional Programming in JSON: How Jasonette implements function call stacks purely in JSON, and how to use it](http://blog.jasonette.com/2017/02/15/functional-programming-in-json/)
- [Require: How $require action works](http://blog.jasonette.com/2017/02/17/require/)
- Mixins: 2-part series.
    - [Remote Mixin](http://blog.jasonette.com/2017/02/27/mixins/): How to dynamically mix in multiple remote JSON objects into a single root JSON
    - [Self Mixin](http://blog.jasonette.com/2017/03/02/self-mixin/): How to mix in a subtree of a JSON object onto itself.
- [Web Container: Introduction to Web Container](http://blog.jasonette.com/2017/04/07/JSON-web-container/)

---

# Examples
Actual JSON examples you can try out with Jasonette:

**[Try examples here](examples.md)**

