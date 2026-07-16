---
title: claude code 問題
date: 2026-07-14
keywords: claude code
---
## claude-code-router
列出所有套件
npm list -g --depth=0

npm install -g @anthropic-ai/claude-code

npm install -g @musistudio/claude-code-router@2.0.0

claude-code-router version:2.0.0

/Users/cici/.claude-code-router/bin/

claude-code-router 安裝路徑:

~/.claude-code-router


此錯誤發生的原因是，Claude Code 被配置為使用一個憑據助手（例如 ccr-claude-code-api-key-default-claude-code ），而該助手在您的路由器配置中已不存在。為了解決此問題，您需要從您的設置中刪除已失效的 apiKeyHelper 路徑，並直接使用 /login 命令設置您的 API 密鑰。

打開您的 Claude 設置：編輯您的 .claude/settings.json 文件（通常位於 /Users/cici/.claude/settings.json ）。

移除輔助功能：刪除或註釋掉 JSON 文件中的 " apiKeyHelper " 鍵值對，以阻止其調用缺失的腳本。

設置環境變量（可選，但建議）：在您的終端中，將您的有效 Anthropic API 密鑰導出。export ANTHROPIC_API_KEY="sk-ant-..."

重新驗證：重啟 Claude Code，並使用 ` /login ` 命令，直接通過您的網頁瀏覽器進行驗證，而不是依賴本地路由器。

{
  "apiKeyHelper": "/Users/cici/.claude-code-router/bin/ccr-claude-code-api-key-default-claude-code",
  "env": {
    "CLAUDE_CODE_ENABLE_GATEWAY_MODEL_DISCOVERY": "1",
    "ANTHROPIC_BASE_URL": "http://127.0.0.1:3456",
    "ANTHROPIC_API_BASE_URL": "http://127.0.0.1:3456",
    "CLAUDE_AGENT_API_BASE_URL": "http://127.0.0.1:3456",
    "ANTHROPIC_MODEL": "Google Gemini/gemini-2.5-flash",
    "CCR_CLAUDE_CODE_MODEL": "Google Gemini/gemini-2.5-flash",
    "CODEXL_CLAUDE_CODE_MODEL": "Google Gemini/gemini-2.5-flash",
    "ANTHROPIC_SMALL_FAST_MODEL": "Google Gemini/gemini-2.5-flash"
  },
  "theme": "dark"
}

cat > ~/.claude-code-router/config.json << 'EOF'
{
  "providers": [
    {
      "name": "ollama",
      "api_base_url": "http://127.0.0.1:11434/v1",
      "api_key": "ollama",
      "models": [
        "qwen:1.8b"
      ]
    }
  ]
}
EOF

## ollama設定
export ANTHROPIC_AUTH_TOKEN="ollama"
export ANTHROPIC_API_KEY=""
export ANTHROPIC_BASE_URL="http://127.0.0.1:11434"

{
  "env": {
    "ANTHROPIC_BASE_URL": "http://127.0.0.1:11434",
    "ANTHROPIC_AUTH_TOKEN": "ollama",
    "ANTHROPIC_MODEL": "qwen:1.8b"
  },
  "theme": "dark"
}





    {
      "name": "ollama",
      "api_base_url": "http://localhost:11434/v1/chat/completions",
      "api_key": "ollama",
      "models": ["qwen2.5-coder:latest"]
    }

ollama launch claude --model qwen:1.8b --yes -- -p "你是那一個大模型?"

cat > ~/.claude-code-router/config.json << 'EOF'
{
  "LOG": true,
  "LOG_LEVEL": "info",
  "HOST": "127.0.0.1",
  "PORT": 3456,
  "API_TIMEOUT_MS": 600000,
  "Providers": [
    {
      "name": "ollama",
      "api_base_url": "http://localhost:11434/v1/chat/completions",
      "api_key": "ollama",
      "models": ["qwen:1.8b"]
    },
    {
      "name": "gemini",
      "api_base_url": "https://generativelanguage.googleapis.com/v1beta/models/",
      "api_key": "sk-xxx",
      "models": ["gemini-2.5-flash", "gemini-2.5-pro"],
      "transformer": {
        "use": ["gemini"]
      }
    }
  ],
  "Router": {
    "default": "ollama",
    "background": "ollama",
    "think": "ollama",
    "longContext": "ollama",
    "longContextThreshold": 60000,
    "webSearch": "gemini,gemini-2.5-flash"
  }
}
EOF


export ANTHROPIC_AUTH_TOKEN="ollama"
export ANTHROPIC_API_KEY="sk-local"
export ANTHROPIC_BASE_URL="http://127.0.0.1:11434"
export CLAUDE_CODE_DISABLE_EXPERIMENTAL_BETAS=1

我需要產生一個app，名字是兒童自然發音，我是第一次使用claude code
你幫我設計產品文件，並記錄到claude.md中
我的一些要求如下:
1.這個App是Flutter寫的，可以在IOS手機、Android手機、網頁上執行
2.使用者年齡為5至12歲
3.剛開始先做簡單訓練兒童切音節的功能