# SimpleVoiceChatWeb – Browser Tools

Client-side helpers for **SimpleVoiceChatWeb**. Everything runs in the browser
tab: no server, no upload, no login.

- **Live:** <https://SimpleVoiceChatWeb.github.io/tools/>
- **Short domain:** <https://SimpleVCWeb.github.io/tools/>
- **Documentation:** <https://SimpleVCWeb.github.io/docs/tools.html>
- **Source:** [`SimpleVoiceChatWeb/webend`](https://github.com/SimpleVoiceChatWeb/webend) (`tools/`)

## Tools

| Route | Tool | Purpose |
|---|---|---|
| `#/` | Overview | pick a tool |
| `#/config` | Config editor | edit `config.yml` in a form and download valid JSON |
| `#/ports` | Ports & firewall | copy-paste rules for ufw, iptables and docker-compose |

Routing is hash-based, so every view is served by the same `index.html`.

### Config editor

Load your `plugins/SimpleVoiceChatWeb/config.yml` and change it as a form
instead of hand-editing JSON.

- **Only the options the plugin reads** are shown. Keys from older releases
  (`webrtc.port`, `web.corsAllowedOrigins`, `svc.*`, …) are removed on load and
  listed separately.
- **Unknown keys survive.** Options the editor does not know stay untouched and
  are listed, so a fork or a newer release does not lose data.
- **Comments survive value edits.** Adding or removing a key rewrites the file,
  and the editor says so before you download.
- **Validation runs live:** ports, the PeerJS host, TURN credentials and the
  empty JWT secret are checked against the same rules as the plugin.
- **Output is always JSON** with a four-space indent, even if you load YAML.
  The plugin reads it through Typesafe Config, which accepts both.

### Ports & firewall

Enter the Minecraft port and the web port; the tool prints the matching rules.

| Output | Use |
|---|---|
| `ufw` | Debian/Ubuntu hosts with ufw |
| `iptables` | Systems without ufw or with custom chains |
| `docker-compose` | Container setups that publish ports |
| Config values | The matching `web.port` line |

The web port needs **TCP** for signalling and **UDP** for voice. There is no
inbound WebRTC port – older releases wrote `webrtc.port`, but it never opened a
socket.

## Do not edit by hand

Every file except this `README.md` is generated. The
`Deploy Tools to the tools repository` workflow in
[`webend`](https://github.com/SimpleVoiceChatWeb/webend) builds the Vite app and
pushes `dist-tools/` into the root of `main` with `rsync --delete`. Changes made
here are overwritten on the next deploy; the README is excluded from that sync.

| Path | Purpose |
|---|---|
| `index.html` | Entry point, assets referenced under `/tools/` |
| `assets/` | Hashed JS/CSS bundles (one chunk per tool) |
| `favicon.svg` | Icon |
| `.nojekyll` | Disables Jekyll on GitHub Pages |

## GitHub Pages

- Source: **Deploy from a branch**, branch `main`, folder `/ (root)`
- Deploys on changes under `tools/` in `webend`
- Deploy key access is limited to the `webend` repository (`TOOLS_DEPLOY_KEY`)
