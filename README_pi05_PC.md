# Pi 0.5 Isaac Lab on Local PC

## Pi 0.5 Env

```bash
bash requirements/install.sh embodied --model openpi --env isaaclab --no-root
```

## Pi 0.5 Isaac Lab Franka Stack Cube Eval

Download model
```bash
cd outputs
hf download YifWRobotics/Mar7-pi-05-stack-cube-pytorch --local-dir RLinf-pi-0.5-SFT-Stack-cube
```

```bash
cd isaac_sim
source ./setup_conda_env.sh
cd ..

source .venv/bin/activate

export EMBODIED_PATH=$(pwd)/examples/embodiment

python examples/embodiment/eval_embodied_agent.py \
  --config-path config \
  --config-name isaaclab_franka_stack_cube_ppo_openpi_pi05 \
  runner.only_eval=True \
  runner.logger.log_path=logs/$(date +%Y%m%d-%H:%M:%S) \
  rollout.model.model_path=$(pwd)/outputs/Mar7-pi-05-stack-cube-pytorch \
  actor.model.model_path=$(pwd)/outputs/Mar7-pi-05-stack-cube-pytorch \
  env.train.total_num_envs=1 \
  env.eval.total_num_envs=1
```