# App Portability

With Jasonette, the apps become completely portable because the entire logic can be expressed as JSON, which can be sent to, stored, and received from anywhere.

<br>

Since portability is one of Jasonette's main benefits, we are developing many different ways of storing, transmitting, and receiving JSON, and planning on supporting many useful protocols.

<br>

One of those efforts is local JSON.

<br>

# Local

There are currently several ways Jasonette supports local JSON, each of which is unique in its own way.

---

1. [Data-uri](#1-data-uri)
2. [Store JSON locally on the app](#2-file-url)
3. [Offline caching](#3-offline-caching)
4. [Global local storage](#4-global-local-storage)
5. [Local Image](#5-local-image)

---

## 1. Data-uri

Jasonette supports [data-uri](https://en.wikipedia.org/wiki/Data_URI_scheme). This means you can store the entire content within the URL itself. It's not always practical but there are several cases where you can get neat effects using this approach.

Try entering a data-url instead of http or https based url. It should work.

---

## 2. File URL

Another URL scheme Jasonette supports is local file scheme (`file://`).

Instead of using `http` or `https`, you can refer directly to your local file bundled up with the app. There are currently three ways to use the file URL scheme:

<br>

### Using File URL Scheme

<br>

> **A. Home URL **
>
> Instead of setting a remote URL you can point it to a local JSON file.

<br>

> Here's what it looks like on Android:
>
>  <br>
>
><img src='../images/android_home_local_url.png' class='large'>

<br>

> Here's what it looks like on iOS:

><img src='../images/ios_home_local_url.png' class='large'>

---

> **B. HREF**

>You can also `href` into local JSON URLs:
```
    {
      "type": "label",
      "text": "Go",
      "href": {
        "url": "file://demo.json"
      }
    }
```

---

> **C. Local Images**
>
> You can also use the `file://` scheme to [render local images](#5-local-image).

---

<br>

### Storing files locally

To utilize this feature, you first need to store files under the right folders. Let me show you how:

<br>

**iOS File URL**

1. Open XCode, go to `Jasonette > Core > file://` from the sidebar, and add your files there by drag and dropping.
2. Access using `file://your_filename.json`

<br>

<img src='../images/ios_local_file.png' class='large'>

<br>

** Android File URL**

1. Open Android Studio, go to `app > assets > file`. Copy and paste your files there.
2. Access using `file://your_filename.json`

<br>

<img src='../images/android_local_file.png' class='large'>

<br>

**(Disclaimer: The file url scheme is currently limited to the home URL and `href`s. It doesn't include mixins, requires, network.request. If you would like to add these features to Jasonette, feel free to send a pull request and contribute!)**

---

## 3. Offline Caching

You can have best of both worlds (Stream the app on demand to keep it up-to-date all the time, as well as have the app logic cached locally so it loads instantly) by using the offline caching feature.

<br>

### How offline caching works

1. The first time the app loads, Jasonette fetches the JSON from your server.

2. If you specify that you want to use offline caching, Jasonette will cache the entire JSON for that view.

3. Next time you open the view, Jasonette will load immediately from the offline cached version.

4. But it doesn't stop there, Jaasonette checks to see if the network is available, and if it is, it re-fetches the JSON and updates the view. The trick is **step 3** comes first, so it will ONLY update if the network is available. Otherwise you'll still have your offline cached version of your app.

<br>

### How to use

Offline caching is managed on a per-view basis. All you need to do to enable is put `"offline": "true"` under `$jason.head`, like this:

    {
      "$jason": {
        "head": {
          "title": "offline test",
          "offline": "true",
          ...
    }

---

## 4. Global Local Storage

In many cases you want to persist content across sessions. You can of course store them on a remote server using [$network.request](/actions/#networkrequest) but you can also persist them **locally**.

1. To access an app-wide global storage, you can use the [$global](/actions/#global) API.
2. To privately persist data under a URL (only accessible from that view), you can use the [$cache](/actions/#cache) API.

---

## 5. Local Image

Jasonette not only lets you render remote images but also lets you bundle them up on the app locally and load them locally. Just like you can use the [file:// scheme](#2-file-url) to render local JSON objects, you can use the same `file://` scheme to access locally stored images.

Supported everywhere:

- [body.background](/document/#background)
- [header.menu](/document/#menu)
- [header.title](/document/#title)
- [footer.tabs](/document/#tabs)
- [footer.input](/document/#input)
- [image component](/components/#image)
- [button component](/components/#button)
- [image layers](/document/#typeimage)

