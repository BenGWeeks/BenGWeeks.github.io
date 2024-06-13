# Using GitHub Pages for NIP-05 Identifiers

## What are NIP-05 Identifiers?

NIP-05 Identifiers are an [e-mail address notation](https://datatracker.ietf.org/doc/html/rfc5322#section-3.4.1) that confirms the owner of the address also owns a specific domain or sub-domain or the owner allowed you to register there.

Relays can prioritize and clients can "trust" NIP-05 identified users.

This trust-minimized verification system is a decentralised way of combating bad actors. If someone uses a domain to setup NIP-05 identified bots, these can be blocked by relays, and it makes it more difficult for bad actors to be NIP-05 identified as it takes time to setup domains in such a way. 

## How do NIP-05 Identifies work?

A NIP-05 identifier, of the form `<local-part>@<domain>` is an address that is resolved to a GET request to the following address:

`https://<domain>/.well-known/nostr.json?name=<local-part>`

For example, the address of `ben@bengweeks.github.io` has a respective JSON file available to a GET request (via GitHub pages) at:

`https://bengweeks.gihub.io/.well-known/nostr.json?name=ben`

Where the contents of the JSON file are as follows:

```
{
  "names": {
    "ben": "971615b70ad9ec896f8d5ba0f2d01652f1dfe5f9ced81ac9469ca7facefad68b"
  }
}
```

CAREFUL: The string in the JSON file of `971615b70ad9ec896f8d5ba0f2d01652f1dfe5f9ced81ac9469ca7facefad68b` is my Nostr public key (PubKey) in HEX format, as apposed to the [more standard] nPub format that starts with `nPub1`. You can convert your nPub key at [https://damus.io/key](https://damus.io/key).

NB The querystring of `name=<local-part>` is there to be able to dynamically create the JSON file when used by a host for multiple users.

The above example of the NIP-05 identifier for ben@bengweeks.github.io can be seen here:

[https://bengweeks.github.io/.well-known/nostr.json?name=ben](https://bengweeks.github.io/.well-known/nostr.json?name=ben)

## How do you to use GitHub pages to host your NIP-05 identifier?

To create a NIP-05 identifier using GitHub:

1. Fork this Repository (or create a new one) using GitHub, naming it `yourusername.github.io` (otherwise the Url will be `yourusername.github.io/repository_name` and not the route of your GitHub pages website).
2. Create a file called `.well-known/nostr.json` (in lower-case) if you are creating a new repositoy rather than forking this and enter your own `"<local-part>": "<public-key>"` as described above.
3. Go to `Settings` for the Repository, and under `Code and automation` choose `Pages`.
4. Under `Source` > `Build and deployment`, rather than  `Deploy from a branch`, choose `GitHub Actions` and `Static HTML` (otherwise you will find the JSON file does not get published).
6. Make a Commit to publish your site.
7. Confirm the JSON file is available in the browser.

NB GitHub web page hosting is case-sensitive hence suggesting all lower case.

## How do you set your NIP-05 Identifier?

Using a client such as [https://astral.ninja](https://astral.ninja) or  [https://metadata.nostr.com](https://metadata.nostr.com) (the latter requires the [Nos2x](https://github.com/fiatjaf/nos2x) browser extension), enter your NIP-05 identifier into your profile settings in the form of `ben@bengweeks.github.io`.

**Note: Most clients are not using this yet.**

## Cross-Origin Resource Sharing (CORS)

Your host must have the Cross-Origin Resource Sharing (CORS) policy set correctly to work (such as with GitHub pages - although custom domains may not work). Specifically, the `Access-Control-Allow-Origin: *` header.

## References

[Setting up a NIP-05 identifier](https://github.com/nostr-protocol/nips/blob/master/05.md)

[Setting up GitHub pages](https://docs.github.com/en/pages/getting-started-with-github-pages/creating-a-github-pages-site)
