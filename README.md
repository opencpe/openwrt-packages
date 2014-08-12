NETCONF support for OpenWRT the OpenCPE way
===========================================

The OpenCPE projects aims to provide NETCONF support in a non-intrusive way.
It therefore does not replace OpenWRT uci framework, but build on top it.

NETCONF support consists of tree main parts:
* NETCONF model and data storage daemon (mand)
* NETCONF protocol frontend (freenetconfd)
* NETCONF to UCI bridge (mand-cfg)

For the Cisco Linksys EA4500 an additional component provides firmware
upgrade support.

Building NETCONF support
------------------------

### Software needed on the Host system

1. xsltproc: should be available as package on all major linux distributions
2. pyang: [download][1] and install it with `python setup.py install`

### Install Feed

* Add [OpenCPE package feed][2] to `feeds.conf.default`, like:

```
src-git opencpe https://github.com/opencpe/openwrt-packages.git
```

* Disable OpenWrt's `management` feed in `feeds.conf.default`, like:

```
# disable openwrt management feed
#src-git management https://github.com/openwrt-management/packages.git
```

### Install all Packages

* Install all OpenCPE Packages:

```
% ./scripts/feeds install -a -p opencpe
```

`opencpe` is the alias in `feeds.conf.default`.

* `libexpat` is needed as a dependency. Installed it from OpenWrt's `packages`
feed:

```
% ./scripts/feeds install -p packages libexpat
```

### Configure OpenWrt Buildroot

* To select all packages from OpenCPE, run:
```
    % make menuconfig
```
* Now select all packages, namely:
  1. `Utilities -> mand`
  2. `Utilities -> mand-cfgd`
  3. `Utilities -> freenetconfd`
* same of this packages maybe already selected.

### Build OpenWrt

Build OpenWrt as usual, like:

```
% make
```

Using NETCONF support
---------------------

Setup and operation of a NETCONF server is out of the scope of this guide. It is
expected that you have a running NETCONF server that supports the
[OpenCPE YANG models][3]

If your OpenWRT was installed without the NETCONF packages, install and enable
freenetconfd, mand and mand-cfg:

    opkg install freenetconfd mand mand-cfg
    /etc/init.d/freenetconfd enable
    /etc/init.d/mand enable
    /etc/init.d/mand-cfg enable

Adjust /etc/config/freenetconfd to point to the correct ssh-keys to permit
your NETCONF server to connect to the device. Restart freenetconfd after
changing the config.

Now try to connect to the device from the NETCONF server. The /system-state
subtree should give you the OpenWRT version and release information.

Interfaces on device other than the Cisco Linksys EA4500 in OpenCPE configuration
---------------------------------------------------------------------------------

Physical interface informations need to be mapped to logical interfaces for NETCONF.
This is done through the default config of mand in /etc/dm/dm.xml. The shiped default
if preconfigured for the Cisco Linksys EA4500 in OpenCPE configuration.

Each device interface needs a /OpenCPE/interfaces and /OpenCPE/interfaces-state section
in this files. The structure of those entries is defined by the OpenCPE YANG models.

Sample:

    <interface instance='2'>
        <name>lan</name>
        <description>LAN Interface</description>
        <type>ethernetCsmacd</type>
        <enabled>true</enabled>
        <link-up-down-trap-enable>enabled</link-up-down-trap-enable>
        <ipv4>
            <enabled>true</enabled>
            <forwarding>false</forwarding>
            <mtu>1500</mtu>
            <address instance='1'>
                <ip>192.168.1.1</ip>
                <prefix-length>24</prefix-length>
            </address>
        </ipv4>
        <ipv6>
            <enabled>true</enabled>
            <forwarding>false</forwarding>
            <mtu>1500</mtu>
            <address instance='1'>
                <ip>1::1</ip>
                <prefix-length>64</prefix-length>
            </address>
            <dup-addr-detect-transmits>0</dup-addr-detect-transmits>
            <autoconf>
                <create-global-addresses>false</create-global-addresses>
                <create-temporary-addresses>false</create-temporary-addresses>
                <temporary-valid-lifetime>0</temporary-valid-lifetime>
                <temporary-preferred-lifetime>0</temporary-preferred-lifetime>
            </autoconf>
        </ipv6>
    </interface>

and:

    <interface instance="2">
        <name>eth0</name>
        <type>ethernetCsmacd</type>
        <if-index>2</if-index>
        <higher-layer-if instance='1'>
            <higher-layer-if>interfaces-state.interface.5</higher-layer-if>
        </higher-layer-if>
    </interface>
 
[1]: https://code.google.com/p/pyang/
[2]: https://github.com/opencpe/openwrt-packages.git
[3]: https://github.com/opencpe/yang
