# What is a Large Language Model?
* A large language model (LLM) is a deep learning algorithm that can perform multiple natural language processing (NLP) tasks, like recognizing, translating, predicting, and generating text or content
* There are trained using massive datasets
* They are also referred as neural networks. They use a network of layered nodes
* Large language models have AI application, but can also be used for understanding protein structures or writing software code.
* LLMs' problem solving capabilities can be applied to many industries, like healthcare, finance, and entertainment. They can also be used for many NLP applications, like translation and chatbots.

## What is a Transformer Model?
* The transformer model is the most common architecture of large language models
*  It has an encoder and a decoder, and processes data by tokenizing input and conducting mathematical equations to find relationships between tokens.
*  Transformer models use self-attention mechanisms, which enable them to consider the entire context of a sequence when making predictions

## Key Components of Large Language Models
* The Embedding Layer-creates embeddings from input text. It captures the semantic and syntactic meaning of the input, so the model can understand context.
* The Feedforward Layer (FFN)-made up of multiple fully connected layers that transform input headings, thus enabling the model to glean higher-level abstractions (or understand the user's intent with text input)
* The Recurrent Layer-captures the relationship between words in a sentence
* The Attention Mechanism-enables a language model to focus on single parts of the input text that's relevant to the task at hand, allowing for more accurate outputs.

### Three Kinds of Large Language Models
* Generic or Raw Language Models-predict the next word based on language in the training data. They perform information retrieval tasks.
* Instruction-Tuned Language Models-trained to predict responses to the instructions given in input. They perform sentiment analysis, or generate text or code.
* Dialog-Tuned Language Models-trained to have a dialog by predicting the next response (chatbots or conversational AI)

## Large Language Models vs Generative AI
* All large language models are generative AI.
* Large language models are a type of generative AI that are trained on text and produce textual content. ChatGPT is a popular example of generative text AI

# How Do Large Language Models Work?
* Training-large language models are pre-trained using large textual datasets from sites like Wikipedia or GitHub. The large language model engages in unsupervised learning, meaning it processes datasets fed to it without specific instructions. The LLM can learn the meaning of words, the relationships between them, and how to distinguish words based on context.
* Fine-Tuning-fine tuning optimizes the performance of specific tasks
* Prompt-Tuning-trains a model to perform a specific task through few-shot prompting, or zero-shot prompting. A prompt is an instruction given to an LLM, and few-shot prompting teaches a model to predict outputs via examples.
  * EX: Sentiment Analysis
  *  Customer review: This plant is so beautiful!
    Customer sentiment: positive
  *   Customer review: This plant is so hideous!
    Customer sentiment: negative
* Zero-shot prompting doesn't use examples, instead clearly indicated which task the language model should perform (EX: "The sentiment in 'the plant is hideous' is...")

## Large Language Models Use Cases
* Information Retrieval-like Bing or Google, retrieves information and summarizes and communicates the answer in a conversational style.
* Sentiment Analyzsis-enables companies to analyze the sentiment of textual data
* Text Generation-like ChatGPT, generates text based on inputs (EX: Write a poem about trees in the style of Emily Dickinson)
* Code Generation-LLMs understand patterns, which enables them to generate code
* Chatbots and Conversational AI-enables customer service chatbots to engage with customers, interpret the meaning of their queries or responses, and respond in turn.

# Benefits of Large Language Models
* Large Set of Applications-language translation, sentiment analysis, question answering
* Always Improving-performance is continually improving because it grows when more data and parameters are added. The more it learns, the better it gets. They can also exhibit "in-context learning", since once an LLM's been pretrained, few-shot prompting enables the model to learn from the prompt without additional parameters.
* They Learn Fast-they don't require too many examples when demonstrating in-context learning

# Limitations and Challenges of Large Language Models
* Hallucinations-an LLM may produce at output that is false, like claiming that it is human or has emotions. Because large language models predict the next syntactically correct word or phrase, they can't wholly interpret human meaning
* Security-when not managed or surveilled property, LLMs can leak private information, participate in phishing scams, and produce spam. Users with malicious intent can reprogram AI to their biases and contribute to spread of misinformation
* Bias-if the training data represents a single demographic or lacks diversity, the outputs will also lack diversity
* Consent-LLMs are trained on trillions of datasets, and when scraping data from the internet, LLMs have been known to ignore copyright licenses, plagiarize written content, and repurpose content without getting permission from the original owners. When it produces results, there is no way to track data lineage, and often no credit is given to creators, leading to copyright infringement issues. They may also scrap personal data, which can compromise privacy
* Scaling-it can be difficult and time- and -resource consuming to scale and maintain large language models.
* Deployment-deploying large language models requires deep learning, a transformer model, distributed software and hardware, and overall technical expertise.

## Popular Large Language Models
* PaLM-Google's Pathways Language Model
* BERT-the Bidirectional Encoder Representations from Transformers model, also developed at Google
* XLNet-permutation language model, generates output predictions in a random order
* GPT-generative pre-trained transformers, developed by Open AI. It can be fine-tuned to perform specific tasks downstream.
* 



