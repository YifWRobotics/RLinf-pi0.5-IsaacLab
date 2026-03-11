# pi 0.5 Isaac Lab on HPC Cluster

## pi 0.5 Env

```bash
mkdir -p ~/scratch/containers_rlinf_isaaclab_openpi_gr00t
cd ~/scratch/containers_rlinf_isaaclab_openpi_gr00t

apptainer pull rlinf_isaaclab_openpi_gr00t.sif \
  docker://yifwrobotics/rlinf:isaaclab-openpi-gr00t
```

## pi 0.5 Isaac Lab Franka Stack Cube Eval

Download model
```bash
cd outputs
hf download YifWRobotics/Mar7-pi-05-stack-cube-pytorch --local-dir RLinf-pi05-SFT-Stack-cube
```

Run eval
```bash
apptainer shell --nv \
  --bind ~/scratch/RLinf-pi0.5-IsaacLab:/workspace/RLinf \
  --bind ~/scratch/.cache:/root/.cache \
  ~/scratch/containers_rlinf_isaaclab_openpi_gr00t/rlinf_isaaclab_openpi_gr00t.sif

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

export EMBODIED_PATH=/workspace/RLinf/examples/embodiment

python /workspace/RLinf/examples/embodiment/eval_embodied_agent.py \
  --config-path /workspace/RLinf/examples/embodiment/config/ \
  --config-name isaaclab_franka_stack_cube_ppo_openpi_pi05 \
  runner.logger.log_path=/workspace/RLinf/logs/eval_$(date +%Y%m%d-%H%M%S) \
  rollout.model.model_path=/workspace/RLinf/outputs/RLinf-pi05-SFT-Stack-cube \
  actor.model.model_path=/workspace/RLinf/outputs/RLinf-pi05-SFT-Stack-cube
```

## pi0.5 Isaac Lab Franka Stack Cube PPO

```bash
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

export EMBODIED_PATH=/workspace/RLinf/examples/embodiment

python /workspace/RLinf/examples/embodiment/train_embodied_agent.py \
  --config-path /workspace/RLinf/examples/embodiment/config/ \
  --config-name isaaclab_franka_stack_cube_ppo_openpi_pi05 \
  runner.logger.log_path=/workspace/RLinf/logs/train_$(date +%Y%m%d-%H%M%S) \
  actor.micro_batch_size=4 \
  actor.global_batch_size=32 \
  runner.val_check_interval=-1 \
  algorithm.update_epoch=1 \
  algorithm.rollout_epoch=1 \
  rollout.model.model_path=/workspace/RLinf/outputs/RLinf-pi05-SFT-Stack-cube \
  actor.model.model_path=/workspace/RLinf/outputs/RLinf-pi05-SFT-Stack-cube
```