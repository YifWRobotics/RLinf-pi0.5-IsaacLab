# GR00T n1.5 Isaac Lab on HPC Cluster

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

export EMBODIED_PATH=/workspace/RLinf/examples/embodiment

python /workspace/RLinf/examples/embodiment/eval_embodied_agent.py \
  --config-path /workspace/RLinf/examples/embodiment/config/ \
  --config-name isaaclab_franka_stack_cube_ppo_gr00t \
  runner.logger.log_path=/workspace/RLinf/logs/$(date +%Y%m%d-%H:%M:%S) \
  cluster.component_placement.env=0 \
  cluster.component_placement.rollout.placement=0 \
  cluster.component_placement.actor=0 \
  rollout.model.model_path=/workspace/RLinf/outputs/RLinf-Gr00t-SFT-Stack-cube \
  actor.model.model_path=/workspace/RLinf/outputs/RLinf-Gr00t-SFT-Stack-cube
```

## GR00T Isaac Lab Franka Stack Cube PPO

