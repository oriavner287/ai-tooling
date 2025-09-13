# Aider Coding Assistant Setup Guide (Updated)

## 1. Install Python via `pyenv` + venv

```bash
python3 --version        # check current default python
lsb_release -a           # check Linux Mint / Ubuntu base version

sudo apt update
sudo apt install -y make build-essential libssl-dev zlib1g-dev \
libbz2-dev libreadline-dev libsqlite3-dev curl git libncursesw5-dev \
xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev

curl https://pyenv.run | bash

# add pyenv root environment variable
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.zshrc

# add pyenv bin directory to PATH
echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.zshrc

# initialize pyenv when shell starts
echo 'eval "$(pyenv init - bash)"' >> ~/.zshrc
echo 'eval "$(pyenv virtualenv-init -)"' >> ~/.zshrc

source ~/.zshrc

pyenv install 3.13.7       # or pyenv install 3.13
pyenv global 3.13.7        # make this version default for user
python --version
```

## 2. Install and Configure Aider

```bash
python -m venv .venv && source .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install aider-install
aider-install

# Recommended SDK
pip install google-genai

# Optional older SDK
pip install google-generativeai

# Set API keys
export GEMINI_API_KEY="ya29.your_gemini_key_here"
export OPENAI_API_KEY="openai_key_here"

# Run Aider
cd /path/to/your/project
aider --model gemini        # Gemini 2.5 Pro
# OR
aider --model gemini-exp    # Free experimental

aider --list-models gemini/

# Example workflow
cd my-repo
aider --model gemini
> /add src/app.py
> /code "Refactor function process() to handle empty input and add unit tests"

# CheatSheet
python -m venv .venv && source .venv/bin/activate
pip install --upgrade pip
pip install aider-install
aider-install
pip install google-genai
export GEMINI_API_KEY="ya29.your_gemini_key_here"
cd /path/to/repo
aider --model gemini
# Experiment
aider --model gemini-exp
# Add files at startup
aider src/*.py --model gemini
# Useful commands: /add, /code, /ask, /architect, /commit, /diff
```

## 3. MCP Servers Setup

### Install mcpm-aider globally

```bash
npm i -g @poai/mcpm-aider
```

### Configure Claude (for MCP server addition)

```bash
cd ~/.config   # On Mac: /User/your_name
mkdir claude
cd claude
touch claude_desktop_config.json
nano claude_desktop_config.json
```

Add content:

```json
{
	"mcpServers": {}
}
```

### After installation, check tool usage

```bash
mcpm-aider toolprompt
```

### 3.1 Context7

```bash
npx -y @upstash/context7-mcp
mcpm-aider add context7
-> "npx"
-> -y @upstash/context7-mcp
```

### 3.2 Sequential Thinking

```bash
mcpm-aider add @modelcontextprotocol/server-sequential-thinking
```

### 3.3 Serena

```bash
git clone https://github.com/oraios/serena.git
cd serena
uvx --from git+https://github.com/oraios/serena serena start-mcp-server
mcpm-aider add serena --command "uvx" --args ["--from","git+https://github.com/oraios/serena","serena","start-mcp-server"]
```
