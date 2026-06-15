# Smart CLI Chatbot with Memory

A powerful command-line chatbot built with LangChain and LLaMA 3.2, featuring intelligent tool integration and persistent conversation memory. This project demonstrates advanced agentic AI capabilities with multi-turn context awareness and automatic state resumption.

## Projects Overview

### Project 1: Smart CLI Chatbot with Multi-Tool Integration
A CLI-based assistant with access to real-time data and external APIs:
- **Weather API Integration** - Fetch current weather data for any city using Open-Meteo
- **Wikipedia Search** - Retrieve factual information about people, events, places, and concepts
- **Movie Recommendations** - Get genre-based movie suggestions using TMDB API
- **Movie Similarity Engine** - Find similar movies and user-recommended titles
- **Mathematical Calculations** - Handle complex mathematical expressions

### Project 2: Enhanced Chatbot with Conversation Memory
Extended the original chatbot with sophisticated memory management:
- **Persistent Session Memory** - Maintain conversation history across multiple turns
- **Context-Aware Responses** - Bot remembers previous interactions and references
- **State Management** - Automatic session resumption using LangGraph checkpointing
- **InMemorySaver** - Modern approach to memory management (superior to legacy ConversationBufferMemory)

## Features

✨ **Multi-Turn Conversations** - Maintain context across multiple user interactions
🛠️ **Automated Tool Calling** - Agent intelligently selects and invokes appropriate tools
🌍 **Real-Time Data** - Live weather, Wikipedia, and movie data integration
💾 **Session Persistence** - Resume conversations with full context awareness
🤖 **LLaMA 3.2 Integration** - Powered by Ollama local LLM
🔄 **Agentic Workflow** - LangGraph-based orchestration for complex interactions

## Technology Stack

- **LLM Framework**: LangChain, LangGraph
- **Language Model**: LLaMA 3.2 (via Ollama)
- **External APIs**:
  - Open-Meteo (Weather data)
  - Wikipedia API
  - TMDB (The Movie Database)
  - Nominatim (Geolocation)
- **Dependencies**: Python 3.8+, requests, wikipediaapi, dotenv

## Installation

### Prerequisites
- Python 3.8 or higher
- Ollama with LLaMA 3.2 model installed
- API keys for TMDB (optional, for movie features)

### Setup

1. **Clone and navigate to the project:**
   ```bash
   cd smart-cli-chatbot
   ```

2. **Create a virtual environment:**
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. **Install dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

4. **Configure environment variables:**
   Create a `.env` file in the project root:
   ```
   TMDB_API=your_tmdb_api_key_here
   OLLAMA_MODEL=llama3.2
   ```

5. **Ensure Ollama is running:**
   ```bash
   ollama serve
   ```

## Usage

### Running the Chatbot

```python
python run_chatbot()
```

**Example Interaction:**
```
You: What's the weather in Tokyo?
Bot: Weather for Tokyo, Japan:
     - Temperature: 28.5°C
     - Feels Like: 29.1°C
     - Humidity: 65%

You: That's warm! Tell me about the history of Japan.
Bot: Title: History of Japan
     Summary: Japan is an island nation in East Asia...

You: Recommend some action movies.
Bot: [List of top-rated action movies with ratings and release dates]
```

### Using the Agent Directly

```python
from langchain_core.runnables import RunnableConfig

config = RunnableConfig(configurable={"thread_id": "user_session_1"})

# First turn
response1 = agent.invoke(
    {"messages": [("user", "What is the weather in Tokyo?")]},
    config=config
)
print(response1["messages"][-1].content)

# Second turn - bot remembers previous context
response2 = agent.invoke(
    {"messages": [("user", "I forgot what city we were talking about")]},
    config=config
)
print(response2["messages"][-1].content)  # Bot will reference Tokyo
```

## Project Structure

```
smart-cli-chatbot/
├── w1 and w2.ipynb          # Project implementations and experiments
├── w3.ipynb                 # PDF document Q&A extension (LangChain basics)
├── README.md                # This file
├── .env                      # Environment variables (not in version control)
└── requirements.txt         # Python dependencies
```

## Key Concepts

### Conversation Memory Approaches

This project demonstrates the evolution of memory management in LangChain:

1. **ConversationBufferMemory (Legacy)** ❌
   - Stores all messages in a simple list
   - High token usage on long conversations
   - Limited support for complex agent loops

2. **InMemorySaver with LangGraph (Modern)** ✅
   - Captures exact workflow state at each step
   - Automatic thread continuity
   - Supports advanced features like time-travel
   - Efficient token management

3. **RunnableWithMessageHistory (Current Standard)** ✅
   - LangChain Expression Language (LCEL) approach
   - Wraps agent chains with dynamic history injection
   - Session-ID based memory management

### Tool Architecture

Tools are decorated with `@tool` and automatically managed by the agent:
- **Weather Tool** - Geolocation + real-time weather API
- **Wikipedia Tool** - Factual information retrieval
- **Movie Tools** - Genre-based and similarity-based recommendations

The agent uses ReAct (Reasoning + Acting) pattern to:
1. Understand user intent
2. Select appropriate tools
3. Execute tool calls
4. Synthesize results
5. Provide conversational response

## API References

### Open-Meteo Weather API
- Free, no authentication required
- Endpoint: `https://api.open-meteo.com/v1/forecast`

### TMDB Movie Database
- Requires free API key from [themoviedb.org](https://www.themoviedb.org)
- Endpoints: Genre list, discover, search, similar, recommendations

### Wikipedia API
- Free access via `wikipediaapi` library
- No authentication required

## Performance Considerations

- **Token Usage**: InMemorySaver is more efficient than ConversationBufferMemory
- **Latency**: Tool calls add ~500ms-2s per API request
- **Session Storage**: In-memory; use persistent storage for production
- **Rate Limiting**: Respect API rate limits (especially TMDB and Open-Meteo)

## Troubleshooting

### Issue: "Model not found" error
**Solution**: Ensure Ollama is running and LLaMA 3.2 is installed:
```bash
ollama pull llama3.2
```

### Issue: TMDB API returns 401 error
**Solution**: Verify your API key is correct in the `.env` file

### Issue: Geolocation failures
**Solution**: Check internet connection and Nominatim service availability

## Future Enhancements

- 🔐 Persistent database storage for session history
- 📊 User interaction analytics
- 🗣️ Multi-language support
- 🎬 Extended media integration (music, books)
- 🔄 Distributed memory for multi-user scenarios
- 🧪 Comprehensive test suite

## Learning Outcomes

This project demonstrates:
- Building agentic AI systems with LangChain/LangGraph
- Integrating multiple external APIs
- Implementing conversation memory and state management
- Creating structured tool interfaces
- Multi-turn reasoning with context awareness
- Production-ready error handling

## References

- [LangChain Documentation](https://python.langchain.com/)
- [LangGraph Documentation](https://langchain-ai.github.io/langgraph/)
- [Ollama Documentation](https://ollama.ai/)
- [TMDB API](https://www.themoviedb.org/settings/api)
- [Open-Meteo API](https://open-meteo.com/)

## License

This project is open source and available under the MIT License.

## Author

Created as an educational project to explore advanced LLM applications and agentic AI patterns.

---

**Last Updated:** June 2026
