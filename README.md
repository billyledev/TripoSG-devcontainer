# TripoSG-devcontainer

This repository offers a simple way to run TripoSG in a Docker environment.

## Requirements
* Docker ([rootless](https://docs.docker.com/engine/security/rootless) is even better)
* [NVIDIA Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html)
* VSCode with the [Dev Containers](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers) extension installed

## Usage
1. Pull TripoSG submodule:
```bash
git submodule update --init --recursive
```
2. In the `.devcontainer/devcontainer.json` file, update the mounts configuration with your models data root:
```json
"mounts": [
  "source=${localEnv:HOME}/Path/To/My/Models,target=/models,type=bind,readonly"
]
```
3. Build the devcontainer and then run the following command inside it to finish the setup:
```bash
pip install diso --no-build-isolation
```
4. Navigate to TripoSG and run the demo generation:
```bash
cd TripoSG && python -m scripts.inference_triposg --image-input assets/example_data/hjswed.png --output-path ./output.glb
```
