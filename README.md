# EV_CHATBOT 🚗⚡

A smart terminal-based chatbot for Electric Vehicle data (attached the data folder)! Built with Python and powered by Groq's lightning-fast LLM API.

## Project Structure 📁

```
New_EV_Chatbot/
├── venv/
│   ├── main.py              # Main chat interface
│   ├── query_engine.py      # Groq API setup
│   ├── requirements.txt     # Dependencies
│   ├── .env                 # Environment variables
│   └── .env.example         # Environment template
├── .gitignore
└── README.md
```

## Quick Setup 🚀

### 1. Clone & Navigate
```bash
cd "New_EV_Chatbot/venv"
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

## Features ✨

- **Real-time Streaming**: Responses appear as they're generated
- **Natural Conversations**: Ask follow-up questions naturally
- **No Rate Limits**: Powered by Groq's fast API
- **Clean Terminal UI**: Simple and distraction-free
- **Easy Exit**: Type 'exit' or 'quit' to leave

## Technical Details 🔧

### Architecture Overview
The EV Chatbot follows a modular architecture with clear separation of concerns:

```
┌─────────────────┐    ┌──────────────────┐    ┌─────────────────┐
│   main.py       │───▶│  query_engine.py │───▶│   Groq API      │
│ (User Interface)│    │ (Logic Layer)    │    │ (LLM Service)   │
└─────────────────┘    └──────────────────┘    └─────────────────┘
                                │
                                ▼
                       ┌─────────────────┐
                       │ Supabase DB     │
                       │ (Data Source)   │
                       └─────────────────┘
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

#### 3. **Environment Configuration**
- **`.env`**: Stores sensitive credentials (API keys, DB credentials)
- **`.env.example`**: Template for users to set up their own environment
- **`python-dotenv`**: Loads environment variables securely

### Data Flow Process

1. **User Input**: User types question in terminal
2. **Database Query**: System fetches relevant EV data from Supabase
3. **Context Enhancement**: User query + database context combined
4. **LLM Processing**: Enhanced query sent to Groq's Llama 3.3 70B model
5. **Streaming Response**: Real-time response generation and display
6. **Loop Continuation**: Process repeats until user exits

### LLM Configuration
- **Model**: Llama 3.3 70B Versatile (via Groq API)
- **Temperature**: 1.0 (balanced creativity and accuracy)
- **Max Tokens**: 1024 per response
- **Streaming**: Real-time token generation
- **Top-p**: 1.0 (full vocabulary consideration)

### Database Integration
- **Database**: PostgreSQL (Supabase)
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
- **Connection Pooling**: Efficient database connections
- **Minimal Dependencies**: Fast startup and low resource usage
- **Warning Suppression**: Clean terminal output

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

## What's Next? 🚀

- [ ] Add RAG integration for custom EV documents
- [ ] Web interface with Streamlit
- [ ] Voice input/output
- [ ] Save conversation history
- [ ] Multi-language support

## Contributing 🤝

Feel free to fork, modify, and improve! This is a learning project so any suggestions are welcome.

## License 📄

Open source 

---

** Project by Kaushal🚗⚡**