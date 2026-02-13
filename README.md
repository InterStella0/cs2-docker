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
```bash
docker compose up -d
```

```bash
docker exec cs2-docker cs2fixes-add-admin.sh <uint steamID64 here> z
```

Generic linux, might need to pass xauth and gpu specific varaibles
```
--gpus all \
-v /tmp/.X11-unix/:/tmp/.X11-unix/ \
-e DISPLAY=$DISPLAY
```

WSL drivers
```
--gpus all \
-v /usr/lib/wsl:/usr/lib/wsl:ro \
--device /dev/dxg \
-e LD_LIBRARY_PATH=/usr/lib/wsl/lib \
-e MESA_D3D12_DEFAULT_ADAPTER_NAME="NVIDIA" \
-e EGL_PLATFORM=surfaceless \
-e MESA_LOADER_DRIVER_OVERRIDE=d3d12 \
-e GALLIUM_DRIVER=d3d12 \
-e DISPLAY=$DISPLAY
```

Use `--rm` if you don't want to save workshop, demos, and shaders

Installation for [WSL](https://learn.microsoft.com/en-us/windows/wsl/install)

# Misc
## Workshop Tools, Disable 2-auth
https://store.steampowered.com/twofactor/manage

## Enable/Update Nvidia GPU support
```
xhost +local:docker
sudo nvidia-ctk runtime configure --runtime=docker && sudo systemctl daemon-reload && sudo systemctl restart docker
```
