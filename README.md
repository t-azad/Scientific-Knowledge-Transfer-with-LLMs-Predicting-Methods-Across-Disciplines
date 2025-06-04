# Scientific-Knowledge-Transfer-with-LLMs-Recommending-Methods-Across-Disciplines

## Description

This project develops a pipeline to use Large Language Models (LLMs) to identify and recommend artificial intelligence (AI), machine learning (ML), and deep learning (DL) methods in research papers outside of AI. The system first detects and extracts AI-related methodologies from abstracts, ensuring high accuracy through validation by multiple LLMs (GPT, Claude, Gemini). It then generates concise summaries of research ideas and objectives, explicitly removing any reference to computational techniques. Finally, the pipeline recommends the most suitable AI/ML/DL methods using both zero-shot and few-shot prompting strategies tailored to the research context. The goal is to support interdisciplinary collaboration by providing method suggestions that align with domain-specific research needs while maintaining transparency and rigor.



## Phases

Phase 1: Paper Fetching (OpenAlex API)
- Uses OpenAlex API to retrieve non-CS papers that mention AI/ML/DL concepts.
- Filters out papers where AI/ML is the primary topic.
- Saves abstracts and metadata for downstream processing.

Phase 2a: Method Extraction from Abstracts
- Uses GPT-4 to first define what qualifies as a valid "method" in the AI/ML/DL context.
- Prompts are tailored to extract only methods aligned with this definition from research abstracts.
- Each abstract is processed to identify methods, tagged using `<method>...</method>`.

Phase 2b: Tri-Model Validation (GPT + Claude + Gemini)
- Extracted methods are compiled into a candidate list.
- Each candidate is evaluated in parallel using GPT-4, Claude 3.5, and Gemini 1.5 Pro.
- A method is accepted **only if all three models agree** it is valid.
- Ensures high precision and filters out ambiguous or invalid terms.

Phase 2c: Filtering and Final Output Preparation
- Re-filters original extraction outputs to retain **only tri-model validated methods**.
- Cleans tagged methods (`<method>...</method>`) into structured Python lists.
- Saves the resulting dataset for Phase 3 summarization and downstream recommendation models.

Phase 3a: Extract Masked Summaries
- Extracts the *research idea* and *objective* from abstracts.
- Removes all references to computational techniques.
- OpenAI GPT is used for structured prompt-response extraction.

Phase 3b: Leakage Detection & Rewriting
- Scans Phase 3 output for ML-related terminology using a curated vocabulary list.
- Uses Claude to rewrite affected sentences, preserving meaning.
- Produces leakage-free, AI-neutral summaries.

Phase 4: Method Recommendation
- Suggests suitable ML/DL methods for a given domain-specific research summary.
- Variants:
  - Zero-shot (basic)
  - Zero-shot CoT (with step-by-step reasoning)
  - Few-shot CoT (with 3-shot and 5-shot examples)
- Supports multiple backends: OpenAI, Claude, and Ollama (LLaMA, Mistral, Gemma).




## Dependencies
- Python 3.9+
- OpenAI, Anthropic APIs
- LangChain
- Pandas, Requests, tqdm
- Ollama (optional for local models)

Install via:
```bash
pip install -r requirements.txt
```



## Directory Structure
```bash
.
├── OpenAlex/
│     └── Fatch Papers.ipynb  
├── Extraction/
│     └── Extraction.ipynb  
│     ├── stage 2a/  #
│     ├── stage 2b/  #
│     ├── stage 3c/  #
├── Summarization/
│     └── Summarization.ipynb  
│     ├── stage 3a/  # Masked summaries
│     └── stage 3b/  # Cleaned summaries
├── Recommendation/
│     ├── zero_shot/
│           └── Recommendation with zero-shot.ipynb 
│     ├── zero_shot_cot/
│           └── Recommendation with zero-shot CoT.ipynb 
│     ├── few_shot/
│           └── Recommendation with few-shot.ipynb    
```



## Run Instructions
```bash
# Run masked extraction
python phase3_extract_masked_summaries.py

# Detect & clean ML leaks
python phase3b_rewrite_leakage.py

# Recommend methods (zero-shot)
python phase4_recommend_zero_shot.py

# Recommend methods (zero-shot CoT)
python phase4_recommend_zero_shot_cot.py

# Recommend methods (few-shot 3 & 5)
python phase4_recommend_few_shot.py
```



## Contributing
Pull requests are welcome! Please make sure to:
- Follow the modular structure
- Document changes clearly
- Include evaluation scripts if possible




