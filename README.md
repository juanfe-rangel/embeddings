# Embeddings for LLMs

Educational notebook explaining how embeddings work in language models and the impact of `max_length` and `stride` parameters.

## Installation

```bash
pip install torch tiktoken matplotlib pandas requests
```

## Notebook Step by Step

### 1. Import Libraries
Load the necessary tools: PyTorch, tiktoken, matplotlib, and pandas.

### 2. Load the Text
Download and read "The Verdict" by Edith Wharton (~5,470 words).

### 3. Tokenization
Convert text into numbers (tokens) using the GPT-2 tokenizer.
- Example: "Hello world" → [15496, 995]

### 4. Dataset with Sliding Window
Create a class that divides the text into windows:
- **max_length**: Size of each window (e.g., 16 tokens)
- **stride**: How much the window moves (e.g., 8 tokens)
- **overlap**: How many tokens repeat = max_length - stride

### 5. Embeddings
Convert tokens into number vectors:
- **Token embeddings**: Token representation
- **Positional embeddings**: Token position in the sequence
- Sum both to obtain the final embedding

## The Experiments

### Experiment 1: What happens if I change max_length?

We test different window sizes (4, 8, 16, 32, 64, 128 tokens):

**Result**: Larger windows → Fewer samples

| Window Size | Samples |
|-------------|---------|
| 4 tokens    | 1,368   |
| 16 tokens   | 342     |
| 128 tokens  | 42      |

**Why?** If each window covers more text, you need fewer windows to cover everything.

### Experiment 2: What happens if I change stride?

We keep max_length = 16 and test different steps (1, 2, 4, 8, 16):

**Result**: Smaller steps → More overlap → More samples

| Stride | Samples | Overlap |
|--------|---------|---------|
| 1      | 5,470   | 94%     |
| 8      | 684     | 50%     |
| 16     | 342     | 0%      |

**Why?** If the window moves less, more content repeats and generates more samples.

## What is Overlap and why is it useful?

**Overlap** = tokens that repeat between consecutive windows

**Advantages of overlap:**
- Context is not lost between windows
- More training examples
- The model sees words in different contexts

**Disadvantages:**
- More training time
- Can cause excessive memorization

---
## authors
Juan Felipe Rangel Rodriguez
