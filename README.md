# Agentic Math Tool
Pair project (w/ [Laura Bengs](https://github.com/LauraBengs)) as part of the Master's block course Infrastructure for Advanced Analytics and Machine Learning in SS2025 @ LMU Munich.  


📄 Read the report [here](https://github.com/ngoax/agent/blob/1aec0f7ccb558ce6b5a01e09d6dd8a994fd8953a/report.pdf)

## Abstract  
Agents are an emergent field in AI, which is concerned with equipping large language models (LLMs) with further capabilities such as tool calling, thereby eliminating the need for users to orchestrate individual steps and extending the capabilities of LLMs beyond conventional chatbots.

In this project, we build a **Math Agent** using [LangGraph](https://www.langchain.com/langgraph) that can solve mathematical problems with two tools:  
- a **document extractor** ([Docling](https://github.com/DS4SD/docling))  
- a **symbolic calculator** ([NumExpr](https://github.com/pydata/numexpr))

We hypothesize that augmenting LLMs with a symbolic calculator could improve accuracy for mathematical tasks, mitigating the inherent tendency of LLMs to hallucinate due to its autoregressive nature.

We evaluate four different models on the [GSM8K dataset](https://github.com/openai/grade-school-math):  
- **Open-weight models (via [Ollama](https://ollama.com/)):**  
  - Qwen 2.5-7B  
  - Qwen 2.5-14B  
- **Closed-source API models:**  
  - Claude 3.5 Sonnet (Anthropic)  
  - Claude 3.7 Sonnet (Anthropic)   

### Key Findings  
1. Tool calling **improves correctness** across all models.  
2. Smaller models with tools can match or outperform larger models without tools.  
3. Tool calling increases **latency** and **token usage**.  
4. Our implementation performs competitively compared to GSM8K benchmarks.  


## Methods & System Design  

### Frameworks & Ecosystem  
- [LangChain](https://www.langchain.com/) – LLM app framework  
- [LangGraph](https://www.langchain.com/langgraph) – graph-based agent orchestration  
- [LangSmith](https://www.langchain.com/langsmith) – debugging & evaluation platform  

### Tools  
- **Document Extractor:** [Docling](https://github.com/DS4SD/docling)  
  - PDF, DOCX, image, and OCR support 
- **Math Calculator:** [NumExpr](https://github.com/pydata/numexpr)  
  - Fast numerical expression evaluation, memory-efficient  


## Evaluation  

- **Dataset:** [GSM8K](https://github.com/openai/grade-school-math) (subset of 100 problems)  
- **Environment:** Google Colab (NVIDIA T4, 15GB GPU, 12.7GB RAM)  

### Results (String Input w/ vs. w/o Tools)  
| Model            | Tool Calling | Accuracy | P50 Latency   | P99 Latency  |
|------------------|--------------|-------------|-------|--------|
| Claude 3.5 Sonnet | ✅         | 0.95        | 4.08s | 11.18s |
| Claude 3.5 Sonnet | ❌        | 0.60        | 0.52s | 1.29s  |
| Claude 3.7 Sonnet | ✅         | 0.92        | 5.45s | 14.41s |
| Claude 3.7 Sonnet | ❌        | 0.68        | 0.73s | 1.53s  |
| Qwen 2.5-14B      | ✅         | 0.46        | 0.38s | 5.34s  |
| Qwen 2.5-14B      | ❌        | 0.32        | 0.31s | 0.55s  |
| Qwen-7B           | ✅         | 0.59        | 1.31s | 6.60s  |
| Qwen-7B           | ❌        | 0.24        | 0.16s | 0.43s  |

**Overall**: Tool calling improves correctness significantly, but at the cost of latency, which could mainly be due to the increased token count when invoking tools. While there is a significant speed tradeoff to be made in search of accuracy, future work attempting to embed symbolic math engines into the native architecture of LLMs could potentially mitigate this tradeoff. 
