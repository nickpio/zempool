# Zempool frontend

Build the explorer UI against a local Zempool backend. There is no production Zempool API to proxy to, and Liquid is not part of this project.

Jump to a section in this doc:
- [Quick setup](#quick-setup)
- [Manual Frontend Setup](#manual-setup)
- [Translations](#translations-transifex-project)

## Quick setup

Set up the [backend](../backend/) first so the frontend has something to talk to.

```
git clone https://github.com/nickpio/zempool
cd zempool/frontend
```

Use Node.js 20.x and npm 9.x or newer.

```
$ npm install
$ npm run serve
```

The frontend is at http://localhost:4200/ and proxies API calls to the local backend.

### Test

Headless:

```
$ npm run config:defaults:mempool && npm run cypress:run
```

Interactive:

```
$ npm run config:defaults:mempool && npm run cypress:open
```

Cypress fixtures are still Bitcoin-era. Expect that suite to lag the Zcash conversion.

## Manual Setup

Set up the [Zempool backend](../backend/) first, if you haven't already.

### 1. Build the Frontend

_Make sure to use Node.js 20.x and npm 9.x or newer._

Build the frontend:

```
cd frontend
npm install
npm run build
```

### 2. Run the Frontend

#### Development

To run the local frontend against the local backend:

```
npm run serve
```

#### Production

The `npm run build` command from step 1 above should have generated a `dist` directory. Put the contents of `dist/` onto your web server.

You will probably want to set up a reverse proxy, TLS, etc. There are sample nginx configuration files in the top level of the repository for reference, but note that support for such tasks is outside the scope of this project.

## Translations: Transifex Project

Frontend strings are still the upstream Mempool locales (Transifex). English copy we touch will say Zempool and ZEC. Other locales will lag.

### Translators

* Arabic @baro0k
* Czech @pixelmade2
* Danish @pierrevendelboe
* German @Emzy
* English (default)
* Spanish @maxhodler @bisqes
* Persian @techmix
* French @Bayernatoor
* Korean @kcalvinalvinn @sogoagain
* Italian @HodlBits
* Lithuanian @eimze21
* Hebrew @rapidlab309
* Georgian @wyd_idk
* Hungarian @btcdragonlord
* Dutch @m__btc
* Japanese @wiz @japananon
* Norwegian @T82771355
* Polish @maciejsoltysiak
* Portugese @jgcastro1985
* Slovenian @thepkbadger
* Finnish @bio_bitcoin
* Swedish @softsimon_
* Thai @Gusb3ll
* Turkish @stackmore
* Ukrainian @volbil
* Vietnamese @BitcoinvnNews
* Chinese @wdljt
* Russian @TonyCrusoe @Bitconan
* Romanian @mirceavesa
* Macedonian @SkechBoy
* Nepalese @kebinm
