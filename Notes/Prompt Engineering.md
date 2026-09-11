---
created: 2026-09-10 17:47
modified: 2026-11-09 08:48
tags:
  - AI/GenAI
prerequisite:
aliases:
  - prompt
  - prompting
  - prompts
---
# What is it?
[^1]
>[!quote] 
>When you write a prompt, you are attempting to set up the LLM to predict the right sequence of tokens. 
  Prompt engineering is the process of designing high-quality prompts that guide
  LLMs to produce accurate outputs.
# LLM Settings
[^1][^2]
## Output Length
Limits the number of tokens used to generate a response.
+ [c] **More tokens = more cost = slower response**.
+ [!]  Reducing output length $\ne$ concise output!
	*Your prompt must accommodate for the length restriction to get a complete response.*
## Temperature
Controls the degree of randomness of the tokens in the output.
+ [d] **Lower temperature = more deterministic**
	*Deterministic $\implies$ highest probable token is always picked.*
+ [u] **Higher temperature = more diverse/unexpected** 
## Top-K
Restricts the next predicted token to be one of the *top-K* predicted tokens.
+ [d] **Lower top-K = more factual answers**
+ [u] **Higher top-K = more diverse answers** 
## Top-P
Restricts the next predicted token to be one of the tokens whose cumulative probability $\lt$ *p*.
	*All candidates for the next predicted token are:
	1. Sorted from highest probability to lowest probability
	2. Their probabilities are added cumulatively until the cutoff threshold of **p** is reached
	3. The set of tokens who contribute to the cumulative probability are considered to select the next predicted token*.
+ [d] **Lower top-p = more factual answers**
+ [u] **Higher top-p = more diverse answers**

>[!faq] Difference between top-k and top-p sampling?
>Top-K with a small number gives safer, more focused text, but can cut off useful words or repeat. 
>
>Top-P adapts to context, making the text feel more natural and creative
## Frequency penalty
Applies a penalty on the next token proportional to its frequency in the prompt and response.
+ [u] **Higher frequency = higher penalty = less repetition**
## Presence Penalty
Applies a penalty on repeated tokens irrespective of frequency i.e. same penalty applied even if a token repeats 2 times or 10 times.
+ [d] **Lower presence penalty = more focused response**
+ [u] **Higher presence penalty = more diverse response** 
## Recommendations
[^3]
1. Creative output:
	- **Temperature**: Higher (0.8–1.0) for more diverse and creative output.
	- **Top-p**: Higher (0.9–1.0) for greater exploration.
	- **Top-k**: Higher (100–500) for more exploratory outputs.
2. Factual output:
	- **Temperature**: Lower (0.2–0.5) for more deterministic and coherent responses.
	- **Top-p**: Moderate (0.9) for a controlled degree of randomness.
	- **Top-k**: Lower (10–50) to focus on the most probable tokens.
# Elements of a Prompt
[^4]
>[!quote]
>A prompt contains any of the following elements:
>
**Instruction** - a specific task or instruction you want the model to perform
**Context** - external information or additional context that can steer the model to better responses
**Input Data** - the input or question that we are interested to find a response for
**Output Indicator** - the type or format of the output.
# Types of prompts
[^1]
## System prompts
It defines the ‘big picture’ of what the model should be doing, like translating a language, classifying a review etc.
## Contextual prompts
Provides immediate, task-specific information to guide the response.
## Role prompts
Frames the model’s output style and voice.

>[!example]
>I want you to act as a travel guide. I will write to you about
my location and you will suggest 3 places to visit near me in
a humorous style.
My suggestion: "I am in Manhattan."
Travel Suggestions:
# Prompting Techniques
[^1][^5]
## Zero shot
One clear instruction, no examples

>[!example] 
>Classify the text into neutral, negative or positive. 
>Text: I think the vacation is okay.
>Sentiment:
## One-shot / Few-shot
One clear instruction, one or more examples.
+ [b] **Rule of thumb:** 3 - 5 examples. (*May need more or less depending on complexity of task and input length restriction*)

>[!example]
> Parse a customer's pizza order into valid JSON:
> EXAMPLE:
> I want a small pizza with cheese, tomato sauce, and pepperoni.
> JSON Response:
> ```json
{
"size": "small",
"type": "normal",
"ingredients": ["cheese", "tomato sauce", "peperoni"]("cheese",%20"tomato%20sauce",%20"peperoni")
}
>```
> Now, I would like a large pizza, with the first half cheese and
mozzarella. And the other tomato sauce, ham and pineapple.
JSON Response:
## Step-back 
[^6]
The method involves two steps:
1. **Abstraction**: The model is prompted to focus on a higher-level concept or principle related to the question.
2. **Reasoning**: Once the high-level abstraction is retrieved, the model uses it to reason through the specifics of the original question.

>[!example]
>Original question:
>```
>Write a one paragraph storyline for a new level of a first-
person shooter video game that is challenging and engaging.
>```
>**Abstraction**
>```
>Based on popular first-person shooter action games, what are
5 fictional key settings that contribute to a challenging and
engaging level storyline in a first-person shooter video game?
>```
>**Reasoning**
>```
>Context: 5 engaging themes for a first person shooter video game:
>1. **Abandoned Military Base**: A sprawling, post-apocalyptic
military complex crawling with mutated soldiers and rogue
robots, ideal for challenging firearm combat.
>2. **Cyberpunk City**: A neon-lit, futuristic urban environment
with towering skyscrapers and dense alleyways, featuring
cybernetically enhanced enemies and hacking mechanics.
>3. **Alien Spaceship**: A vast alien vessel stranded on
Earth, with eerie corridors, zero-gravity sections, and
extraterrestrial creatures to encounter.
>4. **Zombie-Infested Town**: A desolate town overrun by hordes of
aggressive zombies, featuring intense close-quarters combat and
puzzle-solving to find safe passage.
>5. **Underwater Research Facility**: A deep-sea laboratory flooded
with water, filled with mutated aquatic creatures, and requiring
stealth and underwater exploration skills to survive.
Take one of the themes and write a one paragraph storyline
for a new level of a first-person shooter video game that is
challenging and engaging.
>```
## Chain-of-thought
Improves reasoning ability of LLMs by generating intermediate reasoning steps.
+ [c] **Intermediate steps = more tokens = higher cost**
+ [p] Low-effort, highly effective across different LLMs.

>[!example]
>Q: When my brother was 2 years old, I was double his age. Now
I am 40 years old. How old is my brother? Let's think step
by step.
A: When my brother was 2 years, I was 2 * 2 = 4 years old.
That's an age difference of 2 years and I am older. Now I am 40
years old, so my brother is 40 - 2 = 38 years old. The answer
is 38.
Q: When I was 3 years old, my partner was 3 times my age. Now,
I am 20 years old. How old is my partner? Let's think step
by step.
## Self-consistency
This method involves two steps:
1. **Generate diverse paths**: [CoT](#Chain-of-thought) prompts are run multiple times with different [temperature](#Temperature) settings to generate different reasoning paths.
2. **Majority voting**: The outputs of all generated paths are analyzed to pick the most frequent answer.
+ [c] **Multiple outputs = more tokens = higher costs**.
+ [p] More robust for complex reasoning tasks compared to CoT.
## ReAct
It is a paradigm that combines verbal reasoning with external tools (search, code interpreter etc), enabling LLMs to solve complex tasks.
It **cycles** through thw following steps:
1. **Thought**: The LLM analyzes the query and generates a plan of action.
2. **Action**: The model chooses and executes an external tool, such as an API call or search engine.
3. **Observation:** The system feeds the tool's output back to the model.
4. **Repeat:** The model updates its plan based on the new data and continues the loop

![ReAct paradigm](Prompt%20Engineering-1789135469921.webp)
**Source**: [Mario Fontana](https://www.linkedin.com/pulse/meet-mr-prompty-how-make-your-ai-think-act-like-human-mario-fontana/)

+ [p] **Reduces Hallucination:** Grounding predictions in external observations makes responses more factual.
- [p] **Improves Traceability:** Step-by-step thoughts make the AI's decision path easy to audit. 
- [c] **Token Heavy:** Repeating cycles consume more tokens and increase processing costs.  
- [c] **Error Propagation:** Bad search results or invalid tool outputs can trap the model in a loop.

>[!example]
>```python
>from langchain.agents import load_tools
from langchain.agents import initialize_agent
from langchain.agents import AgentType
from langchain.llms import VertexAI
prompt = "How many kids do the band members of Metallica have?"
llm = VertexAI(temperature=0.1)
tools = load_tools(["serpapi"], llm=llm)
agent = initialize_agent(tools, llm,
agent=AgentType.ZERO_SHOT_REACT_DESCRIPTION, verbose=True)
agent.run(prompt)
>```
>**Output**
>```> Entering new AgentExecutor chain...
Metallica has 4 members.
Action: Search
Action Input: How many kids does James Hetfield have?
Observation: three children
Thought: 1/4 Metallica band members have 3 children
Action: Search
Action Input: How many kids does Lars Ulrich have?
Observation: 3
Thought: 2/4 Metallica band members have 6 children
Action: Search
Action Input: How many kids does Kirk Hammett have?
Observation: Hammett has been married to his second wife Lani since 1998.
They have two sons, Angel (b. September 29, 2006) and Vincenzo (b. June
28, 2008).
Thought: 3/4 Metallica band members have 8 children
Action: Search
Action Input: How many kids does Robert Trujillo have?
Observation: 2
Thought: 4/4 Metallica band members have 10 children
Final Answer: 10
>```
>
# Best Practices
[^1]
1. **Provide examples**: Shows the LLM desired style/tone of response/outputs and tailor its generated output accordingly.
2. **Design with simplicity**: Prompts should be clear and concise. Use action verbs like *Analyze*, *Describe*, *Create* etc..
3. **Be specific about the output**: Providing specific details helps the LLM narrow its output space.

>[!example]
>```
❌ Generate a blog post about video game consoles.
✅ Generate a 3 paragraph blog post about the top 5 video game consoles.The blog post should be informative and engaging, and it should be written in a conversational style.
>```

4. **Specify constraints using do's**: Instead of specifying what not to do, specify what it can do, to improve accuracy of responses.

>[!example]
>```
>❌ Generate a 1 paragraph blog post about the top 5 video game consoles.
Do not list video game names.
✅ Generate a 1 paragraph blog post about the top 5 video game consoles.
Only discuss the console, the company who made it, the year, and total
sales.
>```
# References
[^1]: [Google Prompt Engineering Guide](https://drive.google.com/file/d/1AbaBYbEa_EbPelsT40-vj64L-2IwUJHy/view)

[^2]: [LLM Settings \| Prompt Engineering Guide](https://www.promptingguide.ai/introduction/settings)

[^3]: [From Randomness to Precision: How Top-k, Top-p, Temperature, and Beam Search Shape Text Generation \| by Ansil M B \| Medium](https://medium.com/@ansilproabl/from-randomness-to-precision-how-top-k-top-p-temperature-and-beam-search-shape-text-generation-d1f50b5220e2)

[^4]: [Elements of a Prompt \| Prompt Engineering Guide](https://www.promptingguide.ai/introduction/elements)

[^5]: [Prompting Techniques \| Prompt Engineering Guide\<!-- --\>](https://www.promptingguide.ai/techniques)

[^6]: [Step-Back Prompting](https://learnprompting.org/docs/advanced/thought_generation/step_back_prompting)
