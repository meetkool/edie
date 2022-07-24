# Edie, the IPNS site pin manager

Edie is a simple Bash script to help pin and manage seeded sites hosted over IPNS, similar to Freenet's bookmark feature. It reads from a file (at `$HOME/.edie/urls`) with a list of raw peer hashes or domains set up to use DNSLink and can fetch newer versions of the underlying IPNS hash, as well as clean up old files when removing a site.

Edie currently does not support pinning IPNS sites using CIDv1 (hashes starting with 'bafy').

Edie does not run in the background. For auto-updates like in ZeroNet and Freenet, you must edit your own crontab to periodically call `edie -u`.

## Dependencies

- Obviously an IPFS daemon running in the background. Edie has only been tested with [kubo](https://github.com/ipfs/kubo) and expects whatever IPFS implementation to be used to be available at the command `ipfs` (in other words, `ipfs` has to be in your $PATH) and to use the same options and output the same strings as `kubo`.
- `xargs` and the GNU coreutils/findutils.
- Bash.

## Installation

`git clone` this repo and then symlink `edie` anywhere in your $PATH.

## Examples

### Creating your own site

#### Creating a new site (interactive mode only for now)

Input: `edie -c`

Output:
```
Nickname for site (no spaces): EdieTest
Full path to site files (no spaces): /var/www/misc/
 19.81 MiB / 19.81 MiB [============================] 100.00%
This next step may take a while depending on the size of the site. Hang tight!
Published to k2k4r8lzp5lprvvepogfviahclcb5cuau5afgk16zp89l8rfz7w8w5a8: /ipfs/QmdzZfoxk5KPje8qLUKpPPghyhrySzsDAw4SRJXdMWH2qD
Site created.
Keys and config backed up in ~/.edie/sites/
```

#### List info about a created site

Input: `edie -i EdieTest`

Output:
```
Site name: EdieTest
IPNS hash: /ipns/k2k4r8lzp5lprvvepogfviahclcb5cuau5afgk16zp89l8rfz7w8w5a8
Local path to site files: /var/www/misc/
```

#### Update a site

Input: `edie -k EdieTest`

Output:
```
Updating EdieTest
 19.81 MiB / 19.81 MiB [============================] 100.00%
This next step may take a while depending on the size of the site. Hang tight!
Published to k2k4r8lzp5lprvvepogfviahclcb5cuau5afgk16zp89l8rfz7w8w5a8: /ipfs/QmdzZfoxk5KPje8qLUKpPPghyhrySzsDAw4SRJXdMWH2qD
Site updated.
```

### Managing pinned sites from other people

#### Add a new site to your storage

Input: `edie -a shimmy1996.com`

Output: `pinned QmXdHCZYM9oh2ZcGQD4aYFrTGNXygaGXqEiUSQWvFqu1GL recursively`

#### List info about a pinned site

Input: `edie -v shimmy1996.com`

Output:
```
shimmy1996.com
CID: /ipfs/QmXdHCZYM9oh2ZcGQD4aYFrTGNXygaGXqEiUSQWvFqu1GL
Size: 391MiB
```

Please note that the size may actually be smaller than outputted due to deduplication or compression.

#### Update all pinned sites

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
edie v.20220723
for creating sites:
  -c: create a new site (in interactive mode)
  -i: display info about a created site
  -k: update a site
for managing pinned sites:
  -l: list all pinned sites
  -u: update all pinned sites
  -d: unpin a site and delete it from storage
  -a: pin a site and add it to storage
  -v: show detailed info about a pinned site
-h: display this help
```

## License

GPLv3 only.
