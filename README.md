**Note:** Active development has moved to https://code.sitosis.com/rudism/dotplan-cli

# Dotplan CLI

A shell script to interact with [dotplan](https://github.com/rudism/dotplan-online) providers.

## Requirements

- [curl](https://curl.se/)
- [jq](https://stedolan.github.io/jq/)
- [drill](https://www.nlnetlabs.nl/projects/ldns/about/) or [dig](https://www.isc.org/bind/)
- Optional for verifying or publishing signed dotplans: [minisign](https://jedisct1.github.io/minisign/)

Install these using your distribution's package manager, or compile from source if your distribution does not provide recent versions.

## Basic Usage

- `dotplan email@example.com` will print the dotplan for `email@example.com` as plain text
- `dotplan email@example.com PUBLIC_MINISIGN_KEY` will verify the signed dotplan for `email@example.com` using the provided public `minisign` key before printing it

## Advanced Usage

`dotplan-cli` can be used to register an account and publish dotplans on [Dotplan Online](https://dotplan.online) or other providers running compatible implementations.

Configure your email and password in `~/.dotplan.conf.json`. Make sure this file is read-protected after you create it (`chmod 0600 ~/.dotplan.conf.json`):

```json
{
  "auth": {
    "email": "user@example.com",
    "password": "my-password-123"
  }
}
```

- `dotplan register` will prompt for an email address and password and attempt to register it for you, optionally saving it to your `~/.dotplan.conf.json` file
- `dotplan publish` will sign (if `minisign` is available) and publish your `~/.plan` file to your dotplan provider
- `dotplan edit` will open your `~/.plan` file in an editor, then sign (if `minisign` is available) and publish it to your dotplan provider after you save and exit

## Optional Configuration

If you set `auth.provider` in your `~/.dotplan.conf.json` file, the `register`, `publish`, and `edit` commands will use that instead of discovering via `SRV` records. If you set `relayProvider`, all dotplans will be fetched from there instead of discovering via `SRV` records.

```json
{
  "auth": {
    "email": "user@example.com",
    "password": "my-password-123",
    "provider": "https://dotplan.online"
  },
  "relayProvider": "https://dotplan.online"
}
```

Several other aspects can be configured via environment variables:

- `DOTPLAN_CONFIG_PATH`: the config file to read and write (`$HOME/.dotplan.conf.json`)
- `DOTPLAN_MINISIGN_PRIVATE_KEY` the location of your private key (`$HOME/.minisign/minisign.key`)
- `DOTPLAN_PLAN_PATH` the location of your plan for the `publish` and `edit` commands (`$HOME/.plan`)
- `DOTPLAN_CURL_PATH` to specify the location of `curl`
- `DOTPLAN_JQ_PATH` to specify the location of `jq`
- `DOTPLAN_DRILL_PATH` to specify the location of `drill` or `dig`
- `DOTPLAN_MINISIGN_PATH` to specify the location of `minisign`
