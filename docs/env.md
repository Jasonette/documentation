
You can get useful information about the device or the app's current state via the `$env` variable.

Note that `$env` is a **read-only** variable, so you can only read and not write to it.

You can access them using templates, just like other variables (global variable, cache, local variables).

## Supported Attributes

- **$env.device.width** : device width
- **$env.device.height** : device height
- **$env.device.os.name** : os name
- **$env.device.os.version** : os version
- **$env.device.language**: device language (`"en-US"`, etc.)
- **$env.view.url**: current view url

## Example

```
{
  "$jason": {
    "head": {
      "title": "$env.device access",
      "actions": {
        "$load": {
          "type": "$render"
        }
      },
      "templates": {
        "body": {
          "sections": [
            {
              "items": [
                {
                  "type": "label",
                  "text": "Device width: {{$env.device.width}}"
                },
                {
                  "type": "label",
                  "text": "Device height: {{$env.device.height}}"
                },
                {
                  "type": "label",
                  "text": "{{$env.device.os.name}}"
                },
                {
                  "type": "label",
                  "text": "version: {{$env.device.os.version}}"
                },
                {
                  "type": "label",
                  "text": "version: {{$env.device.language}}"
                }
              ]
            }
          ]
        }
      }
    }
  }
}
```
