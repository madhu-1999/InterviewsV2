---
created: 2026-09-11 09:23
modified: 2026-11-09 06:34
tags:
  - AI/GenAI
prerequisite:
aliases:
  - genai
---
It is a type of AI that creates new content, such as text, images or audio based on patterns learned from existing data.[^1]
+ [c] Knowledge freshness depends on training cutoff.

To overcome this limitation, LLMs can use tools (web search, APIs, code interpreter etc.) to ground their predictions.

```mermaid
--- 
config: 
  theme: neo 
  look: handDrawn
  themeVariables: 
    edgeLabelBackground: '#fff' 
---

flowchart LR
D>Only LLM]
A@{ shape: text, label: "What is the price of Emirates flight from New York to Delhi tomorrow" } ==1==> B[LLM] ==2==> C@{ shape: text, label: "I do not know the latest price."}

E>LLM + Tool]
F@{ shape: text, label: "What is the price of Emirates flight from New York to Delhi tomorrow" } ==1==> G[LLM] <==2==> H[Expedia API]
G ==3==> I@{ shape: text, label: "The price is $1200 economy and $3400 business class" }

```

# Memory Vs Knowledge
[^2][^3]
**Knowledge**: External reference material (RAG database, company policies) and pre-existing data (training data) that <mark style="background: #ADCCFFA6;">remains uniform across all users. </mark>
+ [*] It provides a fixed ground truth that is <mark style="background: #FFB8EBA6;">available across different chats.</mark>

**Memory**: It is unique to every user/chat.
1. **Context Window (Short term memory)**: It holds a limited amount of tokens that a model uses to track the conversation flow. Once this window is exceeded, earlier parts of the conversation are forgotten.
	+ [!] Memory lasts for the active session only!
2. **Long term memory**: User preferences or history is stored externally as embeddings or structured records and <mark style="background: #FFB8EBA6;">reused across multiple chat sessions.</mark>

>[!Example]
>**User query**:
>```
>Can you check if my upcoming flight to London fits our company travel budget rules?
>```
>**LLM + Tool + Memory search process**:
>1. It first **checks memory** to recall user specific context. 
> *<mark style="background: #BBFABBA6;"> User role: VP of Engineering 
Home airport: SFO. 
Booking preference: Delta or British Airways.
Upcoming itinerary draft: SFO to LHR on Oct 12, business class fare quoted at $4,800</mark>*
>2. Next, it checks its knowledge base.
>*Here, it uses a tool to query the company's internal knowledge/database to get the result:<mark style="background: #BBFABBA6;"> Executive tier (VP and above) allows Business Class on international flights over 6 hours, capped at $5,000 per long-haul segment.</mark>*
>3. Lastly, it combines the memory and the knowledge output to generate the final response.
><mark style="background: #BBFABBA6;">*Your flight from SFO to LHR ($4,800 in Business Class) is **within company policy**.
Under our executive travel guidelines, VPs are approved for business class on international routes over 6 hours up to a $5,000 limit.
Would you like me to go ahead and save this quote to your expense draft?*</mark>

# References

[^1]: [Generative AI vs AI agents vs Agentic AI - YouTube](https://www.youtube.com/watch?v=O2gerCxEXvc)

[^2]: [Agent Knowledge vs Memories: Understanding the Difference - DEV Community](https://dev.to/bobur/agent-knowledge-vs-memories-understanding-the-difference-4pgj)

[^3]: [Do Large Language Models have Memory? \| by Edgar Bermudez \| about ai \| Medium](https://medium.com/about-ai/do-large-language-models-have-memory-f5f86405ffd3)
