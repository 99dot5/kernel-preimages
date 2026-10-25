# kernel-preimages

Reveal pages (preimages) of each released 99dot5 smart-rollup kernel, one GitHub release per kernel version.

A Tezos smart rollup kernel reaches a rollup node as a tree of content-addressed 4 KiB pages: each page is named by its own hash, the root hash is what the rollup records on chain, and the node reveals the pages to the PVM, which verifies every one of them while reassembling the kernel. A rollup node that replays the rollup from its origination needs the pages of every kernel the rollup has booted, so they are published here, where any node can fetch them over plain HTTPS with no credentials.

## What a release carries

Release `smart-rollup-v<version>`:

| Asset | What it is |
|---|---|
| `preimages-<root hash>.tar.gz` | The pages, flat at the tarball's root: the files of a rollup node's `wasm_2_0_0/` directory |
| `preimages-<root hash>.tar.gz.sha256` | Its digest, the one a node pins |
| `kernel.wasm`, `kernel.wasm.sha256` | The kernel the pages were derived from |

The release notes repeat the kernel digest, the root hash and the page count.

## Trust

This repository is trusted for availability only. Every page is verified by the PVM against its own hash on reveal, and the root hash a rollup accepts is recorded on chain, so a wrong or altered page is rejected by the node, never executed. The kernel bytes are public in any case: any rollup node serves them from `/kernel/boot.wasm`.

A published asset is never replaced or deleted. A new kernel is a new release.

## Re-deriving a set

With the Tezos SDK's installer tool (`cargo install tezos-smart-rollup-installer`):

```bash
sha256sum -c kernel.wasm.sha256
smart-rollup-installer get-reveal-installer --upgrade-to kernel.wasm --output /tmp/unused.hex --preimages-dir ./preimages
