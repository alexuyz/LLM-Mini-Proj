# LLM-Mini-Proj

## Quick Start (3 steps)
1. **Install dependencies:**
   ```bash
   pip install streamlit numpy sentence-transformers openai gdown matplotlib pandas
   ```
2. **Set OpenAI API Key:**
   ```bash
   export OPENAI_API_KEY="your-key-here"
   ```
3. **Run the app:**
   ```bash
   streamlit run YOUR_FILE_NAME.py
   ```
## Results
 Test 1: Basic (Make sure it runs)
- Categories: `Flowers Colors Cars Weather Food`
- Input: `"Roses are red, trucks are blue"`
- Check: All models produce results

| Model                     | Top Category | Confidence Score |
|---------------------------|--------------|------------------|
| glove_25d                 | Food         | 2.1920           |
| sentence_transformer_384  | Food         | 1.0497           |
| openai_small_1536         | Colors       | 1.1700           |
| openai_large_3072         | Cars         | 1.1224           |


Test 2: Sentiment
- Categories: `Positive Negative`
- Input 1: `"The movie was upsetting"`
| Model                     | Top Category | Confidence Score |
|---------------------------|--------------|------------------|
| glove_25d                 | Positive     | 1.9910           |
| sentence_transformer_384  | Negative     | 1.1408           |
| openai_small_1536         | Negative     | 1.2862           |
| openai_large_3072         | Negative     | 1.2693           |

- Input 2: `"This is terrible"`
| Model                     | Top Category | Confidence Score |
|---------------------------|--------------|------------------|
| glove_25d                 | Positive     | 2.0526           |
| sentence_transformer_384  | Negative     | 1.3541           |
| openai_small_1536         | Negative     | 1.3417           |
| openai_large_3072         | Negative     | 1.3204           |


## Analysis
### Part A
1. Which models got it right?

OpenAI Embeddings: Correctly identified the sentiment in almost all cases, including subtle ones.

Sentence Transformers: Performed very well, correctly handling context.

GloVe: Likely struggled with complex sentences or negations (e.g., "Not good").

2. Why did some fail? 
The Averaged GloVe model fails on inputs like "The movie was not good" because of Negation Blindness.
The "good" vector overpowers the neutral "not," resulting in a positive score despite the sentence being negative.

| Model                    | Prediction | Correct? |
|-------------------------|------------|----------|
| GloVe (25d)             | Positive   | WRONG |
| Sentence Transformers   | Negative   | Correct |
| OpenAI Small            | Negative   | Correct |
| OpenAI Large            | Negative   | Correct |

To an averaging model, "The dog bit the man" and "The man bit the dog" are mathematically identical because they contain the exact same bag of words.

Transformer models (OpenAI/Sentence-Transformers) use an attention mechanism, which processes words in sequence. They understand that "not" modifies "good," allowing them to capture the true semantic meaning.

### Part B
| Feature          | GloVe (Averaged)              | Sentence Transformers        | OpenAI Embeddings            |
|------------------|------------------------------|------------------------------|------------------------------|
| Accuracy         | Low to Medium                 | High                         | Very High                    |
| Word Order       | Ignored (Bag of Words)        | Preserved (Contextual)       | Preserved (Contextual)       |
| Dimensionality   | Low (50–300 dim)              | Medium (384–768 dim)         | High (1536+ dim)             |
| Speed            | Extremely Fast                | Fast                         | Slower (API Latency)         |
| Best Use Case    | Keyword matching, simple tagging | Chatbots, Semantic Search | Complex reasoning, nuance    |


### Part C
Group 1: Electronics Food Groceries Money 
- Input1: `"I bought a red Apple Watch"`
| Model                     | Top Category | Confidence Score |
|---------------------------|--------------|------------------|
| glove_25d                 | Money        | 2.3350           |
| sentence_transformer_384  | Electronics  | 1.2516           |
| openai_small_1536         | Groceries    | 1.1929           |
| openai_large_3072         | Electronics  | 1.2472           |


- Input2: `"I watch a red apple"`
| Model                     | Top Category | Confidence Score |
|---------------------------|--------------|------------------|
| glove_25d                 | Money        | 2.3151           |
| sentence_transformer_384  | Food         | 1.2872           |
| openai_small_1536         | Groceries    | 1.2350           |
| openai_large_3072         | Food         | 1.2692           |

First input it means brand Apple product, second it means fruit apple.

Group 2: Furniture Education Law
- Input1: `"The textook was inside the case."`
| Model                     | Top Category | Confidence Score |
|---------------------------|--------------|------------------|
| glove_25d                 | Law          | 2.0924           |
| sentence_transformer_384  | Furniture    | 1.1595           |
| openai_small_1536         | Furniture    | 1.1702           |
| openai_large_3072         | Furniture    | 1.2020           |

- Input2: `"The case in the textbook is typical"`
| Model                     | Top Category | Confidence Score |
|---------------------------|--------------|------------------|
| glove_25d                 | Law          | 2.2599           |
| sentence_transformer_384  | Law          | 1.2251           |
| openai_small_1536         | Law          | 1.2172           |
| openai_large_3072         | Law          | 1.1924           |

First input it means a storage case, second it means a law case.