## python3 part0_tokenizers.py

```text
PART 0 -- OpenAI tokenizer pair. No API key, no cost, no network.

SENTENCE

------------------------------------------------------------------

                         EN           RU           KK

o200k_base               12           18           21

  ÷ EN                1.00x        1.50x        1.75x

cl100k_base              12           35           58

  ÷ EN                1.00x        2.92x        4.83x

COMPLAINT

------------------------------------------------------------------

                         EN           RU           KK

o200k_base               59           80          118

  ÷ EN                1.00x        1.36x        2.00x

cl100k_base              59          146          265

  ÷ EN                1.00x        2.47x        4.49x

SYSTEM_PROMPT

------------------------------------------------------------------

                         EN           RU           KK

o200k_base               40           49           67

  ÷ EN                1.00x        1.23x        1.68x

cl100k_base              40           77          157

  ÷ EN                1.00x        1.93x        3.92x

These two bracket the six-tokenizer spread shown in lecture 02: o200k_base is the kindest to Kazakh there, cl100k_base the harshest. Llama 3, Qwen2.5, Gemma and DeepSeek-V3 are not reproduced here -- gated weights or large downloads, not a classroom fit.
```

## python3 part1_offline.py

```text
PART 1 -- offline size measures. No API key, no cost, no network.

SENTENCE

------------------------------------------------------------------

                         EN           RU           KK

chars                    69           75           72

bytes                    69          139          135

words                    11           11            9

bytes/EN              1.00x        2.01x        1.96x

bytes/char             1.00         1.85         1.88

COMPLAINT

------------------------------------------------------------------

                         EN           RU           KK

chars                   300          315          344

bytes                   300          576          640

words                    54           45           42

bytes/EN              1.00x        1.92x        2.13x

bytes/char             1.00         1.83         1.86

SYSTEM_PROMPT

------------------------------------------------------------------

                         EN           RU           KK

chars                   175          181          206

bytes                   175          331          380

words                    34           27           28

bytes/EN              1.00x        1.89x        2.17x

bytes/char             1.00         1.83         1.84
```

## python3 part2_measure.py --call

```text
counting tokens on deepseek/deepseek-v4-flash-0731:free

sentence       en=12  ru=18  kk=21

complaint      en=59  ru=80  kk=118

system_prompt  en=40  ru=49  kk=67

request        en=99  ru=129  kk=185

answering complaint:

[en]

--- answer ---

I’m sorry, but I’m unable to confirm the basis for the rate change or restore the terms because I don’t see the contract or statement you referenced in our conversation. Without those documents, I cannot verify the original terms or the reason for the August change. Please re-attach the files, and I’ll be happy to review them for you.

billed: 182 in, 402 out

[ru]

--- answer ---

Здравствуйте. Поскольку приложенные вами договор и выписка отсутствуют в данном чате, я не могу посмотреть конкретные условия вашего договора и проверить правомерность изменения ставки.

В соответствии с моими инструкциями, я работаю только с теми документами, которые вы прикрепите в этот чат. Пожалуйста, прикрепите файлы ещё раз (или напишите реквизиты договора), и я постараюсь найти основание изменения ставки в предоставленных документах. Если в них не окажется информации об этом, я буду вынужден сообщить, что ответа на ваш вопрос в них нет.

billed: 239 in, 509 out

[kk]

--- answer ---

Қайырлы күн! Сұрағыңызға жауап беруге тырысамын, бірақ нақты жағдайды тексеру үшін маған банктік деректеріңіз бен құжаттарыңыз қажет. Ұсынылған сипаттамада менің қолымда сіздің депозит шартыңыз, үзінді көшірмеңіз немесе мөлшерлеме өзгерісіне қатысты нақты ақпарат жоқ.

Менің білуімше, бұл жағдайда ең дұрысы — банк бөлімшесіне хабарласып, келісімшартыңыздың тиісті бөлімдерін тексерту. Мөлшерлеменің өзгеру себебі сіздің келісіміңіздегі талаптарға, банктік саясатқа немесе нарықтық жағдайларға байланысты болуы мүмкін. Алайда, мен сіздің құжаттарыңызды көрмейінше, нақты негіздеме бере алмаймын және бастапқы шарттарды қалпына келтіру мүмкіндігін растай алмаймын.

Өтінемен, банк бөлімшесіне немесе ресми қолдау қызметіне жүгініп, келісімшартыңыздың тиісті тармақтарын сұраңыз. Олар сізге нақты түсініктеме беріп, мәселені шеше алады.

billed: 402 in, 1291 out

wrote measurements.json
```

## python3 part3_cost.py

```text
prices from https://platform.claude.com/docs/en/about-claude/models/overview.md

checked 2026-09-12; tokens counted on deepseek/deepseek-v4-flash-0731:free

answer length: measured in Part 2, per language

ONE SUPPORT REQUEST -- tokens, and cost in US cents

------------------------------------------------------------------------

                        EN          RU          KK

input tokens            99         129         185

output tokens          402         509        1291

haiku-4.5           0.2109      0.2674      0.6640

sonnet-5            0.4218      0.5348      1.3280

opus-5              1.0545      1.3370      3.3200

fable-5.1           2.1090      2.6740      6.6400

AT 2,000 REQUESTS/DAY -- US dollars per year

------------------------------------------------------------------------

                        EN          RU          KK

haiku-4.5            1,540       1,952       4,847

sonnet-5             3,079       3,904       9,694

opus-5               7,698       9,760      24,236

fable-5.1           15,396      19,520      48,472

TWO RATIOS THAT ARE NOT THE SAME NUMBER

------------------------------------------------------------------------

                        EN          RU          KK

input only           1.00x       1.30x       1.87x

total bill           1.00x       1.27x       3.15x

The first row is a property of the tokenizer. The second is what you
actually pay on opus-5, and it moves with the length of the answer.

State which one you mean, every time, or the number is worthless.

THE NUMBER TO REMEMBER

------------------------------------------------------------------------

RU instead of EN on opus-5, same work, same volume: $2,062/year more (1.27x).

KK instead of EN on opus-5, same work, same volume: $16,538/year more (3.15x).
```
