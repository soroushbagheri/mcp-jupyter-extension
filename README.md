# MCP-Jupyter Extension: Multi-LLM Enabled Jupyter Notebooks

![Architecture](./docs/architecture.png)

## Overview

**MCP-Jupyter Extension** is a powerful integration that enables [Model Context Protocol (MCP)](https://modelcontextprotocol.io/) support in Jupyter notebooks with seamless Google Colab compatibility and multi-LLM support. This project allows you to leverage multiple large language models directly within your Jupyter environment while maintaining a streamable HTTP transport protocol.

### Key Features

- ✅ **MCP-Enabled Notebooks**: Full support for Model Context Protocol in Jupyter/Colab
- 🔄 **Streamable HTTP Transport**: Real-time streaming responses from LLMs
- 🤖 **Multi-LLM Support**: Integrate with leading LLM providers
- 🚀 **Colab Ready**: One-click setup in Google Colab
- 📊 **Context Management**: Efficient context handling and memory management
- 🔐 **API Key Management**: Secure credential handling
- 💻 **Production Ready**: Complete error handling and logging

## Supported LLMs

### Proprietary Models
- **OpenAI**: GPT-4, GPT-4 Turbo, GPT-3.5-Turbo
- **Anthropic**: Claude 3 (Opus, Sonnet, Haiku), Claude 2.1
- **Google**: Gemini Pro, Gemini Pro Vision
- **Azure OpenAI**: GPT-4, GPT-3.5 (Enterprise)
- **Cohere**: Command, Command Light

### Open Source / Self-Hosted
- **Meta**: Llama 2, Llama 2 Chat
- **Mistral AI**: Mistral 7B, Mistral 8x7B (MoE)
- **Hugging Face**: Any HF-compatible model via Inference API
- **Ollama**: Local model serving
- **vLLM**: High-throughput LLM serving
- **Text Generation WebUI**: Local inference

## Quick Start

### Option 1: Google Colab (Recommended)

1. Open the [Colab Notebook](./notebooks/MCP_Jupyter_Extension_Colab.ipynb) directly in Google Colab:
   [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/soroushbagheri/mcp-jupyter-extension/blob/main/notebooks/MCP_Jupyter_Extension_Colab.ipynb)

2. Run the setup cell to install dependencies
3. Configure your LLM API keys
4. Start using MCP-enabled prompts

### Option 2: Local Jupyter Installation

```bash
# Clone the repository
git clone https://github.com/soroushbagheri/mcp-jupyter-extension.git
cd mcp-jupyter-extension

# Create virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Start Jupyter
jupyter notebook
```

## Architecture

The system architecture consists of five main layers:

```
┌─────────────────────────────────────────┐
│   Colab/Jupyter Notebook Interface      │
├─────────────────────────────────────────┤
│   MCP Client Layer (Context Manager)    │
├─────────────────────────────────────────┤
│   Streamable HTTP Transport Protocol    │
├─────────────────────────────────────────┤
│   LLM Router & Load Balancer            │
├─────────────────────────────────────────┤
│   LLM Providers (OpenAI, Claude, etc.)  │
└─────────────────────────────────────────┘
```

### Components

1. **Jupyter Interface**: User-facing notebook environment
2. **MCP Client Layer**: Manages context and message protocol
3. **HTTP Transport**: Handles streaming and real-time communication
4. **LLM Router**: Routes requests to appropriate LLM provider
5. **LLM Providers**: Integrated API endpoints for various models

## Configuration

Create a `.env` file in the project root:

```env
# OpenAI
OPENAI_API_KEY=sk-...

# Anthropic
ANTHROPIC_API_KEY=sk-ant-...

# Google Gemini
GOOGLE_API_KEY=AIza...

# Hugging Face
HUGGINGFACE_API_KEY=hf_...

# Cohere
COHERE_API_KEY=...
```

## Usage Examples

### Basic Query

```python
from mcp_jupyter import MCPClient

# Initialize client
client = MCPClient(provider="openai", model="gpt-4")

# Send query
response = client.query(
    prompt="Explain quantum computing in simple terms",
    temperature=0.7,
    max_tokens=500
)

print(response)
```

### Multi-LLM Comparison

```python
from mcp_jupyter import MCPClient

prompt = "What are the latest advances in AI?"

for provider, model in [
    ("openai", "gpt-4"),
    ("anthropic", "claude-3-sonnet"),
    ("google", "gemini-pro")
]:
    client = MCPClient(provider=provider, model=model)
    response = client.query(prompt)
    print(f"\n{provider.upper()} ({model}):\n{response}\n")
```

### Context Management

```python
from mcp_jupyter import MCPClient, ContextManager

# Initialize with context
ctx = ContextManager()
ctx.add_document("research_paper.txt")
ctx.add_instruction("Provide academic citations")

client = MCPClient(provider="anthropic", context=ctx)
response = client.query("Summarize the key findings")
```

## Installation & Requirements

**Python Version**: 3.8+

```bash
pip install -r requirements.txt
```

### Core Dependencies
- `jupyter>=7.0.0` - Jupyter notebook environment
- `ipywidgets>=8.0.0` - Interactive widgets
- `openai>=1.0.0` - OpenAI API
- `anthropic>=0.4.0` - Anthropic API
- `google-generativeai>=0.2.0` - Google Gemini API
- `huggingface-hub>=0.16.0` - Hugging Face models
- `python-dotenv>=1.0.0` - Environment variable management
- `aiohttp>=3.8.0` - Async HTTP client
- `pydantic>=2.0.0` - Data validation

## Documentation

- [Installation Guide](./docs/INSTALLATION.md)
- [Configuration Guide](./docs/CONFIGURATION.md)
- [API Reference](./docs/API_REFERENCE.md)
- [Examples & Tutorials](./docs/EXAMPLES.md)
- [Troubleshooting](./docs/TROUBLESHOOTING.md)

## Notebooks

- **[MCP_Jupyter_Extension_Colab.ipynb](./notebooks/MCP_Jupyter_Extension_Colab.ipynb)** - Full-featured Colab notebook with all LLM integrations
- **[Basic_Setup.ipynb](./notebooks/Basic_Setup.ipynb)** - Simple getting-started notebook
- **[Advanced_Examples.ipynb](./notebooks/Advanced_Examples.ipynb)** - Advanced usage patterns

## API Reference

### MCPClient Class

```python
class MCPClient:
    def __init__(
        self,
        provider: str,
        model: str,
        api_key: Optional[str] = None,
        context: Optional[ContextManager] = None
    )
    
    def query(
        self,
        prompt: str,
        temperature: float = 0.7,
        max_tokens: int = 1000,
        stream: bool = True
    ) -> str
    
    def stream_query(
        self,
        prompt: str,
        temperature: float = 0.7,
        max_tokens: int = 1000
    ) -> Iterator[str]
```

## Features in Detail

### Real-time Streaming
Responses are streamed in real-time using HTTP chunked transfer encoding for immediate feedback.

### Context Management
Automatically manages conversation history, documents, and system instructions.

### Error Handling
Comprehensive error handling with retry logic and graceful degradation.

### Rate Limiting
Built-in rate limiting to respect API quotas.

### Logging
Detailed logging for debugging and monitoring.

## Project Structure

```
mcp-jupyter-extension/
├── notebooks/
│   ├── MCP_Jupyter_Extension_Colab.ipynb
│   ├── Basic_Setup.ipynb
│   └── Advanced_Examples.ipynb
├── src/
│   ├── __init__.py
│   ├── client.py
│   ├── context.py
│   ├── providers/
│   │   ├── openai.py
│   │   ├── anthropic.py
│   │   ├── google.py
│   │   ├── huggingface.py
│   │   └── base.py
│   └── utils.py
├── docs/
│   ├── INSTALLATION.md
│   ├── CONFIGURATION.md
│   ├── API_REFERENCE.md
│   ├── EXAMPLES.md
│   ├── TROUBLESHOOTING.md
│   └── architecture.png
├── tests/
│   ├── test_client.py
│   ├── test_providers.py
│   └── test_context.py
├── requirements.txt
├── .env.example
├── setup.py
└── README.md
```

## Contributing

Contributions are welcome! Please:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit changes (`git commit -m 'Add amazing feature'`)
4. Push to branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Citation

If you use this project in your research, please cite:

```bibtex
@software{bagheri2026mcp_jupyter,
  author = {Bagheri, Soroush},
  title = {MCP-Jupyter Extension: Multi-LLM Enabled Jupyter Notebooks},
  year = {2026},
  url = {https://github.com/soroushbagheri/mcp-jupyter-extension}
}
```

## Support

- 📧 Email: [your-email]
- 🐛 Issues: [GitHub Issues](https://github.com/soroushbagheri/mcp-jupyter-extension/issues)
- 💬 Discussions: [GitHub Discussions](https://github.com/soroushbagheri/mcp-jupyter-extension/discussions)

## Related Resources

- [Model Context Protocol Documentation](https://modelcontextprotocol.io/)
- [Jupyter Documentation](https://jupyter.org/)
- [Google Colab Guide](https://colab.research.google.com/)
- [LLM API Documentation](https://platform.openai.com/docs/)

---

**Last Updated**: February 2026  
**Version**: 1.0.0