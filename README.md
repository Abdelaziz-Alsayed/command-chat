# Windows AI Agent

An AI-powered Windows assistant that enables users to interact with their operating system using natural language. 

The agent executes Windows CMD commands, retrieves system information, manages files and directories, performs web searches, and retains conversational context throughout a session.

---

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Architecture](#architecture)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Requirements](#requirements)
- [Installation](#installation)
- [Usage & Examples](#usage--examples)
- [Environment & Interoperability](#environment--interoperability)
- [Security Considerations](#security-considerations)
- [Future Improvements](#future-improvements)
- [License](#license)

---

## Overview

Windows AI Agent bridges natural language interaction with low-level operating system controls. Running within a WSL (Windows Subsystem for Linux) setup, it delegates system-level operations directly to the Windows host through `cmd.exe`, while leveraging Google Gemini via LangGraph for decision-making, tool execution, and contextual dialogue.

---

## Features

- **AI Assistant:** Powered by Google Gemini via LangChain and LangGraph.
- **Windows Command Execution:** Direct execution of Windows CMD commands via `cmd.exe`.
- **System Diagnostics:** On-demand retrieval of hardware, OS, and drive information.
- **File & Directory Management:** Create, inspect, navigate, and modify file system structures.
- **Web Search Integration:** Real-time web queries using DuckDuckGo Search.
- **Session Memory:** Retains multi-turn conversation context across interactions.
- **Web Interface:** Fast, interactive chat UI powered by Chainlit.
- **WSL-Windows Interoperability:** Runs seamlessly inside Linux (WSL 2) while orchestrating tasks on the Windows host.

---

## Architecture

```text
User
  │
  ▼
Chainlit UI
  │
  ▼
LangGraph Agent
  │
  ├── Google Gemini
  │
  ├── Windows CMD Tool
  │       │
  │       └── cmd.exe ──> Windows Host
  │
  ├── DuckDuckGo Search
  │
  └── Memory Saver / Session State
```

---

## Tech Stack

- **Runtime:** Python 3.12, Conda
- **LLM Engine:** Google Gemini
- **Agent Framework:** LangGraph, LangChain
- **User Interface:** Chainlit
- **Search Provider:** DuckDuckGo Search
- **Execution Layer:** Windows Subsystem for Linux (WSL 2), Windows Command Prompt (`cmd.exe`)

---

## Project Structure

```text
command-chat/
│
├── main.py              # Application entry point, agent workflow, and tool definitions
├── requirements.txt     # Python package dependencies
├── .env                 # Local environment variables and API keys
├── .gitignore           # Ignored files and directories
└── README.md            # Project documentation
```

---

## Requirements

- **Operating System:** Windows 10 or Windows 11 with WSL 2 enabled
- **Python:** 3.12
- **Environment Manager:** Conda (Miniconda or Anaconda)
- **API Key:** Google Gemini API Key

---

## Installation

### 1. Clone the Repository

```bash
git clone <your-repository-url>
cd command-chat
```

### 2. Set Up Conda Environment

```bash
conda create -n command-chat python=3.12 -y
conda activate command-chat
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

### 4. Configure Environment Variables

Create a `.env` file in the root directory:

```env
GEMINI_API_KEY=your_api_key_here
GEMINI_MODEL=gemini-2.5-flash
```

> **Note:** Never commit the `.env` file to version control.

---

## Usage & Examples

### Starting the Application

Launch the Chainlit interface from your terminal:

```bash
chainlit run main.py
```

Open your browser and navigate to the local server address (default: `http://localhost:8000`).

### Sample Prompts

The assistant routes queries to appropriate tools automatically:

- **System Diagnostics:**
  - *"What is my computer's system information?"*
  - *"What drives are currently available on my machine?"*

- **File System Operations:**
  - *"Create a folder named ProjectData on my D drive."*
  - *"Does D:\ProjectData exist?"*
  - *"List the files located in C:\Users\<Username>\Downloads."*

- **Web Inquiries:**
  - *"Search the web for the latest Python release."*
  - *"Look up the syntax for PowerShell symlinks."*

---

## Environment & Interoperability

The project is developed and executed within a Linux-based **WSL 2** environment while directly issuing commands to the host Windows system.

Command dispatching is achieved by bridging subshell execution to `cmd.exe`:

```python
import subprocess

result = subprocess.run(
    ["cmd.exe", "/c", command],
    capture_output=True,
    text=True
)
```

This approach allows developers to maintain a Linux workflow while retaining direct operational access to the host Windows OS.

---

## Security Considerations

Granting an autonomous agent terminal execution privileges introduces inherent security risks. 

Before deploying this software outside a local, sandboxed test setup, implement the following safeguards:

1. **Command Allowlists:** Restrict execution to pre-approved, non-destructive commands.
2. **Human-in-the-Loop Confirmation:** Require manual confirmation before running state-altering or destructive commands (e.g., `del`, `rmdir`, `format`).
3. **Path Traversal Restrictions:** Confine file operations to designated directories.
4. **Sandboxing:** Run operations within isolated containers or unprivileged accounts.
5. **Audit Logging:** Record all agent instructions, invoked subcommands, and outputs.
6. **Credential Protection:** Store secrets only in `.env` and verify `.gitignore` excludes sensitive files.

---

## Future Improvements

- Human-in-the-loop command approval workflow
- Strict command parsing and allowlist validation
- Execution history tracking and auditing
- Dedicated structured file-management tool wrappers
- Token-by-token response streaming
- Persistent cross-session user memory
- User authentication and access control
- Real-time system performance monitoring metrics
- Voice input/output integration
- PowerShell and advanced Windows automation modules

---

## License

This project is intended for educational and experimental purposes.