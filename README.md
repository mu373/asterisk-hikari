# asterisk-hikari
Asterisk server on Docker that connects to Hikari Denwa via NTT East Home Gateway (e.g., PR-400NE).

## Usage

### Setup config
- Edit the following config files
  - `pjsip.conf`
  - `pjsip_wizard.conf`: Your endpoints that you want to connect to this SIP server.
  - `extensions.conf`
  - `iax.conf` (If you want IAX connection)
- Change following variables
  - `__HOME_GATEWAY_IP__`: IP address of NTT East Home Gateway (e.g., `192.168.1.1`)
  - `__SIP_CLIENT_ID__`: ID of SIP terminal registered to HGW (e.g., `1`). For username, you might have to use 4 digit id `0001` depending on your setting
  - `__SIP_CLIENT_PASSWORD__`: Password of SIP terminal registered to HGW
  - `__YOUR_PASSWORD__` (in `pjsip_wizard.conf` and `iax.conf`): Password for new SIP terminal that you want to connect to this SIP server.
  - `__YOUR_HIKARI_PHONE_NUMBER__`: Your phone number for Hikari Denwa (0AB-J number)

### Start Docker container
```sh
docker compose up -d
```

### Connect from SIP/IAX clients
- [Acrobits softphone](https://acrobits.net/sip-client-ios-android/) (iOS/Android, SIP)
- [Telephone](https://apps.apple.com/jp/app/telephone/id406825478) (macOS, SIP)
- [Zoiper](https://www.zoiper.com/) (Windows/macOS/Linux/iOS/Android, SIP/IAX)

## Useful commands
```sh
# Open bash inside container
$ docker exec -it asterisk-asterisk-1 bash

# Inside container
asterisk -vvvrc
> pjsip reload
> pjsip show endpoints
> pjsip show registrations
```

## Notes
- Setting `network_mode: bridge` in `docker-compose.yml` could cause RTP/IAX connectivity issues where the phone audio is not transmitted properly
- Confirmed that phone calls can be made on SIP clients connected with [Tailscale](https://tailscale.com/)

## Refrerences
- [江戸の電話を長崎で取る - fetburner.core ](https://fetburner.hatenablog.com/entry/2024/03/10/192545)
- [ひかり電話HGW Pjsip - VoIP-Info.jp](https://www.voip-info.jp/index.php/%E3%81%B2%E3%81%8B%E3%82%8A%E9%9B%BB%E8%A9%B1HGW_Pjsip)
- [mlan/docker-asterisk](https://github.com/mlan/docker-asterisk)
