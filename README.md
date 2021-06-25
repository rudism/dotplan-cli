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

- `dotplan register` will register a new account using the email and password in your `~/.dotplan.conf.json` file
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
