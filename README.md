# Admin Interface for Open Balena

Open source admin interface for [openbalena](https://github.com/balena-io/open-balena), a platform to deploy and manage connected devices.

## Features
The goal of this project is to provide the following areas of functionality to openbalena via a web interface:
- Support for multiple organizations and users to openbalena
- Remote access to devices (ssh, vnc and http into host or containers)
- Fleet / device management (create fleets, add devices, manage variables / tags / labels)
- Support for creating and managing custom device types
- Remote device diagnostics

## Compatibility
This project is compatible with `open-balena-api` v0.139.0 or newer, all the way up to the current builds (v0.190.0).  See [this project](https://github.com/dcaputo-harmoni/open-balena-helm) for a fork of bartversluijs' open-balena-helm project which has helm scripts to build a current version of `open-balena`.

## Installation (Docker Compose)

**Note**: Skip steps 1 and 2 if you have a running instance of openbalena

1. Download open-balena
```sh
git clone https://github.com/balena-io/open-balena.git
```

2. Configure open-balena
```sh
open-balena/scripts/quickstart -U balena@openbalena.local -P balena
```
**Note**: When the script is complete, take note of the values of `OPENBALENA_JWT_SECRET` from `config/activate`, and `OPENBALENA_API_VERSION_TAG` from `compose/versions`

3. Download open-balena-admin
```sh
git clone https://github.com/dcaputo-harmoni/open-balena-admin.git
```

4. Configure open-blena-admin
```sh
open-balena-admin/scripts/quickstart -j [OPENBALENA_JWT_SECRET] -v [OPENBALENA_API_VERSION]
```
**Note**: If you did not complete steps 1 and 2 (i.e. you have a running instance of openbalena) you need to ssh into your running instance of open-balena-api, where you will find `OPENBALENA_JWT_SECRET` via the environment variable `JSON_WEB_TOKEN_SECRET`, and `OPENBALENA_API_VERSION` as "version" within `/usr/src/app/package.json`

5. Set up hostnames

If running locally, edit `/etc/hosts` or `C:\Windows\System32\Drivers\etc\hosts` to include:

```sh
127.0.0.1 openbalena.local
127.0.0.1 api.openbalena.local
127.0.0.1 registry.openbalena.local
127.0.0.1 vpn.openbalena.local
127.0.0.1 s3.openbalena.local
127.0.0.1 tunnel.openbalena.local
127.0.0.1 admin.openbalena.local
127.0.0.1 postgrest.openbalena.local
127.0.0.1 remote.openbalena.local
```
If hosted, set up the hostnames to point to your public IP addresses for the respective containers and/or ingress controllers.

6. Start open-balena
```sh
open-balena/scripts/compose up
```

7. Start open-balena-admin
```sh
open-balena-admin/scripts/compose up
```

## Web Access

Once both open-balena and open-balena-admin are running, you can access the admin interface via `http://admin.<openbalena domain>` (or `http://admin.openbalena.local` if running locally).  Log in using the credentials that were used in step 2 or when your openbalena instance was initially set up.

## Components

The open-balena-admin package is a compilation of three separate but related projects: [open-balena-ui](https://github.com/dcaputo-harmoni/open-balena-ui), [open-balena-remote](https://github.com/dcaputo-harmoni/open-balena-remote) and [open-balena-postgrest](https://github.com/dcaputo-harmoni/open-balena-postgrest).  Take note of the instructions within each of these projects to ensure your openbalena projects are configured to utilize features of open-balena-admin (i.e. remote device access).

## K8S Helm Charts

Helm charts are included in the `/helm` folder of this repository for K8S users, however given the variability of K8S environments, detailed instructions are not provided for these.  Take a look at [this fork](https://github.com/bartversluijs/open-balena) of openbalena by bartversluijs for relevant documentation as this was the inspiration / basis for these charts.

## Limitations and Known Issues
- User authorization is not implemented at the resource level
- API key creation via web is not working

## Credits

- Big credit to [bartversluijs](https://github.com/bartversluijs) for inspiring the helm portion of this project
- The [ra-data-postgrest](https://github.com/raphiniert-com/ra-data-postgrest) project was instrumental in establishing the link to the open-balena database
