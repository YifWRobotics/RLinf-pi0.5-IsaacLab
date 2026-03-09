# GR00T n1.5 Isaac Lab

## GR00T Env

```bash
mkdir -p ~/scratch/containers_rlinf_gr00t
cd ~/scratch/containers_rlinf_gr00t

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
  ~/scratch/containers_rlinf_gr00t/rlinf_isaaclab.sif

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

# Pi 0.5 Env

```bash
mkdir -p ~/scratch/containers_rlinf_openpi
apptainer overlay create --size 30720 ~/scratch/containers_rlinf_openpi/overlay.img

apptainer shell --nv \
  --overlay ~/scratch/containers_rlinf_openpi/overlay.img \
  --bind ~/scratch/RLinf-pi0.5-IsaacLab:/workspace/RLinf \
  --bind ~/scratch/.cache:/root/.cache \
  ~/scratch/containers_rlinf_gr00t/rlinf_isaaclab.sif

cd /workspace/RLinf

export XDG_CACHE_HOME=/root/.cache
export UV_CACHE_DIR=/root/.cache/uv
export PIP_CACHE_DIR=/root/.cache/pip
export TMPDIR=/tmp

bash requirements/install.sh embodied \
  --model openpi \
  --env isaaclab \
  --venv /opt/venv/openpi \
  --no-root
```

## Pi 0.5 Isaac Lab Franka Stack Cube Eval

Download model
```bash
cd outputs
hf download YifWRobotics/Mar7-pi-05-stack-cube-pytorch --local-dir RLinf-pi-0.5-SFT-Stack-cube
```


```bash
apptainer shell --nv \
  --overlay ~/scratch/containers_rlinf_openpi/overlay.img \
  --bind ~/scratch/RLinf-pi0.5-IsaacLab:/workspace/RLinf \
  --bind ~/scratch/.cache:/root/.cache \
  ~/scratch/containers_rlinf_gr00t/rlinf_isaaclab.sif

source /usr/local/bin/switch_env openpi

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
bash examples/embodiment/eval_embodiment.sh isaaclab_franka_stack_cube_ppo_openpi
```

## Pi 0.5 Isaac Lab Franka Stack Cube PPO