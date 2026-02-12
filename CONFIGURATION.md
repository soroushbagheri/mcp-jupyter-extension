# Configuration Guide

## Environment Variables

Create a `.env` file in the project root:

```env
# Copy from .env.example and fill in your keys
OPENAI_API_KEY=sk-...
ANTHROPIC_API_KEY=sk-ant-...
GOOGLE_API_KEY=AIza...
HF_API_KEY=hf_...
COHERE_API_KEY=...
```

## API Key Sources

### OpenAI

1. Go to [platform.openai.com](https://platform.openai.com)
2. Sign in or create account
3. Navigate to "API keys"
4. Click "Create new secret key"
5. Copy and save securely

### Anthropic

1. Visit [console.anthropic.com](https://console.anthropic.com)
2. Sign in or create account
3. Go to "API keys"
4. Click "Create Key"
5. Copy and save securely

### Google Gemini

1. Visit [ai.google.dev](https://ai.google.dev)
2. Click "Get API Key"
3. Create new API key for project
4. Copy and save securely

### Hugging Face

1. Create account at [huggingface.co](https://huggingface.co)
2. Go to [Settings → Access Tokens](https://huggingface.co/settings/tokens)
3. Click "New token"
4. Select "Read" access
5. Copy and save securely

### Cohere

1. Sign up at [dashboard.cohere.ai](https://dashboard.cohere.ai)
2. Navigate to "API keys"
3. Click "Create" or use existing
4. Copy and save securely

## Model Selection

### OpenAI Models

- `gpt-4` - Most capable, slower, most expensive
- `gpt-4-turbo-preview` - Better speed/cost balance
- `gpt-3.5-turbo` - Fast, cheap

### Anthropic Models

- `claude-3-opus-20240229` - Most capable
- `claude-3-sonnet-20240229` - Balanced (recommended)
- `claude-3-haiku-20240307` - Fast and affordable

### Google Models

- `gemini-pro` - General purpose
- `gemini-pro-vision` - With image understanding

### Hugging Face Models

- `mistralai/Mistral-7B-Instruct-v0.1` - Open source, fast
- `meta-llama/Llama-2-7b-chat-hf` - Smaller, efficient
- Any model available on Hugging Face Hub

### Cohere Models

- `command` - Full-featured
- `command-light` - Faster, lower cost
- `command-nightly` - Latest features (preview)

## Configuration Examples

### Minimal Setup (OpenAI only)

```env
OPENAI_API_KEY=sk-...
```

### Development Setup (Free tiers)

```env
GOOGLE_API_KEY=AIza-...  # Free tier available
HF_API_KEY=hf_...         # Free tier available
COHERE_API_KEY=...        # Free tier available
```

### Production Setup (All providers)

```env
OPENAI_API_KEY=sk-...
ANTHROPIC_API_KEY=sk-ant-...
GOOGLE_API_KEY=AIza-...
HF_API_KEY=hf_...
COHERE_API_KEY=...

# Rate limiting
RATH_LIMIT_PER_MINUTE=60
RETRY_ATTEMPTS=3
RESPONSE_TIMEOUT=30
```

## Provider Configuration

In Python code:

```python
from mcp_jupyter import MCPClient

# Use default model
client = MCPClient(provider="openai")

# Specify custom model
client = MCPClient(provider="openai", model="gpt-3.5-turbo")
```

## Colab-Specific Configuration

In Colab, use `getpass` for secure key input:

```python
from getpass import getpass
import os

openai_key = getpass('OpenAI API Key: ')
os.environ['OPENAI_API_KEY'] = openai_key
```

## Security Best Practices

1. **Never hardcode API keys** in notebooks or code
2. **Use environment variables** via `.env` files
3. **Use `getpass` in Colab** for secure input
4. **Rotate keys regularly** on provider dashboards
5. **Monitor usage** for unusual activity
6. **Use read-only tokens** where possible

## Rate Limiting

Each provider has different rate limits:

| Provider | Free Tier | Pro Tier |
|----------|-----------|----------|
| OpenAI | 3 req/min | 3500 req/min |
| Anthropic | 3 req/sec | Higher |
| Google | 60 req/min | Higher |
| Cohere | 100 req/min | Higher |

Configure in `.env`:

```env
RATH_LIMIT_PER_MINUTE=60
RETRY_ATTEMPTS=3
```

## Troubleshooting

### "API key not found"

Check:
- `.env` file exists in project root
- Environment variable names match exactly (case-sensitive)
- Keys are not wrapped in quotes

### "Authentication failed"

Check:
- API key is valid and active
- Key hasn't been regenerated
- Token hasn't expired

### Rate limit exceeded

- Increase `RATE_LIMIT_PER_MINUTE`
- Add delays between requests
- Upgrade to paid tier

## Cost Estimation

Approximate costs per 1M tokens:

| Model | Input Cost | Output Cost |
|-------|-----------|-------------|
| GPT-4 | $30 | $60 |
| GPT-3.5-Turbo | $0.50 | $1.50 |
| Claude 3 Opus | $15 | $75 |
| Gemini Pro | Free (limited) | Free (limited) |
