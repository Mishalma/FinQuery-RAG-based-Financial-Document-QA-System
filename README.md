# ERC2 - Enterprise Report Comprehension Challenge

A comprehensive RAG (Retrieval-Augmented Generation) pipeline for processing and analyzing enterprise PDF reports to answer complex business questions. This system combines advanced PDF parsing, vector databases, BM25 retrieval, and LLM-based question answering to extract insights from financial and corporate documents.

## Features

- **Advanced PDF Processing**: Uses Docling for robust PDF parsing with table extraction and serialization
- **Hybrid Retrieval**: Combines vector databases (FAISS) and BM25 for optimal document retrieval
- **Multi-Model Support**: Compatible with OpenAI GPT models, IBM Llama, and Google Gemini
- **Parallel Processing**: Optimized for high-throughput document processing
- **Configurable Pipeline**: Multiple preset configurations for different use cases
- **Question Processing**: Handles complex business questions about company reports

## Quick Start

### Prerequisites

- Python 3.8+
- Required dependencies (see `requirements.txt`)

### Installation

1. Clone the repository:
```bash
git clone <repository-url>
cd erc2
```

2. Install dependencies:
```bash
pip install -r requirements.txt
```

3. Set up environment variables:
```bash
cp .env.example .env
# Edit .env with your API keys (OpenAI, Google, IBM, etc.)
```

### Basic Usage

The pipeline provides a CLI interface for all operations:

```bash
# Download required models
python main.py download-models

# Parse PDF reports
python main.py parse-pdfs --parallel --max-workers 10

# Serialize tables (optional)
python main.py serialize-tables --max-workers 10

# Process reports through pipeline
python main.py process-reports --config no_ser_tab

# Answer questions
python main.py process-questions --config base
```

## Pipeline Stages

### 1. PDF Parsing
Extracts text, tables, and metadata from PDF reports using Docling backend.

```bash
python main.py parse-pdfs [OPTIONS]
```

Options:
- `--parallel/--sequential`: Processing mode (default: parallel)
- `--chunk-size`: PDFs per worker (default: 2)
- `--max-workers`: Number of parallel workers (default: 10)

### 2. Table Serialization
Converts extracted tables to structured format for better retrieval.

```bash
python main.py serialize-tables --max-workers 10
```

### 3. Report Processing
Processes parsed reports through text splitting, chunking, and database ingestion.

```bash
python main.py process-reports --config [ser_tab|no_ser_tab]
```

### 4. Question Answering
Processes questions using the configured retrieval and generation pipeline.

```bash
python main.py process-questions --config [base|pdr|max|...]
```

Available configurations:
- `base`: Standard configuration
- `pdr`: Parent document retrieval
- `max`: Maximum performance settings
- `max_no_ser_tab`: Max performance without table serialization
- `ibm_llama70b`: IBM Llama 70B model
- `gemini_thinking`: Google Gemini with reasoning

## Configuration

### Pipeline Configurations

The system supports multiple preset configurations in `src/pipeline.py`:

- **Preprocessing configs**: Control table serialization
- **Runtime configs**: Control retrieval methods, model selection, and processing parameters

### Environment Variables

Required environment variables (add to `.env`):

```env
OPENAI_API_KEY=your_openai_key
GOOGLE_API_KEY=your_google_key
IBM_API_KEY=your_ibm_key
# Add other API keys as needed
```

## Project Structure

```
├── main.py                 # CLI entry point
├── src/
│   ├── pipeline.py         # Main pipeline orchestration
│   ├── pdf_parsing.py      # PDF processing with Docling
│   ├── retrieval.py        # Vector and BM25 retrieval
│   ├── reranking.py        # LLM-based result reranking
│   ├── ingestion.py        # Database ingestion
│   ├── questions_processing.py  # Question answering logic
│   └── ...
├── data/
│   ├── erc2_set/          # Full dataset
│   └── test_set/          # Test dataset
├── requirements.txt        # Python dependencies
└── subset.csv             # Document metadata
```

## Data Format

The system processes:
- **PDF Reports**: Corporate/financial documents
- **Questions**: Business questions in JSON format
- **Metadata**: Company information and document attributes in CSV format

Sample metadata fields:
- Company name, industry, currency
- Business indicators (M&A, leadership changes, R&D investment, etc.)
- Financial metrics and strategic information

## Advanced Usage

### Custom Configurations

Create custom pipeline configurations by modifying the config dictionaries in `src/pipeline.py`:

```python
custom_config = RunConfig(
    use_serialized_tables=True,
    parent_document_retrieval=True,
    use_vector_dbs=True,
    use_bm25_db=True
)
```

### Parallel Processing

The system supports parallel processing at multiple levels:
- PDF parsing with multiprocessing
- Table serialization with threading
- Batch question processing

### Debugging

Enable debug output by setting appropriate logging levels and using the debug data paths configured in the pipeline.

## Performance

The system is optimized for:
- Large-scale document processing (hundreds of PDFs)
- Complex question answering with multiple retrieval strategies
- Efficient memory usage with chunked processing
- Parallel execution for improved throughput

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests if applicable
5. Submit a pull request

## License

See `LICENSE` file for details.

## Support

For questions and issues, please refer to the documentation or create an issue in the repository.