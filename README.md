# About
Targeted for Source2ZE


This is a docker build script for the steamrt container that allows for
running the client without steam and the server with/without steam.

**Credit: Von on Discord GFL (He wrote the entire setup and instructions i'm just uploading it)**

# Map List
Edit the maplist to include your target maps
```
artifacts/addons/cs2fixes/configs/maplist.jsonc.example
```
You can edit other files before buildtime to avoid a lengthy recompile, takes 30 minutes to build goldberg and download the game (cpu  + network)

# Usage
## Build
```bash
podman build $PWD --tag cs2-docker
```

```bash
docker buildx build $PWD --tag cs2-docker
```

you can pass `--arg LOGIN='username password'` to download the workshop tools (the free DLC requires a account)

## Run
### No GPU
```bash
docker compose up -d
```

### Generic Linux (GPU)
```bash
docker compose -f compose.yaml -f compose.linux-gpu.yaml up -d
```

### WSL (GPU)
```bash
docker compose -f -compose.yaml -f compose.wsl.yaml up -d
```

## Admin Management

Add an admin by Steam64 ID:
```bash
docker exec cs2-docker cs2fixes-add-admin.sh <uint steamID64 here> z
```

Flags: `z` = root/all permissions, `oy` = generic admin + chat (default)

Installation for [WSL](https://learn.microsoft.com/en-us/windows/wsl/install)

## Game Connect Example
```
ipv4:   connect 255.255.255.255
P2P/SDR: connect "[A:1:0 (steam64):0 (vport)]"
```

You can get the P2P/steam3ID of the server by running `status_json` in the server console

# Misc
## Workshop Tools, Disable 2-auth
https://store.steampowered.com/twofactor/manage

## Enable/Update Nvidia GPU support
```
xhost +local:docker
sudo nvidia-ctk runtime configure --runtime=docker && sudo systemctl daemon-reload && sudo systemctl restart docker
```
