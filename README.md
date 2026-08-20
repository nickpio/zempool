# Zempool

Zempool is a Zcash mempool visualizer, explorer, and API. It is a fork of [the Mempool Open Source Project](https://github.com/mempool/mempool) (AGPL-3.0), reworked to talk to a Zcash full node instead of Bitcoin Core.

It is not affiliated with mempool.space. "Mempool" is a trademark of Mempool Space. This project keeps the original copyright notices and AGPL license.

## What it does

Zempool shows the live Zcash mempool, projected blocks, fees, and a transparent-first explorer (blocks, transactions, search). Shielded transactions appear as first-class objects. Amounts that the chain does not reveal stay hidden.

It connects to **Zebra** (`zebrad`) or **Zakura** (`zakurad`) over JSON-RPC. zcashd is not supported. Zakura's zcashd-compat sidecar is not supported either. Point Zempool at native node RPC.

Default RPC port is 8232 (testnet 18232). Cookie files live at `~/.cache/zakura/.cookie` or `~/.cache/zebra/.cookie`.

The node must be an archive / non-pruned node. A pruned Zakura snapshot is fine for a wallet, not for an explorer.

Address history needs insight-style RPCs on the node (`getaddressbalance` / `getaddressdeltas`). Without those, search still decodes addresses, but the address page has no history.

## Install

Most people should use Docker. See [`docker/`](./docker/). You still have to run Zebra or Zakura yourself, with RPC enabled and prune off.

Developers: [`backend/`](./backend/) and [`frontend/`](./frontend/).

Production-shaped deploys: [`production/`](./production/). Those scripts still show their Bitcoin-era layout. Treat them as a starting point, not a drop-in Zcash stack.

## License

GNU Affero General Public License v3.0. See [LICENSE](./LICENSE) and [COPYING.md](./COPYING.md).
