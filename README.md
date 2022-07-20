# Edie, the IPNS site pin manager

Edie is a simple Bash script to help pin and manage seeded sites hosted over IPNS, similar to Freenet's bookmark feature. It reads from a file (at `$HOME/.edie/urls`) with a list of domains set up to use DNSLink and can fetch newer versions of the underlying IPNS hash, as well as clean up old files when removing a site.

Please note that it does not yet have functionality for *creating* a new IPNS site.

Support for pinning IPNS sites without a domain (eg. /ipns/bafybeibt6ozbpel3rne5obfsofvwep6wefx7iangfpjgqh3fsbdapbm43i) will be added in a future release.

Edie does not run in the background. For auto-updates like in ZeroNet and Freenet, you must edit your own crontab to periodically call `edie -u`.

## Dependencies

- Obviously an IPFS daemon running in the background. Edie has only been tested with [kubo](https://github.com/ipfs/kubo) and expects whatever IPFS implementation to be used to be available at the command `ipfs` (in other words, `ipfs` has to be in your $PATH) and to use the same options and output the same strings as `kubo`.
- `xargs` and the GNU coreutils/findutils.
- Bash.

## Installation

`git clone` this repo and then symlink `edie` anywhere in your $PATH.

## Examples

### Add a new site

Input: `edie -a shimmy1996.com`

Output: `pinned QmXdHCZYM9oh2ZcGQD4aYFrTGNXygaGXqEiUSQWvFqu1GL recursively`

### List info about a pinned site

Input: `edie -v shimmy1996.com`

Output:
```
shimmy1996.com
CID: /ipfs/QmXdHCZYM9oh2ZcGQD4aYFrTGNXygaGXqEiUSQWvFqu1GL
Size: 391MiB
```

Please note that the size may actually be smaller than outputted due to deduplication or compression.

### Update all pinned sites

Input: `edie -u`

Output:
```
Now updating teetotality.blog
pinned QmTJm8JQnud4KSbcCV9BRKEEaKMBFJ8XU61SEohbUTRYUK recursively

Now updating daviddias.me
pinned QmaM39bEEni8SW75KdduCoGVRyhK8fXQAVRxqa1CWCw1Fp recursively

Now updating shimmy1996.com
pinned QmXdHCZYM9oh2ZcGQD4aYFrTGNXygaGXqEiUSQWvFqu1GL recursively
```

...et cetera.

### List all other commands

Input: `edie -h`

Output:
```
edie v.20220720
-l: list all pinned sites
-u: update all pinned sites
-d: unpin a site and delete it from storage
-a: pin a site and add it to storage
-v: show detailed info about a pinned site
-h: display this help
```

## License

GPLv3 only.
