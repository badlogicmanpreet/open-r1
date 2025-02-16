1. Create ssh key pair
   ssh-keygen -t ed25519 -C "mghotra81@gmail.com"
   eval "$(ssh-agent -s)"
   ssh-add ~/.ssh/id_ed25519
   cat ~/.ssh/id_ed25519.pub
   copy and add to github account

2. git clone git@github.com:badlogicmanpreet/open-r1.git
   git checkout dev

3. commands
    pip install uv
    uv venv openr1 --python 3.11 && source openr1/bin/activate && uv pip install --upgrade pip --link-mode=copy
    uv pip install vllm==0.7.2 --link-mode=copy

    uv pip install setuptools --link-mode=copy
    GIT_LFS_SKIP_SMUDGE=1 uv pip install -e ".[dev]" --link-mode=copy --no-build-isolation

    GIT_LFS_SKIP_SMUDGE=1 uv pip install -e ".[dev]" --link-mode=copy

    if anything fails, https://github.com/huggingface/open-r1/issues/282

    pip install tensorboard
    pip install wandb

    python -c "import torch; print(torch.__version__)"
    python -c 'import deepspeed; print("DeepSpeed is working!")'
    python -c 'import flash_attn; print("FlashAttention is working!")'

    huggingface-cli login
    wandb login

    git-lfs --version
    sudo apt-get install git-lfs

    ACCELERATE_LOG_LEVEL=info accelerate launch --config_file recipes/accelerate_configs/zero2.yaml \
    --num_processes=7 src/open_r1/grpo.py \
    --config recipes/Qwen2.5-Math-7B/grpo/config_simple_rl.yaml

    cat ~/.cache/huggingface/token
    Add huggingface token to run_r1_grpo.py

    ACCELERATE_LOG_LEVEL=info accelerate launch --config_file recipes/accelerate_configs/zero2.yaml \
    --num_processes=7 training/run_r1_grpo.py \
    --config training/grpo-qwen-2.5-3b-deepseek-r1-countdown.yaml

    ACCELERATE_LOG_LEVEL=info accelerate launch --config_file training/deepspeed_zero2.yaml \
    --num_processes=7 training/run_r1_grpo.py \
    --config training/grpo-qwen-2.5-3b-deepseek-r1-countdown.yaml

    ======================== DEBUG ========================

    export VLLM_WORKER_MULTIPROC_METHOD=spawn
    upgrading GCC to 12