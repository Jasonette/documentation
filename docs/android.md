# Download

<div class='well'>
Jasonette itself is a pre-built app.<br>All you need to do is download Jasonette and build with <a href='https://developer.android.com/studio/index.html'>Android Studio</a>.
<br><br>
<a href='https://github.com/Jasonette/JASONETTE-android/archive/master.zip' class='btn'><i class='fa fa-android'></i> Download Jasonette-android</a>
</div>


---

# Quick Start
Ready? Let's get your first Jasonette app on your phone, in 20 seconds!

<br>

##Step 1. DOWNLOAD AND UNZIP

<img src='/images/android_1.png' class='large'>

<br>

##Step 2. LAUNCH ANDROID STUDIO

Now launch [Android Studio](https://developer.android.com/studio/index.html), and select "Import project"

<img src='/images/android_2.png' class='large'>

##Step 3. IMPORT THE PROJECT

Find the Jasonette folder you just unzipped and press OK.

<img src='/images/android_3.png' class='large'>

##Step 4. PRESS PLAY

Jasonette comes with a default demo JSON url embedded. Let's try running it to make sure it works.

<img src='/images/android_4.png' class='large'>


##Step 5. CONNECT THE PHONE AND RUN

It will show up a dialog that looks like this.

Here you can either choose to run it on an emulator or a real device.

**To learn how to connect the phone, or how to run it on an emulator, [read this](https://developer.android.com/training/basics/firstapp/running-app.html).**

<img src='/images/android_7.png' class='large'>

##Step 6. CUSTOMIZE!

Now let's try changing the JSON url so you can turn it into your own app.

First, click the "Project" tab on the left side.

Then find `app` > `java` > `com.jasonette.seed` > `JasonSettings`.

Update the return value for the url to your own JSON url. That's it!

If you don't have a JSON yet, [here are some example apps you can try quickly](/examples)

<img src='/images/android_5.png' class='large'>


---

## ★ Did it work?

<br>

  - ###YES?
    - Congratulations! You're ready to transform this into your OWN app! Go on to the next section.

<br>

  - ###NO?
    - **[Check troubleshoot section](#troubleshoot)**


---

# Tutorial Screencast

Watch the 2 videos below and you'll have learned everything you need to know to get started.

**The videos were shot using an iPhone, but it works exactly the same for Android.**

<br>

### A. Do you know JSON?


Before we dive in, do you know JSON? If not, just check out [this tutorial](http://www.w3schools.com/js/js_json_syntax.asp), takes 2 minutes.

<br><br>

### B. Learn the basics
This video walks you through the basics of Jasonette, such as how it works, how to get started, etc.
<br><br>
<div class='video-container'>
<iframe width="640" height="360" src="https://www.youtube.com/embed/hfevBAAfCMQ?rel=0" frameborder="0" allowfullscreen></iframe>
</div>

<br><br>

### C. Learn JASON syntax
This video teaches you how to actually write a JSON markup to build sophisticated interactive layouts.
<br><br>
<div class='video-container'>
<iframe width="640" height="360" src="https://www.youtube.com/embed/S7yGejKIH6Q?rel=0" frameborder="0" allowfullscreen></iframe>
</div>


---


# Documentation

###Anatomy of a Jason view
**[Learn the basic structure of a JASON view](document.md)**<br>
Just like HTML has basic tags such as body, div, span, li, etc, Jasonette has JSON based tags to describe the structure of a view.
---
###Components
**[Learn component syntax](components.md)**<br>
Components are the most basic units of user interface, such as image, label, textarea, button, slider, etc. 
---
###Layout
**[Learn layouts](layout.md)**<br>
In many cases we combine multiple components to construct a unit. We use layouts to do this.
---
###Link multiple views
**[Learn linking](href.md)**<br>
Above three sections are all you need to know to display content in a view. But what if we want multiple views? We can link them using `href`.
---
###Actions
**[Learn actions](actions.md)**<br>
Actions define a task or a sequence of tasks you wish to run, such as network request, audio play, camera access, geolocation, displaying banners, etc.
---
###Templates
**[Learn templates](templates.md)**<br>
You can use templates to dynamically render data, such as remote network content, local data, and user input.
---
# Examples
Actual JSON examples you can try out with Jasonette:

**[Try examples here](examples.md)**


---

# Submitting to the play store

[Check out this documentation](https://support.google.com/googleplay/android-developer/answer/113469?hl=en) on how to submit to the play store.

---

# Troubleshoot

## Need more help?

  - **Slack - **  Come ask quick questions and share tips with other Jasonette users. [Join here](https://jasonette.herokuapp.com)

	<script async defer src="https://jasonette.herokuapp.com/slackin.js?large"></script>

  - **Forum - **  Chat messages on Slack tend to flow away, so you may want to ask questions on the forum. Also it's good for future users who may have the same problem. All messages on the forum will be read. Visit here: [https://forum.jasonette.com](https://forum.jasonette.com)

