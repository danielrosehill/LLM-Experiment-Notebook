# Prompt

I'm interested in evaluating the comparative performance of different LLMs because I am always interested in discovering which LLMs are most suitable for specific use-cases (or which would be most useful to me in general). 

My prompting tends to focus on a few core areas: the bulk of my personal prompting consists of technical queries. 

Here's a prompt I ran today which is very typical:

```
"I have a folder of markdown files that have lost their formatting. Can you recommend some tools that could automatically detect appropriate formatting and add markdown formatting for headers, etc"
```

For work, I use LLMs frequently for general reference and for data discovery. I work in impact investing. An example prompt:

```
"Please summarise how carbon intensity is calculated"
```

```
"Can you identify a company in the oil and gas sector which reported on their scope 1 and 2 emissions last year, each individually?"
```

I would like to develop my own evaluation methodology for both of these use-cases, and others, focusing on the parameters that are of most interest to me:

- Is the information accurate?
- Is the information reasonably current?
- Is the output reasonably succint without too much extraneous information?
- Are the sources cited reliable?

Can you suggest how some ways I might go about formalising a system for comparative model evaluation and some tools that might be helpful in this regard?

# Output
To evaluate the comparative performance of different Large Language Models (LLMs) for your specific use cases, you can develop a structured evaluation methodology focusing on the parameters most relevant to you: **accuracy**, **recency**, **conciseness**, and **source reliability**. Below is a guide on how to formalize this evaluation system and some tools that could assist you.

## **1. Define Evaluation Metrics**
For your use cases—technical queries and impact investing research—you will need to assess models on the following key dimensions:

### **Accuracy**
- **Definition**: How correct and factual are the outputs?
- **Method**: You can manually verify the outputs against trusted sources or use automated tools like DeepEval, which offers metrics such as *hallucination* and *faithfulness* to check if the model fabricates information[7].
  
### **Recency**
- **Definition**: Is the information up-to-date?
- **Method**: For this, you may need to cross-check outputs with recent publications or databases. Some LLMs, especially those integrated with retrieval-augmented generation (RAG), can pull current information from external sources[5]. You can evaluate this using *context precision* and *answer correctness* metrics[3].

### **Conciseness**
- **Definition**: Does the model provide succinct answers without unnecessary details?
- **Method**: You can manually assess this by comparing the length of responses across models or use automated metrics like *response completeness and conciseness*, which are available in frameworks like DeepEval[6].

### **Source Reliability**
- **Definition**: Are the cited sources credible and relevant?
- **Method**: Some tools, such as OpenAI Evals or RAGAS, allow you to evaluate how well the model retrieves and cites reliable sources[5][6]. You could also manually verify citations for credibility.

## **2. Develop a Benchmarking Process**
To ensure consistency in your evaluations, follow these steps:

### **Step 1: Select Benchmark Prompts**
Use a set of representative prompts from your typical use cases. For example:
- Technical query: "I have a folder of markdown files that have lost their formatting. Can you recommend some tools that could automatically detect appropriate formatting?"
- Impact investing query: "Please summarize how carbon intensity is calculated."

### **Step 2: Choose Evaluation Metrics**
You should choose both quantitative and qualitative metrics:
- Quantitative: BLEU, ROUGE, Perplexity for text similarity and fluency[4][5].
- Qualitative: Human evaluation for relevance, coherence, and source reliability[1].

### **Step 3: Run Comparative Tests**
Run these prompts across different LLMs (e.g., GPT-4, Claude, LLaMA) and record their performance based on your defined metrics. Use frameworks like DeepEval or MLFlow LLM Evaluate to automate this process[7].

### **Step 4: Analyze Results**
After collecting results, compare models across each metric. For example:
| Model   | Accuracy | Recency | Conciseness | Source Reliability |
|---------|----------|---------|-------------|--------------------|
| GPT-4   | High     | Medium  | High        | High               |
| Claude  | Medium   | High    | Medium      | Medium             |
| LLaMA   | Low      | Low     | High        | Low                |

## **3. Use Evaluation Tools**
Several tools can help automate parts of this process:

### **DeepEval**
This open-source framework provides over 14 evaluation metrics including hallucination detection, answer relevancy, contextual recall, and more. It’s ideal for testing technical queries and research tasks[7].

### **Promptfoo**
This tool allows systematic testing of prompts across different models with predefined tests. It’s useful for evaluating prompt performance in real-world scenarios[7].

### **MLFlow LLM Evaluate**
A modular tool that integrates into custom pipelines to evaluate models on RAG (retrieval-augmented generation) tasks and question-answering accuracy[7].

### **OpenAI Evals**
This framework allows you to test models using standardized benchmarks and custom metrics tailored to your specific needs, such as accuracy and source reliability[5][6].

## Conclusion
By defining clear evaluation metrics like accuracy, recency, conciseness, and source reliability, and using tools like DeepEval or Promptfoo to automate testing across multiple LLMs, you can develop a robust system for comparing model performance tailored to your technical queries and impact investing research needs.

 