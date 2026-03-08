# GR00T n1.5 Isaac Lab

```bash
bash requirements/install.sh embodied --model gr00t --env isaaclab --venv .venv_gr00t
```

## GR00T Isaac Lab Franka Stack Cube Eval

Run eval
```bash
source .venv_gr00t/bin/activate
cd isaac_sim
source ./setup_conda_env.sh
cd ..
bash examples/embodiment/eval_embodiment.sh isaaclab_franka_stack_cube_ppo_gr00t
```

# Pi 0.5 Isaac Lab

```bash
bash requirements/install.sh embodied --model openpi --env isaaclab --venv .venv_openpi
```

## Pi 0.5 Isaac Lab Franka Stack Cube Eval

```bash
source .venv_openpi/bin/activate
cd isaac_sim
source ./setup_conda_env.sh
cd ..
bash examples/embodiment/eval_embodiment.sh isaaclab_franka_stack_cube_ppo_openpi
```