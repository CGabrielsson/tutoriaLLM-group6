# README

## Description

The purpose of this repository is to experiment with local LLMs using Ollama, throughout our AI engineering course.

**Models used:**

- `Qwen3:4b`
- `Gemma3:4b`

---

## Assignments
### Assignment 1
We created a python script that sends the same prompt to two different models and saves their responses in a CSV-file.

### Assignment 2
We added new consistency tests for Australia to see if the model would correctly identify Canberra at the capital.
The test asks the model three times and requires at least two correct answers.
We also added a test to check if the LLM judge accepts Canberra as the correct answer.

## 🚀 How To Run

1. Python installed
2. Ollama installed and running
3. Qwen3:4b and Gemma 3:4b downloaded through Ollama
4. Run the script in your IDE terminal:

**Assignment 1:**

   `python .\10_call_models.py`

The script sends the same prompt to both models one at a time, and saves the responses in `results.csv`

**Assignment 2:**

   `python .\7_llm_as_judge.py`

The consistency test asks the model three times using one selected model.

- Default: `Qwen3:4b`
- Optionary: `Gemma3:4b`

---

## Our observations

### Assignment 1
Both models were pretty slow generating responses. We occasionally experienced timeouts, especially with Qwen3:4b.

The order in which the models were executed seemed to affect the performance. Running a model requires a huge amount of RAM, running one model first seems to affect the available memory of the second model.

The responses were much less accurate than those from larger commerical language models like OpenAIs GPT models or Anthropics Claude models.

Both models were able to answer simple questions correctly, like "What is the capital of Sweden?" or common riddles. However they sometimes hallucinated answers. For example, Gemma3:4b answered "This is a classic riddle, the answer is a table" to the trick question "What has two legs but cannot drive a motorcycle?". When asked another nonsensical riddle - "What gets wetter the more it loafy loafy?", Gemma3:4b confidently answered "a sponge" and tried to explain the meaning of the ridddle. Qwen3:4b, although considerably slower, identified the original riddle correctly and answered "a towel". It also suggested that the user had misspelled the riddle. The models tended to give long answers even to simple questions. 

To summarize, the models performed OK on simple questions but was not reliable when handling unusual prompts.

### Assignment 2

We decided to set the passing threshold to 100% since the questions and prompts in our test suite was really simple, and it felt safe to assume that the model would answer them correctly.

We ran the tests several times, starting with using Gemma3:4b. The results was almost identical each time. Some small details in the wording sometimes changed, but the main information was the same. The model consistently answered "Canberra" when asked what Australia's capital is.

We also ran the tests with Qwen3:4b. Both models gave the correct answer "Canberra" and passed all six tests in our test suite. But with Qwen the answers were different than the Gemma-tests. Gemma gave shorter answers while Qwen gave much longer explainations, like historical information about Canberra. Qwens answers was also a bit **too** detailed and some information was misleading, for example in one test it stated:
`"Canberra is a **shared capital city** for the Australian Capital Territory (ACT) and New South Wales."`. While Canberra is the capital of ACT, the correct answer for the state of New South Wales is **Sydney**.

With Qwen we also noticed a lot more variation, especially in wording, between runs. We believe that since the answers were much longer and contained more details, the changes were more noticeable. This is an example of non-determinism in LLMs (the same model can generate different responses even though the prompt is the same, due to the fact that models pick a next token from a list of likely options).
Longer answers also made it more difficult to evaluate if the facts were correct or not.

We also tried running the tests with Qwen3:4b generating the answer and Gemma3:4b acting as the judge and vice versa. Both combinations passed all the tests and both judges evaluated the answers to be correct. Again, Qwen3:4b gave longer and more detailed answers with a lot of information. 

As an additional experiment, we extended the llm_as_a_judge_agrees() prompt to "check all factual claims, including dates, history and locations" and "If any claim is incorrect, reply NO". Interestingly, even so, when Qwen3:4b provided misleading historical information, the LLM-as-a-judge accepted the answer as correct.

Sometimes the LLM-as-a-judge timed out instead of giving an answer, which caused the test to fail. Another potential problem that may come up is that the Python code may accept a wrong answer as correct, just as long as the word "Canberra" is mentioned. Thus, a failed or passed test does not always reveal if the AI has answered correctly or not. Reviewing the generated answers as well as the LLM-as-a-judge verdicts, with print statements, was sometimes necessary to understand why a test failed.
