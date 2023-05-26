# Text-simplification-final-project
Text simplification final project IDC 2023


# Text simplification
The Text Simplification task involves modifying the content and structure of a text to make it easier to read and understand while retaining the underlying meaning of the text. A simplified version of a text can benefit a broad range of readers. The main approaches to text simplification are Lexical Simplification and Syntactic Simplification.

In this Project, we aim to extend the conventional text simplification task by performing simplification at various reading levels. We accomplish this by training our language model on a quality data set constructed from multiple sources. The data set contains sentences at three different difficulty levels, enabling the model to simplify text across a range of reading levels. We ran the ,odel on relatively low compute resources(Google colab).

# Model
We used GPT-J 6B model. It is a language transformer model with 6 billion parameters and is publicly available in a pre-trained version. The model consists of 28 layers, with a model dimension of 4096 and a feed-forward dimension of 16384. The dimensions are divided into 16 heads, each with 256 dimensions. The model was trained with a vocabulary of 50,257, using the same set of BPEs as GPT2/GPT3. GPT-J was trained for 402 billion tokens over 383,500 steps on TPU v3-256. It was trained as an autoregressive language model, with the cross-entropy loss used for the loss function.
To run the GPT-J model on Google Colab, we had to make some adjustments, as the original model requires more than 22GB of memory for float32 parameters alone, not including the guardians and optimizer. Therefore, we applied three techniques to make GPT-J suitable for Colab, which could be used for fine-tuning and testing with 11GB available.
The three techniques we used are:
- Large weight tensors are quantized using dynamic 8-bit quantization and are de-quantized only before using them.
- Using gradient checkpoints two for one activation per layer. This method uses less memory at the cost of 30 percent slower training.
- Adding LoRa (Low-Rank Adaptation) layer to the model, which was designed by a group of Microsoft researchers in 2021. This method can reduce the number of trainable parameters for downstream tasks by freezing the pre-trained model weights and injecting trainable rank decomposition matrices into each layer of the Transformer architecture. LoRA can reduce the GPU memory requirement by 3 times and the trainable parameters by 10,000 compared to GPT-3 175B fine-tuned with Adam.

# Data
For the learning process, we utilized Prompt Engineering to teach the model the downstream task of text simplification at different levels. The simplification examples were based on three data sets:

1. OneStopEnglish corpus - this corpus comprises 189 texts written at three reading levels. Each text has three versions, totaling 567. The corpus is freely available on GitHub and also in this repo, you can also find the data post-processing version in this repo.

2. Simple Wiki - this dataset was generated by aligning Simple English Wikipedia and English Wikipedia. It contains 137K aligned sentence pairs based on those sentences that have a similarity above 0.50. You can find this data set and also the data post-processing version in this repo. 

3. Newsela data sets - the corpus consists of thousands of news articles professionally leveled to different reading complexities. It is the most extensive collection of professionally written simplifications available. From this corpus, we extracted 30,000 sentence-aligned pairs, We filters based on the evaluation method so we had left with few thousand at the end. Since this data is not publicly available, you can not find it in the repo, but you can find examples of it in the post-processing version. If needed it is possible to get the data by request from Newsela.

# Evaluation
To evaluate our model's performance, we utilized multiple evaluation metrics, including BLEU, SARI, Flesch Reading, and Flesch-Kincaid Grade Level.

BLEU is a precision-based metric that quantifies the proportion of n-gram matches between the model's output and the references. Initially designed for machine translation tasks, it has been shown to have a strong correlation with human judgments of grammatical and semantic accuracy.

SARI is another metric used to assess text simplification systems. It compares the model's predicted simplified sentences to the source and reference sentences, measuring the quality of words that were kept, deleted, or added by the system. SARI has been shown to correlate well with human judgments of simplicity gain.

Flesch Reading is a widely used method for evaluating text readability. This method scores the text on a scale from 1 to 100, where higher scores indicate easier readability and lower scores indicate greater difficulty. The formula was developed by Rudolf Flesch in 1940 and considers the total number of words, sentences, and syllables in the text.

Flesch-Kincaid Grade Level considers the same variables as Flesch Reading but outputs the US grade level suitable for the text. This score indicates the number of years of education required to comprehend the text.



