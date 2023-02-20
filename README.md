# Dzongkha [Thai] Romanization (ONGOING PROJECT)

## Dzongkha is a notoriously unphonetic language. As a result it's extremely difficult for learners (both native and non-native) to achieve a reasonable degree of literacy. In this project, I attempted (and failed many times) to create a Dzongkha romanizer.

## A quick review of what doesn't work:

- As seductive as LLMs are, they're not typically pretrained on unusual scripts like that of Dzongkha, and require a custom tokenizer. Moreover, the entire logic of romanization doesn't necessarily require the word-based (or even subword-based) tokenization schemes insofar as we're not generating huge amounts of text.
- The whole tokenization issue led to my foolhardy attempt at using few-shot prompts with GPT2 with the idea that the byte-pair encoding of GPT tokenizers would be somewhat more accomodating. This was very much not the case, but I'm glad I tried.
- The problem with RNNs: RNNs are actually pretty good at this task (at least for the first part of the word). If we create a character-level RNN, we get pretty good results for the first few romanized characters, but as the information from longer-range dependencies dry up, the model kind of goes off the rails. [Take a peek at the RNN approach here]().

## What kind of works?:

- Well, so far I've had the best results with finetuning the ByT5 model by Google. This model uses byte-pair encoding as opposed to word/subword based encoding, allowing for the model to direct its attention towards the internal relationships of a given word.
- Because I was working with very limited Dzongkha data, I decided to use a romanized Thai dataset as a proof-of-concept. Even with just 20,000 entries, the results were far from perfect, but they were pretty good. Check out the most successful attempt [here]().

### SAMPLE OUTPUT (for Thai romanization - byT5-small trained 20000 entries):

| example | original            | byT5                | expected            | edit_distance |
| ------- | ------------------- | ------------------- | ------------------- | ------------- |
| 0       | โรคทางระบบประสาท    | rokthangrabopprasat | rokthangrabopprasat | 0             |
| 1       | พีฟลอกซาซิน         | phifloksasinพin     | phifloksasin        | 3             |
| 2       | เกาะคริสต์มาส       | kokhritsamatاṌch    | kokhritmat          | 6             |
| 3       | โกวิท               | kawithorotchoetभp   | kowit               | 13            |
| 4       | บ้านทุ่งครุ         | banthungkhru        | banthungkhru        | 0             |
| 5       | บ้านบ่อแพ           | banbophae           | banbophae           | 0             |
| 6       | เหมาะเจาะ           | maochorae           | mocho               | 4             |
| 7       | ต้นไทรหิน           | tonsaihinะnṣuen     | tonsaihin           | 6             |
| 8       | การหาเสียงเลือกตั้ง | kanhasianglueaktang | kanhasianglueaktang | 0             |
| 9       | พะเนิน              | phanoenเenḣuenE     | phanoen             | 8             |

### Moving forward:

ByT5 seems to be the best model for finetuning on romanization tasks. In the future, I'd like to experiment with different approaches to tokenization as well as larger datasets. I'll eventually attempt training a larger byT5 model on more data. This will most certainly lead to better results.
