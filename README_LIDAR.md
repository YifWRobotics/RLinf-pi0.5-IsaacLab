# GR00T n1.5 Isaac Lab

```bash
mkdir -p ~/scratch/containers_rlinf
cd ~/scratch/containers_rlinf

apptainer pull rlinf_isaaclab.sif docker://rlinf/rlinf:agentic-rlinf0.1-isaaclab
```

## GR00T Isaac Lab Franka Stack Cube Eval

Download model
```bash
cd outputs
hf download RLinf/RLinf-Gr00t-SFT-Stack-cube --local-dir RLinf-Gr00t-SFT-Stack-cube
```

Run eval
```bash
apptainer shell --nv \
  --bind ~/scratch/RLinf-pi0.5-IsaacLab:/workspace/RLinf \
  --bind ~/scratch/.cache:/root/.cache \
  ~/scratch/containers_rlinf/rlinf_isaaclab.sif

source /usr/local/bin/switch_env gr00t

export XDG_CACHE_HOME=/root/.cache
export HF_HOME=/root/.cache/huggingface
export TORCH_HOME=/root/.cache/torch
export UV_CACHE_DIR=/root/.cache/uv
export PIP_CACHE_DIR=/root/.cache/pip

export SSL_CERT_FILE=/etc/ssl/certs/ca-certificates.crt
export REQUESTS_CA_BUNDLE=/etc/ssl/certs/ca-certificates.crt
export CURL_CA_BUNDLE=/etc/ssl/certs/ca-certificates.crt
export AWS_CA_BUNDLE=/etc/ssl/certs/ca-certificates.crt

cd /workspace/RLinf/isaac_sim
source ./setup_conda_env.sh

cd /workspace/RLinf
ray stop --force || true
bash examples/embodiment/eval_embodiment.sh isaaclab_franka_stack_cube_ppo_gr00t
```

## GR00T Isaac Lab Franka Stack Cube PPO









# Pi 0.5 Isaac Lab

```bash
mkdir -p ~/scratch/.cache/uv
mkdir -p ~/scratch/.cache/pip
mkdir -p ~/scratch/.cache/huggingface
mkdir -p ~/scratch/.cache/torch

export XDG_CACHE_HOME=~/scratch/.cache
export UV_CACHE_DIR=~/scratch/.cache/uv
export PIP_CACHE_DIR=~/scratch/.cache/pip
export HF_HOME=~/scratch/.cache/huggingface
export TORCH_HOME=~/scratch/.cache/torch

source .venv_openpi/bin/activate

uv pip uninstall cmake
uv pip install --force-reinstall "cmake<4"

hash -r
which cmake
cmake --version

export CMAKE_ARGS="-DCMAKE_POLICY_VERSION_MINIMUM=3.5"
uv pip install --no-build-isolation --force-reinstall egl-probe==1.0.2

bash requirements/install.sh embodied --model openpi --env isaaclab --venv .venv_openpi --no-root
```

## Pi 0.5 Isaac Lab Franka Stack Cube Eval

Download model
```bash
cd outputs
hf download YifWRobotics/Mar7-pi-05-stack-cube-pytorch --local-dir RLinf-pi-0.5-SFT-Stack-cube
```


```bash
source .venv_openpi/bin/activate
cd isaac_sim
source ./setup_conda_env.sh
cd ..
bash examples/embodiment/eval_embodiment.sh isaaclab_franka_stack_cube_ppo_openpi
```

