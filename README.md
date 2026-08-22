# ZKas Paper Wallet

A single-file, **offline** paper-wallet generator for [ZKas](https://github.com/firecash/zkas-rusty)
— the shielded-by-default (Orchard / Halo 2) rusty-kaspa fork.

**Live:** https://firecash.github.io/zkas-paper-wallet/

It generates a shielded (`zkas:`) address and its 32-byte recovery seed **entirely in
your browser**, in WebAssembly ([`zkas-signer`](https://github.com/firecash/zkas-signer)).
Keys never leave the page and it makes **zero network requests**.

The recovery seed — both the hex string and its QR code — stays blurred until you press
**Reveal**, so it can't be shoulder-surfed or caught in a screenshot. Printing reveals it
automatically so the paper backup is complete.

## For real cold storage, run it offline

Hosting it here gives you an integrity-checked HTTPS download. For anything more than
pocket change:

1. Save this page to disk (`index.html`).
2. Disconnect from the internet — ideally on an air-gapped machine.
3. Open the saved file, generate a wallet, and **print or hand-copy the seed**.
4. Store the seed offline. The seed is the whole secret; anyone who sees it can spend.

The address is deterministically derived from the seed, so it can always be recomputed by
importing the seed into any ZKas wallet (`shielded-pay`, `zkas-walletd`, or the
web/mobile wallet).

## Verify it's offline

Open your browser's Network tab and reload: the page must issue **no requests** after load.
The wallet logic is the [`zkas-signer`](https://github.com/firecash/zkas-signer) crate
compiled to WebAssembly and inlined as base64 — there is nothing external to fetch.

To confirm this page was built from that source, rebuild the wasm and compare its hash
against the copy embedded here:

```
sha256 = a22dfabab0b6129ae4d924a51a1a12608523c1dc3e07965c2897f87dc8a936e9
size   = 498824 bytes
```

Extract the embedded copy from this page and check it yourself:

```sh
python3 - <<'PY'
import re, base64, hashlib
s = open("index.html", encoding="utf-8").read()
w = base64.b64decode(re.search(r'const WASM_B64\s*=\s*"([A-Za-z0-9+/=]+)"', s).group(1))
print(len(w), hashlib.sha256(w).hexdigest())
PY
```

(Built with wasm-bindgen 0.2.100, release profile. See zkas-signer's README for the exact
build steps.)

## License

MIT OR Apache-2.0.
