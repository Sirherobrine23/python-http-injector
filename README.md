# HTTP-INJECTOR OPENWRT

### packages that must be installed
* python 2.7
    * python-codecs 
    * python-logging 
    * python-base 
    * python-light
    ````
    opkg update
    opkg install python3-base python3-light python3-logging python3-codecs 
    ````
* openvpn
    * openvpn-openssl
    ````
    opkg update
    opkg install openvpn-openssl
    ````

1. install all the required packages
2. create an openvpn interface
    ````
    uci set network.openvpn=interface
    uci set network.openvpn.proto='none'
    uci set network.openvpn.ifname='tun0'

    uci commit
    ````
3. set the firewall for the openvpn interface you just created  
    *firewall zone rule*
    ````
    uci add firewall zone # =cfg06dc81
    uci set firewall.@zone[-1].name='openvpn'
    uci set firewall.@zone[-1].output='ACCEPT'
    uci set firewall.@zone[-1].forward='REJECT'
    uci set firewall.@zone[-1].input='ACCEPT'
    uci set firewall.@zone[-1].masq='1'
    uci set firewall.@zone[-1].mtu_fix='1'
    uci set firewall.@zone[-1].network='openvpn'
    ````
    *firewall forwading rule*
    ````
    uci add firewall forwarding
    uci set firewall.@forwarding[-1].src='lan'
    uci set firewall.@forwarding[-1].dest='openvpn'

    uci commit
    ````