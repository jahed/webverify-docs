---
description: Improving the Web Extension.
---

# Development

You will need the latest Node LTS and Yarn installed.

Create a `web-ext` Firefox Profile by going to `about:profiles`.

```bash
git clone git@github.com:jahed/webverify.git
cd webverify
./develop.sh
```

This will install dependencies and launch Firefox using the `web-ext` profile with the extension installed and will automatically reload it when there are any file changes.

`web-ext` makes config changes so using any other profile is not recommended. Any changes you make within the `web-ext` window will not be saved. To make permanent changes, launch the `web-ext` profile from `about:profiles`.

