Write how GRPO works

Use the following setup with vscode/lambdalabs
8*A100 GPUs
install notebook plugin
pip install uv
uv venv rl --python 3.11 && source rl/bin/activate && uv pip install --upgrade pip
cmd-shift-P, reload window

For me the goal is to take a Qwen/Qwen2.5-7B-Instruct model, which is highly capable model for coding and mathematics with No. of parameters - 7.61B and has 28 layers. Instruct model is ideal here since the model is capable of handling intructions (Q/A type). I use the huggingface library to get the pretrained model and use torch.bfloat16 for memory efficiency. Let me explain why bfloat16 and not 32,

Explain
float32: 1 sign bit + 8 exponent bits + 23 mantissa bits = 32 bits
bfloat16: 1 sign bit + 8 exponent bits + 7 mantissa bits = 16 bits
bfloat is known as brain float, while representing a value in bfloat16, the exponent remains the same, which means it can represent very large and small values in similar way to float16 without underflowing or overflowing which is a problem in float16 (has 5 exponent bits). And finally by requiring only 16 bits per element or value, it leads to reduced VRAM usage and is relatively now having faster memory transfers. Precision drop is within a very tolerable level for a model.

Once the model is downloaded using from_pretrained, i get the tokenizer using huggingface AutoTokenizer by passing the model and setting the padding side to left, padding_side="left" here means that when the tokenizer pads sequences to a certain length, it inserts the padding tokens at the beginning (left side) of the sequence rather than at the end (right side). Just to note, Many decoder-based models often require or work best with left padding.

Since we run on 8*A100's or 8*H100s, we get the number of GPUs and initialize the device id's. Following which we prepare the training dataset, shuffle the dataset for randomness, declare the size of our evaluation set to 30 examples and finally create the evaluation and training datasets.

It is extremely important to evaluate the eval set with current model before we finetune using GRPO, this will give us an accuracy index of how well the base model was able to handle these math problems before it was tuned. In our case we get around 63% accuracy, which is not very bad.

Before we proceed with model training, we optimize our model memory, lets talk about it a little. First thing we do in the optimize_model_memory is to set the model to training mode, 