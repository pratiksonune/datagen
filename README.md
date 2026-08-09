# RAFT Training Data Generator

A Streamlit application for generating Retrieval Augmented Fine-Tuning (RAFT) training data from PDF documents. Based on the RAFT methodology from UC Berkeley's Gorilla project.

## Overview

This tool converts PDF documents into high-quality training data for fine-tuning language models with Retrieval Augmented Generation capabilities. The generated data includes question-answer pairs with chain-of-thought reasoning, along with oracle and distractor documents to teach models how to distinguish relevant from irrelevant information.

## Features

- **PDF Processing**: Converts PDF pages to images and extracts markdown content using GPT-4 Vision
- **Document Chunking**: Splits documents into meaningful chunks using markdown headers and recursive character splitting
- **QA Pair Generation**: Creates questions and chain-of-thought answers for each document chunk
- **Distractor Documents**: Includes irrelevant document chunks to train models on context discrimination
- **Dataset Splitting**: Automatically splits generated data into training, validation, and test sets
- **Multiple Export Formats**: Supports JSONL (chat format and full data) and CSV exports
- **Configurable Parameters**: Adjust questions per chunk, distractor count, oracle probability, temperature, and dataset splits

## Installation

### Prerequisites

- Python 3.8 or higher
- OpenAI API key
- Poppler utilities (for PDF to image conversion)

### System Dependencies

**macOS:**
```bash
brew install poppler
```

**Ubuntu/Debian:**
```bash
apt-get install poppler-utils
```

**Windows:**
Download and install from [poppler-windows](https://github.com/oschwartz10612/poppler-windows/releases)

### Python Dependencies

```bash
pip install -r requirements.txt
```

## Usage

1. Start the Streamlit application:
```bash
streamlit run streamlit_app.py
```

2. Enter your OpenAI API key in the sidebar

3. Configure the generation parameters:
   - **Vision Model**: Select model for PDF to markdown conversion (GPT-4o or GPT-4o-mini)
   - **QA Model**: Select model for question-answer generation
   - **Questions per Chunk**: Number of questions to generate per document chunk
   - **Distractor Documents**: Number of irrelevant documents to include with each question
   - **Oracle Probability**: Probability of including the relevant document in context
   - **Temperature**: Creativity level for question generation
   - **Dataset Split**: Configure training/validation/test split percentages

4. Upload a PDF file and click "Process PDF and Generate Training Data"

5. Review the generated datasets and download in your preferred format

## Project Structure

```
datageneration/
├── streamlit_app.py          # Main entry point
├── raft_datagen.py           # Core RAFT data generation logic
├── data_generation.py        # Question and answer generation functions
├── pdf_processor.py          # PDF to image and markdown conversion
├── text_processing.py        # Text cleaning and document chunking
├── requirements.txt          # Python dependencies
└── packages.txt              # System dependencies
```

## Output Format

The tool generates datasets with the following columns:

- `id`: Unique identifier for the data point
- `type`: Type of the data (typically "general")
- `question`: The generated question
- `context`: Collection of documents including oracle and distractor documents
- `oracle_context`: The original document chunk containing answer information
- `cot_answer`: Chain-of-thought answer generated from the oracle context
- `instruction`: Formatted input combining context documents with the question

## Export Formats

- **JSONL (Chat Format)**: Messages format suitable for OpenAI fine-tuning API
- **JSONL (Full Data)**: Complete data structure with all fields
- **CSV**: Tabular format for analysis and inspection

## RAFT Methodology

This implementation follows the RAFT (Retrieval Augmented Fine-Tuning) approach described in the paper "RAFT: Adapting Language Model to Domain Specific RAG" by Zhang et al. The key principles:

1. Train models to answer questions using provided context
2. Include distractor documents to teach context discrimination
3. Vary oracle document inclusion to teach when information is missing
4. Generate chain-of-thought reasoning for improved answer quality

## Configuration Options

### System Prompts

Customize the system prompts for question and answer generation in the sidebar to adapt to your specific domain or requirements.

### Chunking Parameters

Document chunking uses:
- Markdown header splitting (H1, H2)
- Recursive character splitting with 1024 token chunks and 50 token overlap
- Minimum content threshold filtering

## License

MIT License