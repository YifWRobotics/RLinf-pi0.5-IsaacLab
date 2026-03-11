# GR00T n1.5 Isaac Lab on Local PC

## GR00T Env

```bash
bash requirements/install.sh embodied --model gr00t --env isaaclab --no-root
```

## GR00T Isaac Lab Franka Stack Cube Eval

Download model
```bash
cd outputs
hf download RLinf/RLinf-Gr00t-SFT-Stack-cube --local-dir RLinf-Gr00t-SFT-Stack-cube
```

```bash
cd isaac_sim
source ./setup_conda_env.sh
cd ..

source .venv/bin/activate

export EMBODIED_PATH=$(pwd)/examples/embodiment

python examples/embodiment/eval_embodied_agent.py \
  --config-path config \
  --config-name isaaclab_franka_stack_cube_ppo_gr00t \
  runner.only_eval=True \
  runner.logger.log_path=logs/eval_$(date +%Y%m%d-%H:%M:%S) \
  cluster.component_placement.env=0 \
  cluster.component_placement.rollout.placement=0 \
  cluster.component_placement.actor=0 \
  rollout.model.model_path=$(pwd)/outputs/RLinf-Gr00t-SFT-Stack-cube \
  actor.model.model_path=$(pwd)/outputs/RLinf-Gr00t-SFT-Stack-cube \
  env.train.total_num_envs=1 \
  env.eval.total_num_envs=1
```

## Docker

```bash
docker run -it --rm --gpus all \
  -v ~/Robotics/RLinf-pi0.5-IsaacLab:/workspace/RLinf \
  yifwrobotics/rlinf:isaaclab-openpi-gr00t

cd /workspace/RLinf
source /usr/local/bin/switch_env gr00t

cd isaac_sim
source ./setup_conda_env.sh
cd ..

export EMBODIED_PATH=$(pwd)/examples/embodiment

python examples/embodiment/eval_embodied_agent.py \
  --config-path "$EMBODIED_PATH/config" \
  --config-name isaaclab_franka_stack_cube_ppo_gr00t \
  runner.only_eval=True \
  runner.logger.log_path="$(pwd)/logs/eval_$(date +%Y%m%d-%H:%M:%S)" \
  cluster.component_placement.env=0 \
  cluster.component_placement.rollout.placement=0 \
  cluster.component_placement.actor=0 \
  rollout.model.model_path="$(pwd)/outputs/RLinf-Gr00t-SFT-Stack-cube" \
  actor.model.model_path="$(pwd)/outputs/RLinf-Gr00t-SFT-Stack-cube" \
  env.train.total_num_envs=1 \
  env.eval.total_num_envs=1
```