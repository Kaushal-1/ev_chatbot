# EV_CHATBOT 🚗⚡

A smart terminal-based chatbot for Electric Vehicle industry analysis! Built with Python, powered by Groq's lightning-fast LLM API, and enhanced with RAG (Retrieval-Augmented Generation) using real company data from TVS, Bajaj Electric, and Hero MotoCorp.

## Project Structure 📁

```
New_EV_Chatbot/
├── main.py              # Main chat interface
├── query_engine.py      # Groq API setup
├── app.py               # Streamlit web interface (optional)
├── requirements.txt     # Dependencies
├── .env                 # Environment variables (not in repo)
├── .env.example         # Environment template
├── rag/                 # RAG pipeline components
│   ├── pdf_loader.py    # Document processing
│   ├── vectorstore.py   # ChromaDB integration
│   └── test_*.py        # Testing utilities
├── db/                  # Database utilities
│   └── db_utils.py      # Supabase connection
├── data/                # Processed data storage
│   └── output/
│       └── chroma/      # Vector database files
├── .gitignore
└── README.md
```

## Quick Setup 🚀

### 1. Clone & Navigate
```bash
cd "New_EV_Chatbot"
```

### 2. Install Dependencies
```bash
pip install -r requirements.txt
```

### 3. Setup Environment Variables
```bash
cp .env.example .env
```
Then edit `.env` and add your Groq API key from [console.groq.com](https://console.groq.com/)

### 4. Run the Chatbot
```bash
python main.py
```

That's it! The chatbot will start loading and you'll see:
```
🧠 Loading chatbot...
✅ Chatbot ready. Ask your questions!

💬 You: 
```

## How to Use 💬

Just type your questions and hit Enter. The AI will respond in real-time with streaming text.

To exit, type `exit` or `quit`.

## Data Sources 📊

The chatbot is powered by comprehensive EV industry data:

### Company Annual Reports (2021-2023)
- **TVS Motor Company**: Annual reports and financial statements
- **Bajaj Auto (Electric Division)**: EV segment performance and strategy
- **Hero MotoCorp**: Electric vehicle initiatives and market analysis

### Industry Reports
- EV market trends and forecasts
- Government policy documents
- Industry research and analysis reports
- Technical specifications and standards

## Features ✨

- **RAG-Powered Responses**: Answers based on real company data and industry reports
- **Real-time Streaming**: Responses appear as they're generated
- **Document-Grounded**: All responses backed by actual company filings
- **Natural Conversations**: Ask follow-up questions naturally
- **Multi-Source Integration**: Combines data from multiple companies and sources
- **Clean Terminal UI**: Simple and distraction-free
- **Easy Exit**: Type 'exit' or 'quit' to leave

## Technical Details 🔧

### Architecture Overview
The EV Chatbot follows a RAG-enhanced architecture with document processing and vector search:

```
┌─────────────────┐    ┌──────────────────┐    ┌─────────────────┐
│   main.py       │───▶│  query_engine.py │───▶│   Groq API      │
│ (User Interface)│    │ (Logic Layer)    │    │ (LLM Service)   │
└─────────────────┘    └──────────────────┘    └─────────────────┘
                                │
                                ▼
                       ┌─────────────────┐
                       │ Vector Search   │
                       │ (ChromaDB)      │
                       └─────────────────┘
                                │
                                ▼
                       ┌─────────────────┐
                       │ Document Store  │
                       │ (Company Data)  │
                       └─────────────────┘
```

### RAG Pipeline Process

```
┌─────────────────┐    ┌──────────────────┐    ┌─────────────────┐
│ PDF Documents   │───▶│ Text Chunking    │───▶│ HuggingFace     │
│ (Annual Reports)│    │ (rag/pdf_loader) │    │ Embeddings      │
└─────────────────┘    └──────────────────┘    └─────────────────┘
                                                         │
                                                         ▼
┌─────────────────┐    ┌──────────────────┐    ┌─────────────────┐
│ User Query      │◄───│ Similarity Search│◄───│ ChromaDB        │
│ + Context       │    │ (Top-K Retrieval)│    │ Vector Store    │
└─────────────────┘    └──────────────────┘    └─────────────────┘
```

### Core Components

#### 1. **main.py** - User Interface Layer
- **Purpose**: Handles user interaction and chat loop
- **Key Functions**:
  - Initializes chatbot client
  - Manages conversation flow
  - Processes user input/output
  - Integrates database context with user queries

#### 2. **query_engine.py** - Business Logic Layer
- **Purpose**: Manages API connections and data retrieval
- **Key Functions**:
  - `get_chatbot()`: Initializes Groq client with API authentication
  - `get_db_context()`: Retrieves relevant data from Supabase database
  - Environment variable management
  - Error handling for missing dependencies

#### 3. **rag/** - Document Processing Pipeline
- **Purpose**: Handles document ingestion and vector storage
- **Key Components**:
  - `pdf_loader.py`: Extracts and chunks text from company PDFs
  - `vectorstore.py`: Creates and manages ChromaDB vector embeddings
  - Uses HuggingFace sentence-transformers for text embeddings
  - Implements semantic search for relevant document retrieval

#### 4. **Environment Configuration**
- **`.env`**: Stores sensitive credentials (API keys, DB credentials)
- **`.env.example`**: Template for users to set up their own environment
- **`python-dotenv`**: Loads environment variables securely

### Data Flow Process

1. **Document Ingestion** (One-time setup):
   - Annual reports from TVS, Bajaj Electric, Hero MotoCorp (2021-2023)
   - Industry reports and policy documents
   - Text extraction and chunking via `pdf_loader.py`
   - Embedding generation using HuggingFace sentence-transformers
   - Vector storage in ChromaDB via `vectorstore.py`

2. **Query Processing** (Runtime):
   - **User Input**: User types question in terminal
   - **Vector Search**: Query embedded and matched against document chunks
   - **Context Retrieval**: Most relevant company data retrieved from ChromaDB
   - **Database Query**: Additional context from Supabase (if available)
   - **Context Enhancement**: User query + retrieved documents + database context
   - **LLM Processing**: Enhanced query sent to Groq's Llama 3.3 70B model
   - **Streaming Response**: Real-time response generation with source grounding
   - **Loop Continuation**: Process repeats until user exits

### LLM Configuration
- **Model**: Llama 3.3 70B Versatile (via Groq API)
- **Temperature**: 1.0 (balanced creativity and accuracy)
- **Max Tokens**: 1024 per response
- **Streaming**: Real-time token generation
- **Top-p**: 1.0 (full vocabulary consideration)

### Vector Database Integration
- **Primary Storage**: ChromaDB for document vectors
- **Embeddings**: HuggingFace sentence-transformers/all-MiniLM-L6-v2
- **Chunking Strategy**: Semantic text segmentation for optimal retrieval
- **Search Method**: Cosine similarity for document matching
- **Persistence**: Local ChromaDB storage in `data/output/chroma/`

### Relational Database Integration
- **Secondary Storage**: PostgreSQL (Supabase)
- **Connection**: psycopg2-binary driver
- **Authentication**: Environment-based credentials
- **Query Strategy**: Dynamic table discovery and context retrieval
- **Fallback**: Graceful degradation when DB unavailable

### Security Features
- **API Key Protection**: Stored in .env, excluded from version control
- **Database Credentials**: Encrypted connection to Supabase
- **Input Sanitization**: Safe query handling
- **Error Handling**: Prevents credential exposure in error messages

### Performance Optimizations
- **Streaming Responses**: Immediate user feedback
- **Vector Caching**: Pre-computed embeddings for fast retrieval
- **Efficient Chunking**: Optimized text segmentation for better context
- **Connection Pooling**: Efficient database connections
- **Minimal Dependencies**: Fast startup and low resource usage
- **Warning Suppression**: Clean terminal output

### Document Processing Details
- **Supported Formats**: PDF (annual reports, industry documents)
- **Text Extraction**: PyMuPDF for reliable PDF processing
- **Chunking Strategy**: Semantic segmentation preserving context
- **Embedding Model**: sentence-transformers/all-MiniLM-L6-v2 (384 dimensions)
- **Vector Storage**: ChromaDB with persistence
- **Retrieval**: Top-K similarity search (configurable K value)

## Configuration ⚙️

The API key is loaded from the `.env` file. If you need to change it:

1. Get your API key from [Groq Console](https://console.groq.com/)(free signup and use)
2. Edit your `.env` file:
```
GROQ_API_KEY=your_new_api_key_here
```

## Troubleshooting 🔧

### "ModuleNotFoundError"
```bash
pip install groq python-dotenv
```

### "API Key Error"
Check that your Groq API key is valid and has credits.

### "Connection Error"
Make sure you have internet connection for API calls.

## Example Queries 💡

Try asking questions about the companies and industry:

- "What is TVS's electric vehicle strategy for 2023?"
- "Compare Bajaj and Hero's EV market performance"
- "What are the key challenges in EV adoption according to industry reports?"
- "Show me TVS's financial performance in the EV segment"
- "What government policies are mentioned in the company reports?"

## What's Next? 🚀

- [x] RAG integration with company annual reports
- [x] ChromaDB vector storage
- [x] HuggingFace embeddings
- [ ] Web interface with Streamlit
- [ ] Voice input/output
- [ ] Save conversation history
- [ ] Multi-language support
- [ ] Real-time data updates

## Data Sources Attribution 📚

- **TVS Motor Company**: Annual reports and investor presentations
- **Bajaj Auto**: Electric vehicle segment reports and financial data
- **Hero MotoCorp**: EV strategy documents and market analysis
- **Industry Reports**: Various EV market research and policy documents

*All data used is from publicly available sources and company filings.*

## Contributing 🤝

Feel free to fork, modify, and improve! This is a learning project so any suggestions are welcome.

## License 📄

Open source 

---

** Project by Kaushal🚗⚡**