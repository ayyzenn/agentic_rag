# 🧠 Agentic RAG System with Gemini & ChromaDB

A sophisticated **Agentic Retrieval-Augmented Generation (RAG)** system featuring intelligent routing, advanced retrieval techniques, and autonomous decision-making. This system uses a 3-agent architecture to provide optimal answers for both simple and complex queries.

## 🌟 Features

- **🤖 3-Agent Architecture**: Router, Basic Generator, and Advanced Generator agents work together intelligently
- **🧭 Intelligent Routing**: Automatically determines the best retrieval strategy for each query
- **🚀 Advanced RAG Techniques**: 
  - Query Decomposition (breaks complex queries into sub-queries)
  - HyDE (Hypothetical Document Embeddings)
  - Multi-Query Retrieval (generates query variations)
- **⚡ Fast & Efficient**: Uses ChromaDB for semantic search and Gemini Flash for generation
- **🎯 Adaptive**: Automatically switches from basic to advanced techniques when needed
- **📊 Quality Evaluation**: Built-in answer quality assessment and confidence scoring

## 🏗️ Architecture

```
User Query
    ↓
Router Agent (evaluates query complexity & routes)
    ↓
Basic Generator Agent (fast, simple retrieval)
    ↓
Router Agent (evaluates answer quality)
    ↓ (if insufficient)
Advanced Generator Agent
    ├── Query Decomposition
    ├── HyDE (Hypothetical Document Embeddings)
    └── Multi-Query Retrieval
    ↓
Final Answer
```

### Agent Overview

1. **Router Agent**: Analyzes queries, evaluates answer quality, and routes to appropriate generator
2. **Basic Generator Agent**: Fast, straightforward RAG for simple queries using standard retrieval
3. **Advanced Generator Agent**: Employs advanced techniques for complex queries requiring deeper analysis

## 📋 Prerequisites

- **Python 3.8+**
- **Google Gemini API Key** ([Get one here](https://aistudio.google.com/app/apikey))
- **4GB+ RAM** (for embeddings and model processing)

## 🚀 Quick Start

### 1. Clone the Repository

```bash
git clone <your-repo-url>
cd rag
```

### 2. Set Up Virtual Environment

```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

### 4. Configure Gemini API Key

Create a `.env` file in the project root:

```bash
echo "GEMINI_API_KEY=your_actual_api_key_here" > .env
```

Or manually create `.env` and add:
```
GEMINI_API_KEY=your_actual_api_key_here
```

### 5. Run the Agentic RAG System

```bash
python agentic_rag.py
```

## 📁 Project Structure

```
rag/
├── agentic_rag.py          # Main orchestrator and CLI
├── config.py               # Configuration settings
├── vector_store.py         # ChromaDB vector store wrapper
├── preprocess.py           # Document preprocessing with metadata
├── list_models.py          # Utility to list available Gemini models
├── agents/
│   ├── base_agent.py       # Base agent class with Gemini integration
│   ├── router_agent.py     # Router agent for query routing
│   ├── basic_generator.py  # Basic RAG generator
│   └── advanced_generator.py # Advanced RAG with multiple techniques
├── utils/
│   ├── prompt_templates.py  # Prompt templates for all agents
│   └── evaluator.py       # Answer quality evaluation
├── docs/                   # Knowledge base documents
│   ├── about_me.txt
│   ├── education.txt
│   ├── finance.txt
│   └── healthcare.txt
├── requirements.txt        # Python dependencies
├── LICENSE                # MIT License
├── SETUP.md               # Detailed setup guide
└── README.md              # This file
```

## 🔧 Usage

### Running the System

```bash
python agentic_rag.py
```

You'll be prompted to select an output mode:
- **silent**: Shows only the final answer
- **verbose**: Shows routing decision and which agent was used (default)
- **debug**: Shows all steps, evaluations, and detailed reasoning

### Example Queries

**Simple Query (handled by Basic Generator):**
```
Ask your question: What is AI in healthcare?
```

**Complex Query (handled by Advanced Generator):**
```
Ask your question: Compare and contrast AI applications in healthcare versus finance, 
including specific use cases, benefits, and challenges for each domain.
```

### Utility Scripts

**List Available Gemini Models:**
```bash
python list_models.py
```

This helps you identify which Gemini models are available with your API key.

## 🧠 Advanced RAG Techniques

### 1. Query Decomposition
Automatically breaks complex, multi-part queries into simpler sub-queries, retrieves relevant documents for each, and synthesizes a comprehensive answer.

**Example:**
- Original: "Compare AI in healthcare vs finance"
- Decomposed: 
  - "AI applications in healthcare"
  - "AI applications in finance"
  - Synthesized comparison

### 2. HyDE (Hypothetical Document Embeddings)
Generates a hypothetical "ideal answer" first, then uses its embedding to find similar real documents for more focused retrieval.

**Process:**
1. Generate hypothetical answer using LLM
2. Embed the hypothetical answer
3. Search for documents similar to the hypothetical answer
4. Generate grounded answer from real documents

### 3. Multi-Query Retrieval
Creates multiple phrasings of the query to capture different perspectives and improve retrieval coverage.

**Example:**
- Original: "How is AI used in medicine?"
- Variations:
  - "Medical AI applications"
  - "Artificial intelligence healthcare use cases"
  - "AI technologies in clinical settings"

## 📊 Dependencies

| Package | Version | Purpose |
|---------|---------|---------|
| `chromadb` | >=0.4.0 | Vector database |
| `sentence-transformers` | >=2.0.0 | Text embeddings |
| `google-generativeai` | >=0.3.0 | Gemini API client |
| `python-dotenv` | >=1.0.0 | Environment variable management |
| `numpy` | >=1.21.0 | Numerical computations |
| `torch` | >=1.9.0 | PyTorch backend |

## 🛠️ Customization

### Configuration

Edit `config.py` to customize:

- **Model Selection**: Change `GEMINI_MODEL` (default: `gemini-2.5-flash`)
- **Agent Parameters**: Adjust temperature, max_tokens, etc.
- **Retrieval Settings**: Number of chunks per query
- **Evaluation Thresholds**: Answer quality scoring criteria
- **Chunking**: Word limits and overlap settings

### Adding Your Own Documents

1. Place your `.txt` files in the `docs/` directory
2. Run the system - it will automatically process new documents
3. Documents are chunked with 20% overlap for better context preservation

### Modifying Chunk Size

Edit `config.py`:
```python
CHUNK_CONFIG = {
    "max_words": 100,      # Chunk size
    "overlap_words": 20,   # Overlap between chunks
}
```

## 🔍 How It Works

### Routing Flow

1. **Query Reception**: User submits query
2. **Basic Generation**: Router sends query to Basic Generator
   - Simple vector retrieval (top 3 chunks)
   - Straightforward LLM generation
3. **Quality Evaluation**: Router evaluates answer completeness and confidence
4. **Advanced Generation** (if needed):
   - Query Decomposition: Break into sub-queries
   - HyDE: Generate hypothetical answer and search
   - Multi-Query: Generate query variations
   - Combine results and synthesize final answer
5. **Response**: Return final answer with metadata

### Answer Evaluation

The Router Agent uses LLM-based evaluation to assess:
- **Completeness**: Does the answer fully address the question?
- **Relevance**: Is the answer relevant to the query?
- **Confidence**: How confident is the system in the answer?

## 🎯 Performance

- **Response Time**: ~3-8 seconds (basic) or ~10-20 seconds (advanced)
- **Memory Usage**: ~2-3GB (embeddings + models)
- **Storage**: ~500MB for embeddings and documents
- **Accuracy**: High relevance, especially for complex multi-part queries

## 🔄 Workflow Example

```
Query: "Compare AI in healthcare and finance"

[Router] → Using Basic Generator Agent
[Basic] Retrieved 3 chunks
[Router] Evaluating answer... Insufficient
[Router] → Using Advanced Generator Agent

[Advanced/Decomposition] Generated 3 sub-queries
[Advanced/HyDE] Generated hypothetical answer
[Advanced/Multi-Query] Generated 4 query variations
[Advanced] Combined 12 unique chunks
[Advanced] Final answer generated

Answer: [Comprehensive comparison with specific examples]
```

## 🚧 Future Enhancements

- [ ] Web interface with Gradio/Streamlit
- [ ] Support for PDF and Word documents
- [ ] Additional retrieval techniques (RAG-Fusion, etc.)
- [ ] Conversation memory and context management
- [ ] Fine-tuned evaluation models
- [ ] Multi-modal support (images, tables)
- [ ] Streaming responses
- [ ] Batch processing for multiple queries

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## 📝 License

This project is open source and available under the [MIT License](LICENSE).



*Built with ❤️ for intelligent, adaptive AI applications*
