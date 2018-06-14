# vertcoin_openwrt
Packages to build Vertcoin for OpenWRT.

### INSTALLATION (Tested on OpenWRT 18.06)

Add the following line to feeds.conf.default in the OpenWRT trunk:

`src-git vertcoin_openwrt https://github.com/dext3r/vertcoin_openwrt.git`

Update the system with the feed:

`./scripts/feeds update vertcoin_openwrt`

Install the packages:

1. `./scripts/feeds install vertcoin`

   For the node daemon and command line interface.
2. `./scripts/feeds install db48`

   To add libdb48, required to enable wallet functionality. 
   
