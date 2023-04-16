# TokenGenerationPipeline

Due to the revolution of LLM model in the world of AI, companies want to integrate its performances into their services. However the use of the API can be expensive and the models are  too large to fit on regular devices. The solution will be to split parameters over to disk and ram.

The main objective was to create a token generation pipeline for the following models: 
1. bigscience/bloom-7b1
2. EleutherAI/gpt-neox-20b
3. google/flan-ul2
4. cerebras/Cerebras-GPT-13B

We need to use the pacakges created from HuggingFace. The client is not looking for a perfect solution but for pipelines suceptible to work after code optimization - something to start with.

We need to have in mind that the pipeline need:
1. To load as much of the model into the GPU as possible, then load the remainder into CPU Ram.
2. To work on GPU google Colab T4 instance.

The solution is divided on several parts and parameters and we use some high level packages developed by HuggingFace based on the following documentations:
1. https://huggingface.co/blog/accelerate-large-models
2. https://huggingface.co/docs/accelerate/usage_guides/big_modeling
3. https://huggingface.co/docs/transformers/main/main_classes/quantization

Many aspects of the model have to be taken with consideration like the parameters format (float32, float16, int8), its architecture also. However, when you load a model directly with HuggingFace package, you load the architecture and the parameters. So, the main idea is to start with an empty model and make sure on which device to store each layer (submodule).

Here the main steps of the pipeline:
1. We define the checkpoint of the model we want to load.

2. We load the empty model 
-it contains the architecture only

3. We define the device mapping 
- it is a dictionary where keys are the name of the model's block and the values are the device to store it.

4. We configure a quantization object 
- it manages the format of model's weights during load but affects the performances.

5. We load the model form HuggingFace set up with the previous steps and an offload folder.

The last step is to run the model with a tokenizer.
