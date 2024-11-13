# Prompt

I am recording comparative evaluations for prompting LLMs in a Github repository. 

Initially, I'd like to use a simple but effective structure for logging my prompts and outputs and later I might evolve it to something more advanced. 

The purpose of this repository is to keep a log of the comparative performance of LLMs for various prompts (which are chosen because they either reflect discrete prompting strategies or subjects that I frequently prompt for).

My initial idea is to use a folder-based structure in which each evaluation has a descriptive title and nested within it are separate files recording: 1) the prompt, 2) the outputs (which the filename for each model being the model name).

For example:

```
- looking-for-ssd-drive
--- chatgpt.md
--- perplexity.md
--- llama.md
```

Additionally, I would like to record a 'notes.md' file in each experiment providing my thoughts on the results. Later, I would like to add to this objective assessment parameters (perhaps characters and words and tokens used per output). Ideally this would be generated programatically.

Provide feedback. Guide me towards a more effective system. Provide very clear actionable guidance on how to struture the repository for the intended purpose.

# Output (GPT 4-o, Perplexity)

Your idea of using a folder-based structure to log prompts and model outputs is a great starting point. It is simple, intuitive, and allows for easy comparison between models. However, there are a few ways to enhance this structure to make it more scalable, organized, and adaptable for future improvements.

Here’s an outline of how you can structure your GitHub repository for logging and comparing LLM (Large Language Model) performance:

## **1. Folder Structure**

### **Top-Level Structure**
At the top level of your repository, you can organize the experiments by topic or task. Each folder will represent a specific prompt or evaluation scenario.

```
- experiments/
  - looking-for-ssd-drive/
    - chatgpt.md
    - perplexity.md
    - llama.md
    - notes.md
  - summarizing-research-paper/
    - chatgpt.md
    - perplexity.md
    - llama.md
    - notes.md
- scripts/  # For automation scripts (optional)
- README.md  # Overview of the project
```

### **Explanation:**
- **`experiments/`**: This folder will contain subfolders for each evaluation task or prompt.
  - **Subfolders (e.g., `looking-for-ssd-drive/`)**: Each subfolder contains:
    - One markdown file per model (e.g., `chatgpt.md`, `perplexity.md`, etc.) where you record the outputs.
    - A `notes.md` file where you log subjective observations and qualitative assessments.
  
- **`scripts/`**: This folder can store any Python or shell scripts that automate tasks like token counting, word counting, or other objective metrics (optional but highly recommended for scaling).

## **2. File Structure and Content**

### **Model Output Files (`chatgpt.md`, `perplexity.md`, etc.)**
Each model file should follow a consistent format to ensure clarity and comparability across models. Here’s a suggested template:

```markdown
# Model: ChatGPT

## Prompt:
```
[Insert the prompt here]
```

## Output:
[Insert the model's output here]

## Objective Metrics:
- **Word Count**: X words
- **Character Count**: Y characters
- **Token Count**: Z tokens (if applicable)

## Additional Notes:
[Optional space for any model-specific remarks]
```

### **Notes File (`notes.md`)**
The `notes.md` file in each experiment folder will be used to record your reflections on the outputs from all models. It could include subjective assessments such as coherence, factual accuracy, creativity, etc.

```markdown
# Experiment Notes: Looking for SSD Drive

## General Observations:
- Model A (ChatGPT) provided more technical details but was verbose.
- Model B (Perplexity) was concise but missed some key points.

## Comparative Analysis:
- ChatGPT used more tokens but delivered more comprehensive information.
- Perplexity was faster in response generation but lacked depth.
```

## **3. Objective Metrics Tracking**

To enhance your system later with objective metrics like token count, word count, etc., you can automate this process using simple Python scripts. Here’s how you can integrate it:

### **Automation Script Example**

You can create a script that reads the `.md` files in each experiment folder and automatically calculates metrics like word count, character count, and token count.

```python
import os

def count_words(text):
    return len(text.split())

def count_characters(text):
    return len(text)

def process_model_output(file_path):
    with open(file_path, 'r') as f:
        content = f.read()
    
    # Extracting the output section from markdown
    output_start = content.find("## Output:")
    if output_start != -1:
        output_text = content[output_start + len("## Output:"):].strip()
        return {
            "word_count": count_words(output_text),
            "character_count": count_characters(output_text)
        }
    
def process_experiment(experiment_folder):
    results = {}
    for model_file in os.listdir(experiment_folder):
        if model_file.endswith('.md') and model_file != 'notes.md':
            file_path = os.path.join(experiment_folder, model_file)
            metrics = process_model_output(file_path)
            results[model_file] = metrics
    
    return results

# Example usage
experiment_dir = 'experiments/looking-for-ssd-drive'
metrics = process_experiment(experiment_dir)
print(metrics)
```

This script will read through all markdown files in a given experiment folder (except `notes.md`) and calculate word counts and character counts for each model's output.

### **Integration with Markdown Files**
Once you have these metrics calculated programmatically, you can either manually insert them into each `.md` file or automate this process further by appending these metrics to the corresponding sections in each file.

## **4. Version Control and Reproducibility**

To ensure reproducibility and traceability of your experiments:
- Use Git version control to track changes to your prompts and outputs.
- Consider adding a `README.md` at the root level of your repository that explains the purpose of each folder and how to navigate through the experiments.
  
Example `README.md`:

```markdown
# LLM Comparative Evaluation Repository

This repository logs comparative evaluations of large language models (LLMs) for various prompts. Each experiment folder contains:
- The original prompt used for evaluation.
- Outputs generated by different models (one markdown file per model).
- Notes on subjective observations and qualitative comparisons.

## Folder Structure:
```
experiments/
  ├── looking-for-ssd-drive/
      ├── chatgpt.md   # Output from ChatGPT for this prompt
      ├── perplexity.md # Output from Perplexity AI for this prompt
      ├── llama.md      # Output from LLaMA model for this prompt
      └── notes.md      # Subjective analysis of results
```

Future improvements include automated tracking of objective metrics like word count, token usage, etc.
```

## **5. Future Enhancements**

Once you have the basic structure in place, you can evolve it with more advanced features:
1. **Automated Metrics Collection**: Expand your Python script to calculate additional metrics such as token usage (if using APIs that provide tokenization data).
2. **Visualization**: Use tools like Matplotlib or Seaborn to generate visual comparisons of metrics across models (e.g., bar charts showing token usage).
3. **Experiment Tracking Tools**: If your evaluations grow larger in scale, consider integrating tools like [MLFlow](https://mlflow.org/) or [Weights & Biases](https://www.wandb.com/) to track experiments programmatically.

By starting with this clear structure and automating key parts of the process over time, you'll be able to scale your evaluations efficiently while maintaining clarity and organization.

 
 # Prompt 2

 Generate templates for recording prompts, outputs, and experiment notes.

Begin each with a markdown table. 
Use [Text] as a placeholder value instead of generating filler text.

# Output 2

Here are the templates for recording prompts, outputs, and experiment notes, starting with a markdown table in each file. Each table provides a clear summary of the key information, followed by sections for more detailed content.

---

## **1. Template for Recording Prompts (`prompt.md`)**

```markdown
# Prompt Information

| Field         | Description       |
| ------------- | ----------------- |
| **Prompt ID** | [Unique identifier] |
| **Date**      | [Date of prompt]   |
| **Context**   | [Brief context or scenario] |
| **Goal**      | [Objective of the prompt] |

## Prompt Text:

```
[Insert the exact text of the prompt here]
```

## Additional Notes:
[Optional space for any additional context or background information]
```

---

## **2. Template for Model Outputs (`model-name.md`)**

```markdown
# Model Output Summary

| Field             | Description           |
| ----------------- | --------------------- |
| **Model Name**    | [Model name (e.g., ChatGPT)] |
| **Version**       | [Model version or API version] |
| **Date**          | [Date of output generation] |
| **Temperature**   | [Temperature setting, if applicable] |
| **Max Tokens**    | [Maximum tokens allowed in response] |

## Prompt:
```
[Insert the prompt here]
```

## Output:
[Insert the model's output here]

## Objective Metrics:

| Metric             | Value                |
| ------------------ | -------------------- |
| **Word Count**     | [X words]            |
| **Character Count**| [Y characters]       |
| **Token Count**    | [Z tokens (if applicable)] |

## Additional Notes:
[Optional space for any model-specific remarks or observations]
```

---

## **3. Template for Experiment Notes (`notes.md`)**

```markdown
# Experiment Notes

| Field             | Description           |
| ----------------- | --------------------- |
| **Experiment ID**  | [Unique identifier]   |
| **Prompt ID**      | [Corresponding prompt ID] |
| **Date**           | [Date of experiment]  |
| **Models Compared**| [List of models compared in this experiment] |

## General Observations:
- Model A: [Observations about Model A's performance]
- Model B: [Observations about Model B's performance]
- Model C: [Observations about Model C's performance]

## Comparative Analysis:

### Strengths:
- Model A: [Strengths specific to this model]
- Model B: [Strengths specific to this model]

### Weaknesses:
- Model A: [Weaknesses specific to this model]
- Model B: [Weaknesses specific to this model]

## Future Considerations:
[Notes on how future experiments could be improved or what additional metrics to track]
```

---

These templates provide a structured way to document your experiments and will help you maintain consistency across different evaluations. You can easily adapt them as your needs evolve by adding more fields or sections as necessary.