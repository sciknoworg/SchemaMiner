<p align="center">
<img width="450" src="assets/schema-miner-logo.png" alt="schema-miner logo" />
</p>

<div align="center">

![Maintained Yes](https://img.shields.io/badge/maintained-yes-green)
[![MIT License](https://img.shields.io/github/license/sciknoworg/schema-miner)](LICENSE)

</div>

**LLMs4SchemaDiscovery: A Human-in-the-Loop Workflow for Scientific Schema Mining with Large Language Models**

This is an open-source implementation of Schema-miner (LLMs4SchemaDiscovery).

## 📋 Schema-miner Overview

Schema-miner (LLMs4SchemaDiscovery) is novel framework that leverages Large Language Models (LLMs) and continuous human feedback to automate and enhance schema mining task. Through an iterative process, the framework uses LLMs to extract and organize properties from unstructured text, refine schemas with expert input, and incorporate domain-specific ontologies to add semantic knowledge.

<p align="center">
  <img src="assets/LLM4SchemaMining - Workflow design.svg" height="300">
</p>

<p align="center">
  Figure 1: Overview of the LLMs4SchemaDiscovery workflow.
</p>

## 🧪 Installation

Install all the necessary Python packages listed in the requirements.txt [requirements.txt](requirements.txt) file.

```bash
pip install -r requirements.txt
```

## 🚀 Quick Start

For a quick start, see the provided example notebooks highlighting the overall workflows of the schema-miner.

<div align="center">

|  | Notebook |
| --- |----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1 | [Schema Mining With LLMs and expert Example](notebooks/schema_mining_with_LLMs_and_expert_example.ipynb) |

</div>

##  🧑‍💻 Schema-miner Tool Usage

### 🛠️ Configuration

Parameters such as API keys, base URLs, and model settings are managed using a dedicated environment file. The .env file has to be saved at the root directory of this project.

### 🗂️ Preparing Knowledge Base

The knowledge base includes an initial domain specification document and relevant research papers for stages 2 and 3. For machine processing, the input documents are in plain text format. If research papers are in PDF format, the pdf to text conversion [script](src/experiments/file_text_extractor.py) can be used. The script takes the directory of the PDFs and the directory to save the converted text files.

```bash
$ python file_text_extractor.py

LLMs4SchemaDiscovery Framework -- A Human-in-the-Loop Workflow for Scientific Schema Mining with Large Language Models 
Formatting Knowledge Base - Converting PDF Documents to Text Files

Please input the directory location containing the PDF documents  
Directory Path> data/research-papers

Please input the directory Location to store the converted text files
Directory Path> data/research-papers

Extracting text from all PDFs from the directory: data/research-papers

Extracting text from the PDF: ALD-E_Simulation-Parameters-Observables-List.pdf
Text file saved successfully: True

PDF documents successfully convert to text format!
```

### 📊 Generating Initial Schema from Stage 1

An initial JSON schema can be generated based on the domain specification document and the preferred LLM. Stage 1 can be executed using this [script](src/experiments/schema_extraction/schema_extraction_stage1.py). An example run is show below:

```bash
$ python schema_extraction_stage1.py

LLMs4SchemaDiscovery Framework -- A Human-in-the-Loop Workflow for Scientific Schema Mining with Large Language Models
Stage 1: Initial Schema Mining

Please specify the LLM name to perform schema mining...
List of possible LLMs:
    1. OPENAI Models:
        - gpt-4o
        - gpt-4-turbo
    2. LLAMA Models:
        - meta-llama-3.1-8b-instruct
    3. Any other local OLLAMA Model

LLM> gpt-4o

Please specify the location of the process specification document
Document location> data/stage-1/ALD-Process-Development.txt

Please specify the location to save the schema
Schema location> results/stage-1

Performing LLM (gpt-4o) Inference to extract schema...
Using OPENAI - LLM Inference with Model: gpt-4o

Writing the models response to the file at the specified location: results/stage-1

Extracting the JSON object from the models output...
JSON schema Saved at location: results/stage-1
```

### 📊🔄 Refining Stage 1 Schemas with scientific literature and human feedback

The schema from stage 1 can refined by the LLM iteratively by analyzing a curated set of research papers and incorporating expert feedback. Stage 2 can be executed using this [script](src/experiments/schema_extraction/schema_extraction_stage2.py). An example run is show below:

```bash
$python schema_extraction_stage2.py

LLMs4SchemaDiscovery Framework -- A Human-in-the-Loop Workflow for Scientific Schema Mining with Large Language Models 
Stage 2: Preliminary Schema Refinement

Please specify the LLM name to perform schema mining...
List of possible LLMs:
    1. OPENAI Models:
        - gpt-4o
        - gpt-4-turbo
    2. LLAMA Models:
        - meta-llama-3.1-8b-instruct
    3. Any other local OLLAMA Model

LLM> gpt-4o

Please specify the location to the schema from stage 1
Stage 1 - schema location> results/stage-1

Please specify the location to the domain-expert feedbacks on stage 1 schema
Expert feedback location> data/stage-2/domain-expert-reviews/experimental-usecase/method-1

Please specify the location to the small domain-expert curated collection of research papers
Research papers location> data/stage-2/research-papers/experimental-usecase

Please specify the location to save the schema
Schema location> results/stage-2

Performing LLM (gpt-4o) Inference to extract updated experimental schema...
Using OPENAI - LLM Inference with Model: gpt-4o

Reading the schema from stage-1: results/stage-1/gpt-4o.json

Reading the domain expert review on the initial schema: data/stage-2/domain-expert-reviews/experimental-usecase/method-1/gpt-4o.txt

========================== Iteration 1 =================================

Reading the scientific paper: data/stage-2/research-papers/experimental-usecase/1 Groner et al.txt

Calling the completion API of the model
Writing the models response to the file at the specified location: results/stage-2

Do you want to provide feedback?
Feedback (yes/no)> yes

Please input your feedback
Feedback> The reactivity should be mentioned either at the process conditions or film properties. It’s the result of deposition under specific conditions not a standard property of the precursor or co-reactant.

Do you want to continue with the next paper?
Continue (yes)/Stop (no)> yes

========================== Iteration 2 =================================
Reading the scientific paper: data/stage-2/research-papers/experimental-usecase/2 Aaltonen et al.txt

Calling the completion API of the model
Writing the models response to the file at the specified location: results/stage-2

Do you want to provide feedback?
Feedback (yes/no)> no

Do you want to continue with the next paper?
Continue (yes)/Stop (no)> no
JSON schema updated with LLM: gpt-4o
```

Stage 3, validates and finalizes the schema using a larger, uncurated corpus of research papers, ensuring generalizability and semantic robustness. Stage 3 can be executed using this [script](src/experiments/schema_extraction/schema_extraction_stage3.py). The execution of stage 3 is similar to stage 2 except only the scientific corpus is been changed.

## 📃 License

This work is licensed under a [MIT License](LICENSE.txt)