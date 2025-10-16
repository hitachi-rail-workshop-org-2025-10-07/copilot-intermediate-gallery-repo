# GitHub Copilot — Engineering Practices Demo (Codespaces Edition)

Welcome to the GitHub Copilot engineering practices demo (optimized for GitHub Codespaces).  
This walkthrough uses only features available to individual / trial Copilot accounts—no enterprise-only editing or system prompt customization.

## What You’ll Learn

By the end you will:

- [ ] Understand how Copilot gathers and uses context inside a Codespaces environment
- [ ] Inspect Copilot’s decision process using the Debug view (system prompt visibility, context sources, response metadata)
- [ ] Apply prompt-engineering patterns (Few-Shot, Chain-of-Thought, RAG-style retrieval, Step-Back)
- [ ] Export and reuse effective chat prompts/conversations
- [ ] Use (and distinguish) personal instructions vs. organization system prompts
- [ ] Apply simple privacy controls (exclude patterns / mindful context)

**Estimated Time:** 10–15 minutes

---

## 1. Prerequisites (Codespaces Setup)

You’ll run everything inside a GitHub Codespace.

### Repository
Create (or fork) a minimal demo repo (structure below). Open it in a Codespace.

### Extensions
Ensure these extensions are active:
- GitHub Copilot
- GitHub Copilot Chat

> [!NOTE]
> I installed the GitHub Copilot extension in Codespace to enable the Chat Debug View.

If Copilot isn’t authorized:
1. Sign in via GitHub account (top-left account badge).
2. Enable Copilot via [GitHub Copilot settings](https://github.com/copilot).

### Recommended Environment Tweaks
- Editor font size: 14–16
- Remove distractions (close extra panels)
- High-contrast or light theme for clarity

---

## 2. Sample Demo Repository

Baseline files:

```md
# README.md
# Copilot Engineering Demo Repo
This repo demonstrates GitHub Copilot engineering practices: context, prompt engineering, and the chat debug view.

Files:
- src/main.py
- src/utils.py
```

```python
# src/main.py
from utils import parse_items

def process_data(items):
    # TODO: Improve performance & validation
    parsed = [parse_items(i) for i in items]
    result = []
    for p in parsed:
        if p not in result:
            result.append(p)
    return result
```

```python
# src/utils.py
def parse_items(item):
    # naive parsing
    return item.strip().lower()
```

```gitignore
# .gitignore
.vscode/
.env
```

Init (Codespaces terminal):
```bash
git add .
git commit -m "baseline: minimal README and process_data()"
```

---

## 3. How Copilot Uses Context in Codespaces

Copilot gathers context from:
- Active editor file(s) and your current selections
- Open tabs
- Recent edits
- Repository content (README is especially important for retrieval)
- Chat history

### Context Shift Demo (non-RAG)

To observe how Copilot’s responses change based on open files, follow these steps:

1. **Start with only `README.md` open.**  
   Ask in Copilot Chat:  
   `Summarize this repository.`  
   _Copilot will refer mainly to the README content for its summary._

2. **Next, open `src/utils.py` alongside `README.md`.**  
   Ask again:  
   `Summarize this repository.`  
   _Copilot now incorporates details from both the README and the helper code._

3. **Finally, open `src/main.py` as well.**  
   Ask once more:  
   `Summarize this repository.`  
   _Copilot’s summary should now reflect the main logic in addition to the README and utilities._
   
> [!TIP]
> Always use the “Send” button (not “Send with dispatch”) in Copilot Chat. This ensures only your currently open files are included in the context, enforcing the intended scope.

4. **Observe how each answer becomes more detailed as you add files to the context.**  
   Copilot’s output should narrow in focus and incorporate information from each file as it is opened.

---

## 4. Debug View & System / Personal Instructions

**Purpose:** Inspect underlying context and system guidance.
> Adding developer or using the ... dots next to chat setting in chat window.
### Access Debug View
1. Command Palette: `Ctrl+Shift+P` / `Cmd+Shift+P`
2. Run: `Copilot Chat Debug: Focus on Copilot Chat Debug View`
3. Ask:
```text
Explain this function step by step:

def process_data(items):
    parsed = [parse_items(i) for i in items]
    result = []
    for p in parsed:
        if p not in result:
            result.append(p)
    return result
```

### Inspect
- System Prompt (view-only baseline)
- User Prompt
- Context Sources (open files, README)
- Response Details (tokens / metadata may vary)

### Personal Instructions
If enabled:
- Visit [https://github.com/copilot](https://github.com/copilot)
- Add: “Prefer Python examples”, “Keep answers under 150 words”
- Note: Org system prompt editing requires Enterprise.

---

## 5. Prompt Engineering Patterns

### Few-Shot
```text
Example 1:
Input: def add(a, b): return a+b
Output explanation: simple function adding two numbers

Example 2:
Input: def multiply(a, b): return a*b
Output explanation: simple function multiplying two numbers

Now: Please write a concise docstring for the function below:

def process_data(items):
    parsed = [parse_items(i) for i in items]
    result = []
    for p in parsed:
        if p not in result:
            result.append(p)
    return result
```

### Chain-of-Thought
```text
Explain your reasoning step by step before providing the final optimized version of this function:

def process_data(items):
    parsed = [parse_items(i) for i in items]
    result = []
    for p in parsed:
        if p not in result:
            result.append(p)
    return result
```

### RAG-Style Retrieval (Focused “Before / After” Step)

This step demonstrates how upgrading README content changes Copilot’s ability to apply specific validation logic.

1. BEFORE (Baseline README):
   Prompt:
   ```
   Use the validation rules from README.md to update process_data() in main.py.
   ```
   Expected: Copilot has no explicit table/rules → may produce generic or incomplete validation or ask for clarification.

2. UPGRADE README:
   Replace `README.md` with:

   ```md
   # Copilot Engineering Demo Repo

   This repository is used to demonstrate **GitHub Copilot engineering practices** — including context understanding, prompt engineering, and debugging Copilot’s decision process using the Chat Debug view.

   ## 📂 Repository Structure

   src/
   ├── main.py      # Contains the process_data() function
   └── utils.py     # Contains helper parsing utilities
   
   ## 🧩 Function Overview
   ### `process_data(items: list[str]) -> list[str]`
   Processes and normalizes a list of text items (strip, lowercase, dedupe while preserving order).

   ## ✅ Input Validation Rules
   | Rule | Description | Example |
   |------|-------------|---------|
   | 1️⃣ Type validation | `items` must be a `list` of strings | Raise `TypeError` if not a list or any element not a string |
   | 2️⃣ Empty list check | Reject empty input lists | Raise `ValueError("items cannot be empty")` |
   | 3️⃣ Null / None check | Ignore `None` entries | `["apple", None]` → `["apple"]` |
   | 4️⃣ Length limit | Max 1000 items | Raise `ValueError("too many items")` |
   | 5️⃣ Character validation | Allow only alphanumeric, spaces, underscores, hyphens | `"hello_world"` ✅ / `"hello!"` ❌ |
   | 6️⃣ Duplicate handling | Preserve first occurrence only (case-insensitive) | `["Apple", "apple"]` → `["apple"]` |
   | 7️⃣ Output guarantee | Return list of validated lowercase strings | `list[str]` |

   ## 🧠 Demo Instructions
   Use this README in retrieval-based prompts to enforce validation.
   
   ```




   Commit:
   ```bash
   git add README.md
   git commit -m "add validation rules to README for RAG demo"
   ```

3. AFTER (Enhanced README open in an editor tab):
   Re-run the SAME prompt:
   ```
   Use the validation rules from README.md to update process_data() in main.py.
   ```
   Expected: Copilot applies all listed rules (type check, empty list, length limit, None filtering, character whitelist, case-insensitive dedupe, lowercase output).  
   Tip: Open Debug View to confirm README is a context source.

4. ADVANCED RAG PROMPT (Explicit referencing of rule numbers):
   ```text
   Using the validation rules (1–7) in README.md, rewrite process_data(). Cite each rule you implement and explain any trade-offs. Then provide tests for at least 3 edge cases.
   ```

5. DIFFERENCE TALKING POINTS:
   - “Before” lacked structured retrieval → ambiguous response.
   - “After” shows direct mapping from table to code.
   - Debug View reveals README presence in context sources.

### Step-Back Prompting
```text
First list high-level strategies to speed up this function. Then choose the best one and apply it with code:

def process_data(items):
    parsed = [parse_items(i) for i in items]
    result = []
    for p in parsed:
        if p not in result:
            result.append(p)
    return result
```

---

## 6. Export & Reuse Chat Conversations

### Export
Command Palette → `Chat: Export Chat...`  
Save: `copilot-prompts/process_data_validation.json`

### Import
Command Palette → `Chat: Import Chat...` → select file

Folder README:
```md
# copilot-prompts/README.md
Reusable Copilot chat sessions.
Name files by topic: validation.json, performance.json.
```

---


## 7. Wrap-Up

You have:
- Demonstrated context sensitivity (open files & README changes)
- Shown RAG difference (before vs after README upgrade)
- Inspected system prompt & context sources via Debug view
- Applied Few-Shot, Chain-of-Thought, RAG-style, Step-Back prompts
- Exported / imported conversations for reuse
- Used personal instructions (if available)
- Practiced basic safety & exclusion patterns
