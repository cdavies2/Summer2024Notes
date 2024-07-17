# Llama3
* Llama3 is Meta's open-source large language model.
* Llama3 has 8B and 70B parameters, and has a decoder-only transformer architecture.
* Llama3 uses a tokenizer with a vocabulary of 128K tokens.
* Llama3 models are trained on sequences of 8,192 tokens, using a mask to ensure self-attention doesn't cross document boundaries.
* Llama3 is pretrained on over 15T tokens that were collected from publicly available sources. Over 5% of the Llama3 pretraining dataset consists of high-quality non-English data that covers over 30 languages, but the model has its highest level of performance with English data.
* To ensure that Llama3 was trained on data of the highest quality, several data-filtering pipelines were implemented, including heuristic filters, NSFW filters, and text classifiers
* A series of detailed scaling laws were developed for downstream benchmark evaluations. These scaling laws allow us to select an optimal data mix and make informed decisions on how to best user our training computer. They also alow the performance of models on key tasks to be predicted before said models are actually trained.
* To train Llama3 models, three types of parallelization were combined....
    * Data Parallelization
    * Model Parallelization
    * Pipeline Parallelization
*  The developer's appraoch to post-training is a combination of supervised fine tuning (SFT), rejection sampling, proximal policy optimization (PPO), and direct preference optimization (DPO). The quality of prompts used in SFT and preference rankings that are used in PPO and DPO has an outsized influence on the performance of aligned models. Learning from preference rankings via PPO and DPO also greatly improved the performance of Llama3 on reasoning and coding tasks.
*  Source:https://ai.meta.com/blog/meta-llama-3/
## Tokens Used with Meta Llama3
* <|begin_of_text|>: This is equivalent to the BOS token
* <|eot_id|>: this signifies the end of the message in a turn.
* <|start_header_id|>{role}<|end_header_id|>: These tokens enclose the role for a particular message. The possible roles are system, user, and assistant.
* <|end_of_text|>: This is equivalent to the EOS token, On generating this token, Llama 3 will cease to generate more tokens.
* A prompt can optionally contain a single system message, or multiple alternating user and assitant messages, but always ends with the last user message followed by the assistant header.
## Meta LLama 3 Instruct
* Newlines (0x0A) are part of the prompt format.
* The model expects the assistant header at the end of the prompt to start completing it.
* Below is an example instruct prompt:
```
<|begin_of_text|><|start_header_id|>system<|end_header_id|>

You are a helpful AI assistant for travel tips and recommendations<|eot_id|><|start_header_id|>user<|end_header_id|>

What can you help me with?<|eot_id|><|start_header_id|>assistant<|end_header_id|>
```
*  <|begin_of_text|>: Specifies the start of the prompt
*  <|start_header_id|>system<|end_header_id|>: specifies the role for the following message, i.e. "system"
*  You are a helpful AI assistant for travel tips and recommendations: The system message
*  <|eot_id|>: Specifies the end of the input message
*  <|start_header_id|>user<|end_header_id|>: Specifies the role for the following message i.e. “user”
*  What can you help me with?: The user message
*  <|start_header_id|>assistant<|end_header_id|>: Ends with the assitant header, to prompt the model to start generation.
*  Following this prompt, Llama 3 completes it by generating the {{assistant_message}}. It signals the end of the {{assistant_message}} by generating the <|eot_id|>
*  This is an example prompt with a single user message:
```
<|begin_of_text|><|start_header_id|>user<|end_header_id|>

What is France's capital?<|eot_id|><|start_header_id|>assistant<|end_header_id|>
```
*  System prompt and multiple turn conversation between the user and assistant
```
<|begin_of_text|><|start_header_id|>system<|end_header_id|>
You are a helpful AI assitant for travel tips and recommendations<|eot_id><|start_header_id|>user<|end_header_id|>
What is France's capital <|eot_id|>
<|start_header_id|>assistant<|end_header_id|>
Bonjour! The capital of France is Paris! <|eot_id|>
<|start_header_id>user<|end_header_id|>
What can I do there?<|eot_id|>
<|start_header_id|>assistant<|end_header_id|>

Paris, the City of Light, offers a romantic getaway with must-see attractions like the Eiffel Tower and Louvre Museum, romantic experiences like river cruises and charming neighborhoods, and delicious food and drink options, with helpful tips for making the most of your trip.<|eot_id|><|start_header_id|>user<|end_header_id|>
Give me a detailed list of the attractions I should visit, and time it takes in each one, to plan my trip accordingly.<|eot_id|><|start_header_id|>assistant<|end_header_id|>
```
* Source: https://llama.meta.com/docs/model-cards-and-prompt-formats/meta-llama-3/
## Running Llama 3 with JavaScript
* Install Replicate's Node.js client library with the command below
```
npm install replicate
```
*  Set the REPLICATE_API_TOKEN environment variable
```
export REPLICATE_API_TOKEN=<paste-your-token-here>
```
*  Import and set up the client
```
import Replicate from "replicate";

const replicate = new Replicate({
  auth: process.env.REPLICATE_API_TOKEN,
});
```
*  Run meta/meta-llama-3-70b-instruct using Replicate's API.
```
const input = {
  prompt: "Can you write a poem about open source machine learning?"
};

for await (const event of replicate.stream("meta/meta-llama-3-70b-instruct", { input })) {
  process.stdout.write(event.toString());
};
```
## Running Llama 3 with Python
* Install Replicate's Python client library
```
pip install replicate
```
*  Set the REPLICATE_API_TOKEN environment variable
```
export REPLICATE_API_TOKEN=<paste-your-token-here>
```
* Import the client
```
import replicate
```
* Run meta/meta-llama-3-70b-instruct using Replicate's API.
```
# The meta/meta-llama-3-70b-instruct model can stream output as it's running.
for event in replicate.stream(
    "meta/meta-llama-3-70b-instruct",
    input={
        "prompt": "Can you write a poem about open source machine learning?"
    },
):
    print(str(event), end="")
```
* Source: https://replicate.com/blog/run-llama-3-with-an-api?input=nodejs#running-llama-3-with-javascript
# Falcon Models
* Falcon LLM is a generative large language model from the Technology Innovation Institute (TII).
* Falcon2-11B is an 11B parameters causal decoder-only model built by TII and trained over 5,000B tokens of RefinedWeb enhanced with curated corpora. It is made available under the TII Falcon License 2.0.
* It is a raw, pretrained model, which should be further finetuned for most usecases.
## How to Get Started with the Model
```
from transformers import AutoTokenizer, AutoModelForCausalLM
import transformers
import torch

model = "tiiuae/falcon-11B"

tokenizer = AutoTokenizer.from_pretrained(model)
pipeline = transformers.pipeline(
    "text-generation",
    model=model,
    tokenizer=tokenizer,
    torch_dtype=torch.bfloat16,
    device_map="auto",
)
sequences = pipeline(
   "Can you explain the concepts of Quantum Computing?",
    max_length=200,
    do_sample=True,
    top_k=10,
    num_return_sequences=1,
    eos_token_id=tokenizer.eos_token_id,
)
for seq in sequences:
    print(f"Result: {seq['generated_text']}")

```
## Training Details
* Falcon2-11B was trained over 5,000B tokens of RefinedWeb, a high-quality filtered and deduplicated web dataset which was enhanced with curated corpora.
* Overall, the data sources included RefinedWeb-English, Refined Web-Europe (cs, de, es, fr, it, nl, sv), high quality technical data, code data, and conversational data extracted from public sources.
## Model Architecture and Objective
* Falcon2-11B is a causal decoder-only model trained on a causal language modeling task (i.e., predict the next token).
* Source: https://huggingface.co/tiiuae/falcon-11B
