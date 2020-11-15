---
description: Authors sign web pages.
---

# Signing Web Pages

For this example, we'll be signing an `index.html` page.

1. Generate a PGP key pair if you haven't already. Make sure it has a name and email attached.

```bash
gpg --full-generate-key
```

{% hint style="info" %}
Support for other signing protocols will be added in the future.
{% endhint %}

2. Upload your public key to [https://keys.openpgp.org/](https://keys.openpgp.org/) and verify an email address.

{% hint style="info" %}
Support for other Key Servers will be added in the future.
{% endhint %}

3. Decide where you're going to place your signature. For example:

```text
https://your.domain/path/to/index.html.sig
```

4. Add your signature path to your page:

```markup
<link rel="signature" href="https://your.domain/path/to/index.html.sig" />
```

{% hint style="info" %}
Currently, the signature is used to fetch the public key from keys.openpgp.org. An option to include the public key as a separate tag will be added in the future.
{% endhint %}

5. Sign the web page using `gpg` on any other OpenPGP tool to generate a detached armored signature.

```bash
gpg --detach-sign --armor --output "${sig_path}" "${html_path}"

# Check the signature.
cat "${sig_path}"

# It should look something like:
-----BEGIN PGP SIGNATURE-----

iQIzBAABCAAdFiEEAjotydKrjAeXmRALhHSlzvcvms0FAl+nWdoACgkQhHSlzvcv
ZlxVaS... and so on.
-----END PGP SIGNATURE-----
```

{% hint style="info" %}
If you have multiple HTML files, [see this script](https://github.com/jahed/webverify/blob/master/contrib/sign-html.sh).
{% endhint %}

6. You're done! WebVerify will detect your signature and verify your web page.

![A verified page.](.gitbook/assets/verified.png)

## Adding an Avatar

Photo IDs aren't supported on keys.openpgp.org. Instead you can use your public key's verified email to upload an avatar to [Libravatar](https://www.libravatar.org/). Libravatar is a lot like Gravatar, except it uses [iavatar](https://git.linux-kernel.at/oliver/ivatar) which is open source.

If your email is under your own domain, you can also [run your own instance](https://wiki.libravatar.org/running_your_own/) and WebVerify will check your instance first before falling back to Libravatar.

{% hint style="info" %}
Support for disabling avatars and using other Universal Avatar APIs will be added in the future.
{% endhint %}

## Dynamic Pages

For dynamic pages, the same rules apply. Fully render your page first including the `<link />` tag, then generate the signature. Since the web page URL can have multiple signatures, you'll need to generate a unique signature path when the content differs.

## Cache Conflicts

Caching can be a problem when you have two static endpoints that need to be in sync, in this case: the web page and signature. If a cached signature does not represent the current page, it will fail verification. This is similar to when your CSS or JavaScript goes out of sync so similar solutions apply. Some of those solutions include:

* Add `?v=1` to the end of your signature path and increment `v` any time there's a change.
* Use a placeholder for your signature path, hash the page and replace the placeholder with a hash-based path like `index.html.abc123.sig`.

## Subresource Verification

Only the HTML source of the web page will be verified. External files like images, scripts and stylesheets are not verified. [Subresource Integrity](https://developer.mozilla.org/en-US/docs/Web/Security/Subresource_Integrity) \(SRI\) already exists for some of those cases, and by including `integrity` attributes it can be assumed the content of those files are approved by the author. All that's left is for the web browser to do its job and verify the integrity.

SRI is not available for all content, like images. However, [according to the spec](https://www.w3.org/TR/SRI/), a future revision will include them.

{% hint style="info" %}
Subresource Verification may be supported in the future using HTTP Headers.
{% endhint %}

## HTTP Headers

{% hint style="info" %}
Support for HTTP Headers will be added in the future.
{% endhint %}



