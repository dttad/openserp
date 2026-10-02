# OpenSERP for SerpSmart

Config used on ws next to SerpSmart (`/opt/serpbear-prod/openserp.yaml`):
Google limited to 10 requests/min (one residential IP), cache 5 min, 3 Chrome processes.
The service runs as `openserp` on the SerpSmart compose network with no published port;
SerpSmart reaches it at `http://openserp:7000` (env `OPENSERP_URL`).
