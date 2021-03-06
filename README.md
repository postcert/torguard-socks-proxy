# A socks5 proxy via Torguard. (Not Working Yet)
An [Alpine](https://alpinelinux.org/) Linux container running a socks5 proxy (using [dante](https://www.inet.no/dante/)) via Torguard (OpenVPN).

Protect your browsing activities through an encrypted and anonymized VPN proxy!

You will need a [Torguard](https://torguard.net/index.php) account.
If you don't have one, you can [sign up here](https://torguard.net/anonymoustorrentvpn.php) for one.

[![Docker Build Status](https://img.shields.io/docker/build/postcert/torguard-socks-proxy.svg?style=flat-square)]()
[![Docker Build Status](https://img.shields.io/docker/automated/postcert/torguard-socks-proxy.svg?style=flat-square)]()
[![Docker Build Status](https://img.shields.io/docker/pulls/postcert/torguard-socks-proxy.svg?style=flat-square)]()
[![Docker Build Status](https://img.shields.io/docker/stars/postcert/torguard-socks-proxy.svg?style=flat-square)]()


## Starting the VPN Proxy

```sh
docker run -d \
--cap-add=NET_ADMIN \
--device=/dev/net/tun \
--name=pia-socks-proxy \
--restart=always \
-e "USERNAME=<pia_username>" \
-e "PASSWORD=<pia_password>" \
-e "REGION=<region>" \ # default US East
-e "DNS=<dns servers" \ # default: cloudflare's tls servers 1.1.1.1@853#cloudflare-dns.com 1.0.0.1@853#cloudflare-dns.com
-e "ENCRYPTION=[strong|normal]" # default: strong
-e "PROTOCOL=[udp|tcp]" #default: udp
-p 1080:1080 \
oneofone/pia-socks-proxy
```

Substitute the environment variables for `REGION`, `USERNAME`, `PASSWORD` as indicated.

A `docker-compose-dist.yml` file has also been provided. Copy this file to `docker-compose.yml` and substitute the environment variables are indicated.

Then start the VPN Proxy via:

```sh
docker-compose up -d
```

### Environment Variables

`REGION` is optional. The default region is set to `USA-SEATTLE`. `REGION` should match the Torguard `.opvn` filename.

The vpn location names don't follow much of a pattern so below are the current available vpn locations:
```
gfind ./ -name "*.ovpn" -printf "%f\n" | sed -e "s/TorGuard\.//" | sed -e "s/\.ovpn//"

India.Bangalore
Poland
Romania
New.Zealand
South.Korea
Spain
USA-LOS.ANGELES
USA-NEW-JERSEY
Czech
Chile
Netherlands
Norway
UK.London
Luxembourg
Switzerland
Taiwan
Thailand
Finland
Mexico
Iceland
Australia.Sydney
Brazil
USA-ATLANTA
USA-CHICAGO
Belgium
Portugal
Canada.Vancouver
USA-NEW-YORK
Slovakia
USA-DALLAS
Germany
Italy
Hungary
Singapore
Israel
USA-MIAMI
Cyprus
France
Bulgaria
Japan
Sweden
Ireland
USA-LAS-VEGAS
Latvia
Austria
Hong.Kong
Denmark
Greece
Moldova
USA-SAN-FRANCISCO
Canada.Toronto
USA-SEATTLE
Belarus
```
(The OVPN zips are freely available to download and can be used to check for new regions)

`USERNAME` / `PASSWORD` - Credentials to connect to Torguard

## Connecting to the VPN Proxy

To connect to the VPN Proxy, set your browser socks5 proxy to localhost:1080.

### Recommended (100% personal preference) addons

- Chrome: [ProxySwitchyOmega](https://chrome.google.com/webstore/detail/proxy-switchyomega/padekgcemlokbadohgkifijomclgjgif)

- Firefox: [ProxySwitcher](https://addons.mozilla.org/en-US/firefox/addon/proxy-switcher/)

### Commend line

- curl:

```shell
# socks5h so the dns is done over the socks proxy
$ curl -v --proxy socks5h://localhost:1080
```

- git:

```shell
env ALL_PROXY=socks5h://localhost:1080 git clone https://github.com/some/one.git
```

## Credits
- [OneOfOne/pia-socks-proxy](https://github.com/OneOfOne/pia-socks-proxy), modified for use with Torguard
- [act28/pia-openvpn-proxy](https://github.com/act28/pia-openvpn-proxy), used the openvpn config from his image, but he uses privoxy rather than socks5.
- [qmcgaw/private-internet-access](https://github.com/qdm12/private-internet-access-docker) unbound config + dynamically download the latest PIA configs.
