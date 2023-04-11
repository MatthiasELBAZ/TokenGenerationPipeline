# TokenGenerationPipeline

Due to the revolution of LLM model in the world AI, companies want to integrate their powers into their services. However the use of the API can be expensive and the models are  too large to fit on GPU. The solution will be to spill parameters over to disk and ram.

The main objective was to create a token generation pipeline for the following models: 
1. bigscience/bloom-7b1
2. EleutherAI/gpt-neox-20b
3. google/flan-ul2
4. cerebras/Cerebras-GPT-13B

Requirements:
Load as much of the model into the GPU as possible, then load the remainder into CPU Ram.
Use GPU google Colab T4 instance.

The solution is divided on several parts and parameters and we use some high level packages developed by HuggingFace.

1. We define the checkpoint of the model we want to load.

2. We load the empty model 
-it contains the architecture only

3. We define the device mapping 
- it is a dictionary where keys are the name of the model's block and the values are the device to store it.

4. We configure a quantization object 
- it manages the format of model's weights during load but affects the performances.

5. We load the model form HuggingFace set up with the previous steps and an offload folder.

The last step is to run the model with a tokenizer.
