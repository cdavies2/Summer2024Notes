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
# Falcon Models
* 
