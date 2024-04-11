# Tokens

## Introduction

What is the relationship between tokens and prompts? Prompts are user inputs / text that will be send towards the model. Tokens are basically chunks from the user inputs, in which the the whole text are split up approximately 4~5 characters == 1 token (varies depending on implementation)

```bash
"Hello, world!" == ["Hello", ",", " world", "!"]
```

## Why Is It Important?

LLMs are not human that can interpret words however, they are amazing at interpreting numbers and associating objects to values. It will be inefficient to generate a long token value if a word could be broken up.

```bash
extrapolate == ["extrapolate"], can be used as a single token but the numerical value will be much higher
extrapolate == ["extr", "apol", "ate"], numerical values are smaller which can use algorithm to combine context of these values
```

Having smaller numerical value (ID) would be easier to perform calculation in vectorised formats. This results in faster processing for predictions of next chunk (in numerical value). Accuracy of prediction will be dependent on algorithms that handle context between each numerical value. Details will be discussed further in Transformers for Deep Learning.

A vocabulary (think of it as database) is created based on prefix number of tokens which will be used for contextual and semantic linking of tokens. Transformers will make use of this vocabulary to perform matrix calculation. As such, **bigger vocabulary == more computational power** needed thus, limiting vocabulary size is necessary. At the same time, vocabulary size limiting means any tokens not within vocabulary may result in **loss of information** during contextual linking.

## Types of Token

### Word-based

Splits inputs based on delimter(s) or RegEx (punctuations and rule-based are subsets). Different libraries implementation `NLTK`, `spaCy`, `Keras`, `Gensim` in python to use.

Punctuations can make model training difficult as `coffee? != coffee. != coffee`. Variations of same word would exponentially increase, causing processing time for model training and testing.

### Character-based

Splits word into individual characters to have smaller vocabulary and require fewer tokens compared to [word-based](#word-based) since English language has 256 differenct characters and ~170,000 words.

Advantages:
* Very little to no unknown words for vocabulary
* Misspelled words can be corrected to known words and prevent loss of information

Disadvantages:
* A character does not carry semantic menaing
* Each word would carry more tokens than other types `knowledge == 9 tokens`

### Subword-Based

Aims to address issues of [word-based](#word-based) and [character-based](#character-based)

#### Principles
1. Do not split frequently used words into smaller subwords
2. Split rare words into smaller meaningful words

```bash
tokenization == ["token", "ization"]
```

"token" becomes root word while "ization" is additional information to append to the root word. This improves model efficiency by giving different words meaning to add onto the root word if inputed. Further improvements can be done:

```bash
tokenization == ["token", "##ization"]
```

Using special symbols can help dictate the suffix or prefix of the root word to ensure accuracy towards the vocabulary and that there is no loss or misinterpreted information. Model can process unknown words by decomposing input into known subwords.

#### Types

* [BERT](https://arxiv.org/pdf/1810.04805.pdf)
* [Unigram](https://arxiv.org/pdf/1804.10959.pdf)
* [Byte-Pair Encoding](https://arxiv.org/pdf/1508.07909.pdf) by GPT
* [Roberta](https://arxiv.org/pdf/1907.11692.pdf) 
* [SentencePiece](https://arxiv.org/pdf/1808.06226.pdf)

## Token Limiting

As mentioned previously, depending on the word, the tokens will vary. But does that mean you can have unlimited tokens for each input? No, too much tokens will hog up the model processing as it requires even more computational resource to fully compute all tokens. Token limitation per input (and additional factors) will be added to allow model to run efficiently and reliably over long periods.

### Truncation

Clip words from either ends but model will suffer loss of information due to missing context.

```python
def truncate(long_text: str, i: int):
    return ‘ ’.join(long_text.split()[:-i])
txt = "This is probably a long sentence!"
short_text = truncate(txt, 3)
print(short_text)
```
```bash
>>> print(short_text)
This is probably
>>>
```

### Chunk Processing

Breaking text into smaller chunks for processing and results concatenated to form a single output. Final result may be prone to errors due to potential loss of information if chunking cuts apart the word incorrectly.

```bash
This is probably a long sentence! == "This is prob" + "ably a long" + " sentence!"
```

### Summarize

Long text may not add value to overall meaning. Summarise input to maximise value.

```bash
Long_text = “It’s such a fine day today, The sun is out, and the sky is blue. Can you tell me what the weather will be like tomorrow?”
Short_text = “It's sunny today. What will the weather be like tomorrow?”
Shorter_text = “Tell me the weather forecast for tomorrow”
```

### Remove Redundant Terms

Stop word removal technique is used in NLP such as "to" and "the".

```bash
“Weather forecast tomorrow Texas”
```

Not reliable in complex sentences due to missing context so manual verification or using another algorithm to better retain the meaning before perform this technique.

### Fine-tuning Language Models

Use existing weights of model to continue training with specific data to improve the model processing.

## Interview Questions

* Could you explain some of the tokenization methods used?
* What are some of the ways to format tokens under usage limitation?
* Given a long complex paragraph, explain your choice of algorithms/strategies.

## Author

- [Zheng Jie](https://github.com/Bread7)

## References

1. [Deepchecks 5 approaches to solve llm token limits](https://deepchecks.com/5-approaches-to-solve-llm-token-limits/)
2. [Tokenization Differences](https://towardsdatascience.com/word-subword-and-character-based-tokenization-know-the-difference-ea0976b64e17)
3. [Tokenizer Summary](https://huggingface.co/docs/transformers/en/tokenizer_summary#introduction)