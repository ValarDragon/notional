# Notional VPN

It's become evident that a global VPN system will smooth operations at Notional.  

There seem to be three top candidates:

* Tailscale
* Zerotier
* Slack's Nebula

What do we want?

We want a polycentric VPN that can help our growing team to coordinate globally.  Our emphasis on edge systems means that we do not get cloud automation niceties.  We have to make up for that by building our own niceties, because foregoing niceties would not be smart. 

## Links

https://github.com/wenerme/wener/blob/76c45c3ba784a318cb5f0b1d348ecba2232e2f43/notes/service/network/vpn-awesome.md

https://github.com/wenerme/wener/blob/master/notes/service/network/vpn-awesome.md


## Deployment Phases

1) Zerotier

Join this network, and make an issue on this repository:

sudo zerotier-cli join 9f77fc393e7dfae7


2) A fully self hosted solution:


https://github.com/wiretrustee/wiretrustee
