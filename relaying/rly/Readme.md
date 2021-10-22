# Go-relayer operational notes

## Vs Hermes
* When using Hermes, you relay all of the channels that you do not filter. When using the go relayer, you relay only the channels that you specifically specify.
* Hermes takes a minute or two to get running, especially if you are relaying many chains. The go relayer starts almost instantly.
* I found the go relayer to be a little more difficult to configure for the first time than hermes, specifically because I needed to carefully specify:
  * channels
  * connections
  * clients
* Hermes get s a bit upset when it sees a packet on a chain you're not running.  The solution is filtering, and you end up anti-specifying channels, instead of specifying them.
* It is still too early to make comparisons on fee calculations or tx latency


## Status

Notional is currently using rly to relay:

osmo, cosmos, akash, regen, dig, sentinel, starname

6x6=36
36x2=72

72 unidirectional channels is the goal, then.  We need a process monitor OR to run multiple channels with a single binary.

- [x] look into pm2
- [x] look into systemd
- [x] look into other process monitoring solutions

Amusingly, the solution is at present bash loops that run in screen sessions. 


We can expect this to expand exponentially, and believe that for the time being, the best pattern is all:all.


* osmosis-1 <-------> cosmoshub-4
* sentinelhub-2 <--------> osmosis-1
* sentinelhub-2 <--------> cosmoshub-4
* sentinelhub-2 <------> akashnet-2
* sentinelhub-2 <------> regen-1
* akashnet-2 <---------> cosmoshub-4
* akashnet-2 <---------> osmosis-1
* dig <--------> osmosis-1
* dig <--------> cosmoshub-4
* regen-1 <-----> cosmoshub-4
* regen-1 <-----> osmosis-1
* regen-1 <-----> akashnet-2
* 


## Setup
As with hermes, we recommend that you create a single seed phrase, and use that to create all of the wallets that you'll need for relaying.  For rly, we just moved over our cosmos hub and osmosis wallets.

PS: Notional urges the use of go 1.17 because it creates vastly clearer go.mod files and you can install it like:

```bash
wget -q -O - https://git.io/vQhTU | bash -s -- --version 1.17
````


**quickstart**

You'll need a synced Osmosis Archive node and a synced Gaia Archive node.  You can use pruning settings, but archive nodes provide certainty.  You will almost certainly need to adjust the rpc endpoints found in the config.yaml file-- unless you're just on localhost.


**Setup for quickstart:**
```bash
git clone https://github.com/strangelove-ventures/relayer
make install
rly init
wget -O ~/.relayer/config/config.yaml https://raw.githubusercontent.com/notional-labs/notional/master/rly/config.yaml
rly keys recover cosmoshub-4 cosmos "24 or 12 magic words invoked like a spell"
rly keys recover osmosis-1 osmosis "24 or 12 magic words invoked like a spell"
rly keys recover dig-1 dig "24 or 12 magic words invoked like a spell"
rly keys recover sentinelhub-2 sentinel "24 or 12 magic words invoked like a spell"
rly keys recover regen-1 regen "24 or 12 magic words invoked like a spell"
rly keys recover akashnet-2 akash "24 or 12 magic words invoked like a spell"
rly keys recover iov-mainnet-ibc iov "24 or 12 magic words invoked like a spell"
rly keys recover microtick-1 mtm "24 or 12 magic words invoked like a spell"
```

**relay hub to osmosis as a long-running daemon:**
```bash
rly start hubosmo
```

```bash
rly start osmohub
```

**relay osmo<->hub (0 and 141) in a crazy bash loop:**
```bash
bash osmoandhub.bash
```








