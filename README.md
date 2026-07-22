# ZKas Paper Wallet

A single-file, **offline** paper-wallet generator for [ZKas](https://github.com/firecash/firecash-rusty)
— the shielded-by-default (Orchard / Halo 2) rusty-kaspa fork.

**Live:** https://firecash.github.io/firecash-paper-wallet/

It generates a shielded (`firecash:`) address and its 32-byte recovery seed **entirely in
your browser**, in WebAssembly ([`firecash-signer`](https://github.com/firecash/firecash-signer)).
Keys never leave the page and it makes **zero network requests**.

## For real cold storage, run it offline

Hosting it here gives you an integrity-checked HTTPS download. For anything more than
pocket change:

1. Save this page to disk (`index.html`).
2. Disconnect from the internet — ideally on an air-gapped machine.
3. Open the saved file, generate a wallet, and **print or hand-copy the seed**.
4. Store the seed offline. The seed is the whole secret; anyone who sees it can spend.

The address is deterministically derived from the seed, so it can always be recomputed by
importing the seed into any ZKas wallet (`shielded-pay`, `firecash-walletd`, or the
web/mobile wallet).

## Verify it's offline

Open your browser's Network tab and reload: the page must issue **no requests** after load.
The wallet logic is the [`firecash-signer`](https://github.com/firecash/firecash-signer) crate
compiled to WebAssembly and inlined as base64 — there is nothing external to fetch.

To confirm this page was built from that source, rebuild the wasm and compare its hash:

```
sha256(firecash_signer_bg.wasm) = db0f4f715b4bab1b4ea6e27940a2c3a548bb134e85f2841dc734fa116e5fc7ee
```

(Built with wasm-bindgen 0.2.100, release profile. See firecash-signer's README for the
exact build steps.)

## License

MIT OR Apache-2.0.
