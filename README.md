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
2. Comment the `snapshot_download()` methods to use your local shared models:
```python
# snapshot_download(repo_id="VAST-AI/TripoSG", local_dir=triposg_weights_dir)
# snapshot_download(repo_id="briaai/RMBG-1.4", local_dir=rmbg_weights_dir)
# snapshot_download(repo_id="VAST-AI/TripoSG-scribble", local_dir=triposg_scribble_weights_dir)
```
3. In the `.devcontainer/devcontainer.json` file, update the mounts configuration with your models data root:
```json
"mounts": [
  "source=${localEnv:HOME}/Path/To/My/Models,target=/home/vscode/TripoSG/pretrained_weights,type=bind,readonly"
]
```
4. Navigate to TripoSG and run the demo generation:
```bash
cd TripoSG && python -m scripts.inference_triposg --image-input assets/example_data/hjswed.png --output-path ./output.glb
```

## Building and running the image manually
```bash
docker build . -t triposg-devcontainer -f .devcontainer/Dockerfile
docker run -it --name triposg --gpus all --mount type=bind,src=.,dst=/home/vscode --mount type=bind,src=$HOME/Path/To/My/Models,dst=/home/vscode/TripoSG/pretrained_weights,ro triposg-devcontainer
pip install diso --no-build-isolation
cd /home/vscode/TripoSG && python -m scripts.inference_triposg --image-input assets/example_data/hjswed.png --output-path ./output.glb
```

## Performance comparison
You can get the total elapsed time of a 3D model generation using the following command:

```bash
time python -m scripts.inference_triposg --image-input assets/example_data/hjswed.png --output-path ./output.glb
```

I've benchmarked this project on multiple graphic cards and put the results in the following table. The total time corresponds to the "real" time value outputted from the `time` command.

### assets/example_data/hjswed.png
| Card          | Time      |
|---------------|-----------|
| RTX 3060 12GB | 1m17.856s |
| RTX 4070m 8GB | 0m56.743s |

### assets/example_data/iuvbww.png
| Card          | Time      |
|---------------|-----------|
| RTX 3060 12GB | 1m20.531s |
| RTX 4070m 8GB | 1m0.724s  |

### assets/example_data/jkghed.png
| Card          | Time      |
|---------------|-----------|
| RTX 3060 12GB | 1m24.487s |
| RTX 4070m 8GB | 1m3.433s  |

### assets/example_data/qwvfsd.png
| Card          | Time      |
|---------------|-----------|
| RTX 3060 12GB | 1m34.897s |
| RTX 4070m 8GB | OOM       |

### assets/example_data/uegvsd.png
| Card          | Time      |
|---------------|-----------|
| RTX 3060 12GB | 1m18.112s |
| RTX 4070m 8GB | 0m58.096s |

### assets/example_data/uihghj.png
| Card          | Time      |
|---------------|-----------|
| RTX 3060 12GB | 1m5.337s  |
| RTX 4070m 8GB | 0m49.606s |

### assets/example_data/yuyyuw.png
| Card          | Time      |
|---------------|-----------|
| RTX 3060 12GB | 1m25.479s |
| RTX 4070m 8GB | 1m3.598s  |

### assets/example_data/yywdsd.png
| Card          | Time      |
|---------------|-----------|
| RTX 3060 12GB | 1m3.119s  |
| RTX 4070m 8GB | 0m47.198s |
