# Getting Device Information

You can access the device information via `$env` variable.

Note that `$env` is a **read-only** variable, so you can only read and not write to it.

You can access them using templates, just like other variables (global variable, cache, local variables).

## Supported Attributes

- **$env.device.width** : device width
- **$env.device.height** : device height
- **$env.device.os.name** : os name

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
                  "text": "version: {{$env.device.os.sdk}}"
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
