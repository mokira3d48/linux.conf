## Disable touchscreen
What worked for me on Ubuntu 24.04 was:

```sh
xinput list; # Find the id of the touchscreen
xinput disable <device-id>; # e.g. `xinput disable 10`
```
