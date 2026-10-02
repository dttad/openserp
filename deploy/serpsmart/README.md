# OpenSERP for SerpSmart

Config used on ws next to SerpSmart (`/opt/serpbear-prod/openserp.yaml`):
Google limited to 10 requests/min (one residential IP), cache 5 min, 3 Chrome processes.
The service runs as `openserp` on the SerpSmart compose network with no published port;
SerpSmart reaches it at `http://openserp:7000` (env `OPENSERP_URL`).

## Normal-machine mode (`Dockerfile.headful`)

Real Google Chrome, headful on Xvfb (1920×1080), WebGL on the host NVIDIA GPU through Vulkan
(`gpus: all`, `NVIDIA_DRIVER_CAPABILITIES=all`), vi_VN locale, Asia/Ho_Chi_Minh time zone, full font set,
persistent profile in the `openserp-profile` volume, images/fonts/CSS not blocked, Google lanes pinned to the
`chrome-linux-nvidia` profile (`profiles.json`), 4 Google requests/min.

New config keys added in this fork:
- `app.browser_args` — extra Chrome switches, `;`- or newline-separated (values may contain commas).
- `app.user_data_dir` — persistent Chrome profile directory (use with `max_processes: 1`).

Fingerprint check (2026-10-02): sannysoft 56/56, rebrowser 9/9, browserscan 3/3, pixelscan 104/105
(overall verdict still "bot"). Google still answered HTTP 429 to automated searches from the home IP, while a
plain request from the same IP got HTTP 200 — Google flags the automated session, not the IP as a whole.

If the ws Docker registry mirror (cr.mercat-qilin.ts.net) is down, BuildKit cannot resolve base images:
`docker pull` them (dockerd falls back to Docker Hub) and build with the classic builder from a copy of the
Dockerfile without `--platform=$BUILDPLATFORM`.
