# Enhanced Q&A Chatbot With Groq

A simple Streamlit chatbot that uses LangChain and Groq chat models to answer user questions. The app provides a browser UI where users enter a Groq API key, choose a model, adjust response settings, and ask questions.

## Features

- Streamlit web interface
- Groq-powered chat responses through `langchain-groq`
- LangChain prompt pipeline using `ChatPromptTemplate`
- Plain string output parsing with `StrOutputParser`
- Sidebar controls for:
  - Groq API key
  - Groq model selection
  - Temperature
  - Maximum tokens
- LangSmith tracing configuration for the `qna-chatbot` project

## Project Structure

```text
.
|-- app.py              # Main Streamlit application
|-- requirements.txt    # Python dependencies
|-- .env                # Local environment variables, not committed
|-- .gitignore          # Ignored files and folders
`-- README.md           # Project documentation
```

## Requirements

- Python 3.9 or newer
- A Groq API key
- A LangSmith API key if you want tracing enabled

## Environment Variables

Create a `.env` file in the project root.

```env
LANGCHAIN_API_KEY=your_langsmith_api_key_here
LANGCHAIN_PROJECT=qna-chatbot
```

Notes:

- The Groq API key is entered in the Streamlit sidebar at runtime.
- `LANGCHAIN_API_KEY` is used for LangSmith tracing.
- `LANGCHAIN_TRACING_V2` is enabled in `app.py`.
- The code currently hardcodes `LANGCHAIN_PROJECT` as `qna-chatbot`.

## Installation

From the project directory:

```powershell
python -m venv venv
.\venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

If `from dotenv import load_dotenv` fails, install `python-dotenv`:

```powershell
pip install python-dotenv
```

## Run The App

```powershell
streamlit run app.py
```

Streamlit will start a local server and print a browser URL, usually:

```text
http://localhost:8501
```

## How To Use

1. Start the Streamlit app.
2. Enter your Groq API key in the sidebar.
3. Select one of the available Groq models:
   - `llama-3.1-8b-instant`
   - `llama-3.3-70b-versatile`
   - `qwen/qwen3.6-27b`
4. Adjust temperature and max token settings.
5. Type a question in the input box.
6. View the chatbot response on the page.

## Current Implementation Notes

- The prompt instructs the model to behave as a helpful assistant.
- The app uses this LangChain flow:

```text
Prompt Template -> Groq Chat Model -> String Output Parser
```

- The sidebar exposes `temperature` and `max_tokens`, but the current code does not pass those values into `ChatGroq`. To make those controls affect responses, update `generate_response()` in `app.py` to pass them to the model constructor.
- A Groq API key is required before the app can generate a response.
- LangSmith tracing requires a valid `LANGCHAIN_API_KEY` in `.env`.

## Troubleshooting

### Streamlit command not found

Make sure dependencies are installed and the virtual environment is activated:

```powershell
.\venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

### Missing Groq API key warning

Enter your Groq API key in the sidebar before submitting a question.

### LangChain or dotenv import error

Reinstall dependencies:

```powershell
pip install -r requirements.txt
pip install python-dotenv
```

### LangSmith tracing error

Check that `.env` contains a valid `LANGCHAIN_API_KEY`. If you do not want tracing, remove or disable the LangSmith environment configuration in `app.py`.

## Possible Improvements

- Pass `temperature` and `max_tokens` into `ChatGroq`.
- Move the Groq API key to `.env` or Streamlit secrets for local development.
- Add chat history instead of only one question at a time.
- Improve validation for missing environment variables.
- Pin dependency versions in `requirements.txt` for reproducible installs.
