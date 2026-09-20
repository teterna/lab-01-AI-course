# Lab 01 – The Price of One Request

## 1. Part 1

1. A Latin letter is 1 byte in UTF-8; Cyrillic is 2. Kazakh letters
   outside the Russian alphabet (ә, ғ, қ, ң, ө, ұ, ү, һ, і) are also 2.
   You can see that in the bytes/char row above.
2. Predict, for the COMPLAINT item: how many TOKENS will the Russian
   version cost, as a multiple of the English one?   RU / EN = 576 / 300 = 1.92x
3. Same question for Kazakh.                          KK / EN = 640 / 300 = 2.13x
4. On what did you base the prediction -- characters, bytes, or words?
    I based my prediction on the number of bytes
5. Which one, if any, would a tokenizer have any reason to follow?
    A tokenizer has more reason to follow bytes than words or characters,
    because text is ultimately represented as bytes. However, tokenizers do
    not tokenize directly by bytes. They use learned tokenization rules and
    merge tables, so the relationship between bytes and tokens is only an
    approximation.

Measured values from Part 2:
* RU / EN = 80 / 59 = 1.36×
* KK / EN = 118 / 59 = 2.00×

Comparison:

|  Ratio  | Prediction | Measured |
| ------- | ---------- | -------- |
| RU / EN |   1.92×    |   1.36×  |
| KK / EN |   2.13×    |   2.00×  |

---

## 2. Annual cost table

I used a volume of **2,000 support requests per day**, which is a realistic workload for a medium-sized online banking support service.

Annual cost (2,000 requests/day × 365 days = 730,000 requests/year):

| Model     | English | Russian |  Kazakh |
| --------- | ------: | ------: | ------: |
| DeepSeek V4 Flash| $50.73 | $64.48 | $157.54 |
| Haiku-4.5 |  $1,540 |  $1,952 |  $4,847 |
| Sonnet-5  |  $3,079 |  $3,904 |  $9,694 |
| Opus-5    |  $7,698 |  $9,760 | $24,236 |
| Fable-5.1 | $15,396 | $19,520 | $48,472 |

The number to remember (python3 part3_cost.py):
RU instead of EN on opus-5, same work, same volume: $2,062/year more (1.27x).
KK instead of EN on opus-5, same work, same volume: $16,538/year more (3.15x).

DeepSeek V4 Flash costs were calculated separately using OpenRouter pricing and the measured token counts from Part 2.

---

## 3. Model choice for a Kazakh-language support queue

To evaluate quality, I used the following checklist:

1. Does not invent a reason for the rate change.
2. Does not invent numbers or information that are not present in the complaint.
3. Responds entirely in the language of the request.
4. Provides a concrete next step.

The English and Russian answers passed the checklist. Both responses correctly stated that the contract and statement were not actually attached, avoided inventing information, and asked the user to provide the documents before any conclusion could be made.

The Kazakh answer also followed the main instruction by acknowledging that no documents were available and by avoiding specific claims about the contract contents. However, it introduced general speculation about possible reasons for rate changes, such as bank policy or market conditions, even though this information was not present in the provided documents. This suggests that additional evaluation of Kazakh-language responses is necessary before production deployment.

Based on the results of this lab, I would select **DeepSeek V4 Flash as the primary candidate for production deployment**. It produced acceptable responses in all three languages while maintaining a significantly lower operating cost than the Claude models used in the cost comparison. Based on the annual cost estimates calculated in this report, DeepSeek V4 Flash would cost approximately **$50.73 per year for English**, **$64.48 per year for Russian**, and **$157.54 per year for Kazakh** at a workload of 2,000 requests per day.

However, cost alone is not sufficient for a production decision. The results show that answer quality, instruction following, and hallucination risk must also be considered. While DeepSeek V4 Flash demonstrated strong cost efficiency, additional testing on real Kazakh-language support cases would be required before deployment in a customer-support environment.

---

## 4. Cost lever not used in this lab

One cost lever not used in this lab is **prompt caching**, where the repeated system prompt is cached and reused instead of being charged at full input-token cost on every request.

## AI Usage Declaration

AI tools were used to assist with adapting the provided lab code to work with the selected model (**DeepSeek V4 Flash**) and to help explain some concepts related to tokenization and cost calculation. All experiments, measurements, command execution, calculations, result verification, analysis, and final conclusions presented in this report were performed and verified by the author.
