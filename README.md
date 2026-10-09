# KeyBoardWordPrediction

# ⌨️ KeyBoard Word Prediction & Autocorrect

A simple **NLP-based word prediction and autocorrection system** that suggests the most likely words when a user enters a partially typed or misspelled word.

The system combines:

- **Word frequency**
- **Probability of a word**
- **Character 2-grams**
- **Jaccard similarity**
- **Ranking of candidate words**

The project is implemented as a web application using **HTML, CSS and JavaScript**, with a Python implementation used to develop and test the prediction logic.

🔗 **Live Demo:** [https://keyboard-word-prediction.vercel.app/](https://key-board-word-prediction.vercel.app/)

---

## 1. What Problem Does This Project Solve?

When we type on a phone or computer keyboard, the system often suggests words or corrects spelling mistakes.

For example:

```text
User types:  hel

Possible suggestions:
hello
help
held
```

Or:

```text
User types:  movi

Possible suggestions:
movie
moving
movies
```

Instead of simply checking whether a word exists, this project tries to find **similar words from a dictionary and rank them according to similarity and frequency**.

---

# 2. Basic Idea

The project follows this simple pipeline:

```text
User enters a word
        ↓
Convert to lowercase
        ↓
Check dictionary
        ↓
If exact match → return the word
        ↓
Otherwise:
        ↓
Generate character 2-grams
        ↓
Calculate Jaccard similarity
        ↓
Calculate word probability
        ↓
Rank candidate words
        ↓
Return Top 3 suggestions
```

### In one sentence:

> **The system finds words that look similar to the input and then uses how frequently those words occur to rank the suggestions.**

---

# 3. Technologies Used

| Technology | Purpose |
|---|---|
| HTML | Web page structure |
| CSS | User interface and styling |
| JavaScript | Main prediction logic and interaction |
| Python | NLP algorithm development/testing |
| Pandas | Handling word-frequency data in Python |
| NumPy | Numerical operations |
| TextDistance | Jaccard similarity implementation |
| Regular Expressions | Cleaning and extracting words |
| Counter | Calculating word frequencies |
| Vercel | Deployment |

---

# 4. Dataset

The project uses a text file:

```text
words.txt
```

The text is processed to create a vocabulary and calculate how frequently each word occurs.

### Preprocessing

The text is:

1. Read from the file
2. Converted to lowercase
3. Tokenized using a regular expression
4. Stored as a list of words
5. Used to calculate word frequencies

Example:

```text
"Hello world! Hello everyone."

↓

hello
world
hello
everyone
```

The frequency becomes:

```text
hello     → 2
world     → 1
everyone  → 1
```

---

# 5. Word Frequency

Word frequency means:

> **How many times a particular word appears in the training text.**

For example:

```text
hello → 100 occurrences
world → 50 occurrences
computer → 20 occurrences
```

A Python `Counter` is used to calculate this.

```python
word_freq_dict = Counter(words)
```

So:

```text
word_freq_dict["hello"] = 100
```

---

# 6. Word Probability

Frequency alone is converted into a probability.

### Formula

```text
P(word) = Frequency(word) / Total number of words
```

Example:

Suppose:

```text
Total words = 1000

hello appears = 100 times
```

Then:

```text
P(hello) = 100 / 1000
         = 0.1
```

So `hello` has a probability of **10%** in this dataset.

The project calculates this for every word.

```python
probs[k] = word_freq_dict[k] / Total_words_freq
```

---

# 7. What is a 2-Gram?

This is one of the **most important interview concepts** in this project.

An **n-gram** is a sequence of `n` consecutive characters or words.

Here we use **character 2-grams**.

Example:

```text
hello
```

With padding:

```text
#hello#
```

The 2-grams are:

```text
#h
he
el
ll
lo
o#
```

The JavaScript implementation creates these character n-grams using padding.

### Why use 2-grams?

Because they allow us to compare two words based on their **character-level similarity**.

For example:

```text
hello
helo
```

share many character pairs.

Therefore, they should have a relatively high similarity.

---

# 8. What is Jaccard Similarity?

Jaccard similarity measures how similar two sets are.

### Formula

```text
Jaccard Similarity =
Size of Intersection
--------------------
Size of Union
```

or:

```text
J(A,B) = |A ∩ B| / |A ∪ B|
```

The value lies between:

```text
0 → completely different
1 → exactly the same
```

---

# 9. Jaccard Example

Suppose:

```text
Word 1 = hello
Word 2 = helo
```

After converting them into character 2-grams, we compare the sets.

If many 2-grams are common:

```text
Intersection = large
Union = relatively small
```

Therefore:

```text
Jaccard similarity → high
```

A high similarity means the two words are likely candidates for autocorrection/prediction.

---

# 10. How Does Autocorrection Work?

Suppose the user enters:

```text
hel
```

The system first checks:

```text
Is "hel" present in the dictionary?
```

If yes:

```text
return hel
```

If not:

```text
Compare "hel" with dictionary words
```

For every dictionary word:

```text
Calculate Jaccard similarity
```

Only words with sufficient similarity are considered.

The JavaScript implementation currently uses a threshold of:

```text
similarity > 0.1
```

Then it ranks the candidates and returns the top 3 suggestions.

---

# 11. How Are Suggestions Ranked?

This is another **very important interview question**.

The project considers two things:

### 1. Similarity

How similar is the candidate word to what the user typed?

### 2. Probability

How frequently does that word occur in the dataset?

So a good suggestion should ideally be:

```text
Highly similar
       +
Frequently occurring
```

The JavaScript implementation calculates a combined score:

```text
Combined Score =
Similarity × Probability × 1000
```



The Python implementation similarly sorts candidates using:

```text
Similarity
Probability
```

and selects the top 3.

---

# 12. Why Do We Need Probability?

Imagine the user enters:

```text
hel
```

Suppose we have:

```text
hello → similarity 0.90
helicopter → similarity 0.89
help → similarity 0.88
```

If `hello` occurs much more frequently in our dataset, it should probably be ranked higher.

Therefore:

```text
Similarity tells us:
"Does this word look like the input?"

Probability tells us:
"How common is this word?"
```

Combining both gives a better ranking.

---

# 13. Overall Architecture

```text
                 ┌──────────────────┐
                 │    words.txt     │
                 └────────┬─────────┘
                          ↓
                  Text Preprocessing
                          ↓
                  Word Tokenization
                          ↓
                 Frequency Calculation
                          ↓
                  Probability Calculation
                          ↓
                  ┌─────────────────┐
User Input ─────→ │ Prediction Engine│
                  └────────┬────────┘
                           ↓
                    2-Gram Generation
                           ↓
                  Jaccard Similarity
                           ↓
                  Candidate Ranking
                           ↓
                      Top 3 Words
                           ↓
                    Web Interface
```

---

# 14. Important Files

```text
KeyBoardWordPrediction/
│
├── index.html
├── styles.css
├── script.js
├── untitled5.py
├── words.txt
└── README.md
```

### `index.html`

Creates the structure of the web page.

For example:

- Input box
- Prediction button
- Result area
- Example words

### `styles.css`

Controls:

- Layout
- Fonts
- Colors
- Buttons
- Prediction cards
- Dark mode styling

### `script.js`

This is the **main application logic**.

It:

- Loads the word data
- Calculates probabilities
- Generates n-grams
- Calculates Jaccard similarity
- Performs autocorrection
- Ranks suggestions
- Displays results

### `untitled5.py`

This is the Python implementation used to develop/test the NLP logic.

It uses:

```text
Pandas
NumPy
TextDistance
Regex
Counter
```

### `words.txt`

Contains the text/words used to build the vocabulary and frequency distribution.

---

# 15. Python Implementation

The basic workflow is:

```python
words = []
```

Read the dataset:

```python
with open('words.txt', 'r', encoding='utf-8') as f:
    data = f.read()
```

Convert to lowercase:

```python
data = data.lower()
```

Extract words:

```python
word = re.findall(r'\w+', data)
```

Calculate frequency:

```python
word_freq_dict = Counter(words)
```

Calculate probability:

```python
probs[k] = word_freq_dict[k] / Total_words_freq
```

Calculate similarity:

```python
similarities = [
    1 - textdistance.Jaccard(2).distance(w, word)
    for w in word_freq_dict.keys()
]
```

Then sort:

```python
df.sort_values(
    ['Similarity', 'Prob'],
    ascending=False
).head(3)
```

So the **Python version clearly shows the NLP algorithm**, while the JavaScript version integrates that idea into the web application.

---

# 16. What Kind of NLP Is This?

This is a **classical NLP approach**, not deep learning.

It uses:

```text
Text preprocessing
      ↓
Word frequency
      ↓
Probability
      ↓
Character n-grams
      ↓
Jaccard similarity
      ↓
Ranking
```

There is:

❌ No neural network  
❌ No LSTM  
❌ No Transformer  
❌ No BERT  
❌ No large language model  

This is actually a good interview project because you can explain **every part of the algorithm**.

---

# 17. Is This AI?

A safe interview answer is:

> "It is an NLP-based intelligent text prediction and autocorrection system. It uses statistical word frequency and character-level similarity rather than a deep learning model."

Don't say:

> "I trained an AI model."

Because you did **not** train a neural network.

A better description is:

> **NLP-based word prediction and autocorrection using statistical frequency and Jaccard similarity.**

---

# 18. Example Dry Run

Suppose the user enters:

```text
hel
```

### Step 1 — Normalize

```text
"hel" → "hel"
```

### Step 2 — Dictionary check

```text
Is "hel" present?

NO
```

### Step 3 — Generate 2-grams

For the input, character pairs are generated.

### Step 4 — Compare with dictionary words

For example:

```text
hello
help
held
world
computer
```

Calculate:

```text
Jaccard(hel, hello)
Jaccard(hel, help)
Jaccard(hel, held)
...
```

### Step 5 — Remove weak candidates

Candidates below the similarity threshold are ignored.

### Step 6 — Calculate probability

For each remaining word:

```text
P(word) = frequency / total words
```

### Step 7 — Rank

Use:

```text
similarity + frequency/probability
```

to determine the best candidates.

### Step 8 — Return

```text
Top 3 suggestions
```

---

# 19. Complexity

This is an important area where an interviewer may ask follow-up questions.

Suppose:

```text
V = number of words in vocabulary
L = average word length
```

For each input, the current implementation potentially compares the input against many dictionary words.

Generating/comparing n-grams takes approximately:

```text
O(L)
```

for one candidate word.

For `V` vocabulary words:

```text
O(V × L)
```

approximately.

Therefore, the approach can become slow with a **very large vocabulary**.

---

# 20. How Could You Improve Performance?

If the interviewer asks:

> "How would you improve this system?"

You can say:

### 1. Use a Trie

A Trie can efficiently find words with a given prefix.

Example:

```text
Input: "com"

Trie
 └── c
     └── o
         └── m
             ├── e
             ├── p
             └── m
```

This is useful for autocomplete.

---

### 2. Use an n-gram index

Instead of comparing against every dictionary word, maintain an index:

```text
2-gram → words containing that 2-gram
```

Then only compare with relevant candidates.

---

### 3. Use a better ranking model

Instead of only:

```text
Similarity + frequency
```

we could consider:

```text
Similarity
+
Word frequency
+
Previous words
+
User history
+
Context
```

---

### 4. Use a language model

A more advanced system could use:

```text
RNN
LSTM
Transformer
BERT/GPT-style language model
```

to understand the **context** of the sentence.

---

# 21. Current Limitation

The biggest limitation is:

> **The current system primarily considers the similarity and frequency of individual words rather than the full sentence context.**

For example:

```text
I am going to the ...
```

A context-aware model could predict:

```text
market
store
office
```

based on the previous words.

Our current system is more focused on:

```text
"helo" → "hello"
```

rather than:

```text
"I am going to the" → "market"
```

This is an excellent answer if the interviewer asks:

> "What is the limitation of your project?"

---

# 22. Difference Between Autocorrect and Next-Word Prediction

This distinction is VERY important.

### Autocorrect

User types:

```text
helo
```

System suggests:

```text
hello
```

It is trying to correct the **current word**.

### Next-word prediction

User types:

```text
I am going
```

System predicts:

```text
home
to
there
```

It predicts the **next word using context**.

### Your current project

Your implementation is primarily closer to:

> **word completion/autocorrection based on character similarity and word frequency.**

Do not claim that it is a sophisticated context-aware next-word language model.

---

# 23. Why Jaccard Instead of Exact Matching?

Exact matching would only work when:

```text
input == dictionary word
```

For example:

```text
hello == hello → match
helo != hello → no match
```

Jaccard allows us to find **approximately similar words**.

Therefore:

```text
helo
```

can still find:

```text
hello
```

because their character n-grams overlap.

---

# 24. Why Character n-grams Instead of Word n-grams?

Because the problem is about **partially typed/misspelled words**.

For:

```text
helo
hello
```

character-level information is useful.

Word-level n-grams are more useful for:

```text
"I am going"
```

where we want to predict the next word.

So:

```text
Character n-grams
        ↓
Spelling / word similarity
```

while:

```text
Word n-grams
        ↓
Sentence/context prediction
```

---

# 25. Important Interview Questions

## Q1. Explain your project.

**Answer:**

> "I built an NLP-based word prediction and autocorrection system. The user enters a partially typed or misspelled word, and the system compares it with words in a vocabulary using character 2-grams and Jaccard similarity. I also calculate the probability of each word based on its frequency in the dataset. Finally, I rank the candidate words using similarity and frequency and display the top three suggestions through a web interface."

---

## Q2. What is the main algorithm?

> "The main algorithm uses character 2-gram generation, Jaccard similarity, word-frequency probability and candidate ranking."

---

## Q3. What is Jaccard similarity?

> "Jaccard similarity measures how similar two sets are. It is the size of their intersection divided by the size of their union. In my project, the sets are character 2-grams of two words."

---

## Q4. Why did you use 2-grams?

> "2-grams capture local character patterns. They are useful for comparing partially typed or misspelled words because similar words tend to share many character pairs."

---

## Q5. How do you calculate probability?

> "I divide the frequency of a word by the total number of words in the dataset."

```text
P(word) = Frequency(word) / Total words
```

---

## Q6. Why do you need word frequency?

> "Two words may have similar character similarity, so frequency provides another signal for ranking. A more frequently occurring word can be given higher priority."

---

## Q7. How many suggestions do you return?

> "The system returns the top three candidate words."

---

## Q8. What happens if the word already exists?

> "The system first checks whether the word exists in the dictionary. If it exists, it returns it as an exact match instead of performing the similarity search."

The current implementation does exactly this.

---

## Q9. What happens if the word doesn't exist?

> "I calculate its character-level similarity with dictionary words, filter weak matches, rank the remaining candidates using similarity and probability, and return the top three."

---

## Q10. What is the biggest limitation?

> "It does not deeply understand sentence context. It mainly focuses on word-level similarity and frequency. A more advanced version could use word n-grams or a neural language model to consider previous words."

---

## Q11. Is it machine learning?

> "It is an NLP-based statistical approach rather than a trained machine-learning or deep-learning model. I use the frequency distribution of the dataset and similarity measures to make predictions."

---

## Q12. How would you make it better?

> "I would add context-aware prediction using bigram or trigram language models, use a Trie for efficient prefix matching, improve candidate retrieval using an n-gram index, and eventually compare the system with a neural language model."

---

# 26. 5 Concepts You MUST Know Before the Interview

If you have very little time tonight, **do NOT try to study everything.**

Master these five:

### ⭐ 1. NLP

```text
Natural Language Processing
=
Teaching computers to process human language.
```

---

### ⭐ 2. Frequency

```text
How many times a word appears.
```

---

### ⭐ 3. Probability

```text
Frequency / Total words
```

---

### ⭐ 4. 2-gram

```text
Two consecutive characters.
```

Example:

```text
hello

#h
he
el
ll
lo
o#
```

---

### ⭐ 5. Jaccard Similarity

```text
Intersection / Union
```

Range:

```text
0 → completely different
1 → identical
```

---

# 27. One-Minute Project Explanation

If the interviewer suddenly says:

> **"Explain your project."**

Say this:

> "My project is an NLP-based keyboard word prediction and autocorrection system. The main goal is to suggest likely words when a user enters a partially typed or misspelled word.
>
> I first preprocess the text dataset and calculate the frequency of each word. From the frequency, I calculate the probability of each word. When the user enters a word, I generate character 2-grams and compare them with the 2-grams of dictionary words using Jaccard similarity.
>
> I then combine the similarity information with word frequency to rank the candidate words and return the top three suggestions. I implemented the core NLP logic in Python and integrated the prediction logic into a JavaScript-based web interface.
>
> The main limitation is that the current system focuses mainly on individual-word similarity rather than understanding the complete sentence context. In the future, I could improve it using n-gram language models or neural language models."

---

# 28. Resume Description

### Short version

> **Keyboard Word Prediction & Autocorrection** — Developed an NLP-based word suggestion system using character 2-grams, Jaccard similarity and word-frequency probabilities to rank and return top word suggestions through a web interface.

### Technologies

```text
HTML | CSS | JavaScript | Python | NLP | Jaccard Similarity
```

---

# 29. Final Mental Model

Remember this picture before your interview:

```text
                 WORD DATASET
                      ↓
                Clean the text
                      ↓
             Count word frequency
                      ↓
              Calculate probability
                      ↓
              USER ENTERS "helo"
                      ↓
             Generate character
                 2-GRAMS
                      ↓
            Compare with dictionary
                      ↓
             JACCARD SIMILARITY
                      ↓
          Combine with word frequency
                      ↓
                 RANK WORDS
                      ↓
             ┌──────┬──────┬──────┐
             │hello │help  │held  │
             └──────┴──────┴──────┘
                      ↓
                 TOP 3 RESULTS
```

### The whole project in 5 words:

> **Frequency + Probability + 2-grams + Jaccard + Ranking**

If you remember those five things, you can handle most basic questions about this project.
