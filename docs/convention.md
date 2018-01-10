Jasonette is built around various conventions in order to make things simpler.

---

# Rules

## ■  Everything is a string
The JSON we use in Jasonette is completely string based. All values are string. Sometimes this may look inconvenient, but **assuming everything is a string** provides lots of convenience when JSON parsing is concerned.

## ■  All keywords are lowercased
All reserved keywords on Jasonette (component name, action name, etc.) are lowercased. 

---

#Values

## ■ color

###rgb
We can use rgb code.

    {
      "body": {
        "background": "rgb(244,244,244)"
      }
    }

###rgba
We can use rgba code. r,g,b parts are same as rgb, but the last value is the opacity.

    {
      "body": {
        "background": "rgba(255,255,255,0.3)"
      }
    }

###hex code
We can also use hex code

    {
      "body": {
        "background": "#ff0000"
      }
    }

---

## ■ dimension

All numbers are pixels unless otherwise specified.

###1. Pixels
Below we create a label with 300pixel width.

    {
      "type": "label",
      "text": "hi",
      "style": {
        "width": "300"
      }
    }

###2. Percentage
We can use the `%` sign. When we do so, it sets the dimension based on the current orientation.

    {
      "type": "label",
      "text": "hi",
      "style": {
        "width": "100%"
      }
    }


###3. Pixels and Percentage
We can also use a combination of percentage and pixels.

In the following example we create a label that's 80 pixels smaller horizontally than the full device width:

    {
      "type": "label",
      "text": "hi",
      "style": {
        "width": "100%-80"
      }
    }

