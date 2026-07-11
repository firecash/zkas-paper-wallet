# FireCash Paper Wallet

A single-file, **offline** paper-wallet generator for [FireCash](https://github.com/firecash/firecash-rusty)
— the shielded-by-default (Orchard / Halo 2) rusty-kaspa fork.

**Live:** https://firecash.github.io/firecash-paper-wallet/

It generates a shielded (`firecash:`) address and its 32-byte recovery seed **entirely in
your browser**, in WebAssembly ([`firecash-signer`](https://github.com/firecash/firecash-rusty)).
Keys never leave the page and it makes **zero network requests**.

## For real cold storage, run it offline

Hosting it here gives you an integrity-checked HTTPS download. For anything more than
pocket change:

1. Save this page to disk (`index.html`).
2. Disconnect from the internet — ideally on an air-gapped machine.
3. Open the saved file, generate a wallet, and **print or hand-copy the seed**.
4. Store the seed offline. The seed is the whole secret; anyone who sees it can spend.

The address is deterministically derived from the seed, so it can always be recomputed by
importing the seed into any FireCash wallet (`shielded-pay`, `firecash-walletd`, or the
web/mobile wallet).

## Verify it's offline

Open your browser's Network tab and reload: the page must issue **no requests** after load.
The wallet logic is the `firecash-signer` crate compiled to WebAssembly and inlined as
base64 — there is nothing external to fetch.

## License

MIT OR Apache-2.0.
