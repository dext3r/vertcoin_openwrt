# vertcoin_openwrt
Packages to build Vertcoin for OpenWRT.

### INSTALLATION

Add the following line to feeds.conf.default in the OpenWRT trunk:

`src-git vertcoin_openwrt https://github.com/dext3r/vertcoin_openwrt.git`

Update the feed:

`./scripts/feeds update vertcoin_openwrt`

Install the package:

`./scripts/feeds install vertcoin`
