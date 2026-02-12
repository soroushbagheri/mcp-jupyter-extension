# Installation Guide

## Prerequisites

- Python 3.8 or higher
- pip package manager
- Virtual environment (recommended)

## Step 1: Clone the Repository

```bash
git clone https://github.com/soroushbagheri/mcp-jupyter-extension.git
cd mcp-jupyter-extension
```

## Step 2: Create Virtual Environment

```bash
# On macOS/Linux
python3 -m venv venv
source venv/bin/activate

# On Windows
python -m venv venv
venv\\Scripts\\activate
```

## Step 3: Install Dependencies

```bash
pip install --upgrade pip
pip install -r requirements.txt
```

## Step 4: Configure API Keys

1. Copy the example environment file:

```bash
cp .env.example .env
```

2. Edit `.env` and add your API keys:

```bash
# On macOS/Linux
nano .env

# On Windows
type .env
```

## Step 5: Start Jupyter

```bash
jupyter notebook
```

Then navigate to `notebooks/MCP_Jupyter_Extension_Colab.ipynb`

## Installation Options

### Option A: Google Colab (No Installation Required)

1. Open [this Colab notebook](https://colab.research.google.com/github/soroushbagheri/mcp-jupyter-extension/blob/main/notebooks/MCP_Jupyter_Extension_Colab.ipynb)
2. Run the first cell to install dependencies
3. Add your API keys in the configuration cells

### Option B: JupyterLab

```bash
pip install jupyterlab
jupyter-lab
```

### Option C: VSCode with Jupyter Extension

1. Install VSCode
2. Install the "Jupyter" extension
3. Open any `.ipynb` file
4. Select the Python kernel

## Troubleshooting

### Import Error: No module named 'openai'

```bash
pip install --upgrade openai
```

### API Key Not Found

- Ensure `.env` file exists in project root
- Check that environment variables are properly formatted
- Restart your Jupyter kernel after adding keys

### Connection Timeout

- Check your internet connection
- Verify API keys are valid
- Check if the API service is operational

### Kernel Issues in Colab

- Try: Runtime → Restart runtime
- Reinstall packages: `!pip install -q openai anthropic google-generativeai`

## Verify Installation

Run this in a notebook cell:

```python
import openai
import anthropic
import google.generativeai
import huggingface_hub
import cohere

print("✅ All packages installed successfully!")
```

## Next Steps

- Read the [Configuration Guide](./CONFIGURATION.md)
- Check [Examples](./EXAMPLES.md)
- Review [API Reference](./API_REFERENCE.md)
