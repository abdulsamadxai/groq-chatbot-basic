# 🤖 Building Your First AI Chatbot with Groq

I made this because after learning what an LLM is, the next question is obvious okay but how do I actually talk to one with code? This is the simplest way to do it. No extra stuff. Just a working chatbot in under 20 lines of Python.

---

## Table of Contents

1. [What are we building?](#what-are-we-building)
2. [What you need first](#what-you-need-first)
3. [The full code](#the-full-code)
4. [Line by line - what each part does](#line-by-line---what-each-part-does)
   - [Part 1 - Imports](#part-1---imports)
   - [Part 2 - API Key setup](#part-2---api-key-setup)
   - [Part 3 - The chat loop](#part-3---the-chat-loop)
   - [Part 4 - Sending the message](#part-4---sending-the-message)
5. [How it all flows](#how-it-all-flows)
6. [Word List](#word-list)

---

## What are we building?

A simple chatbot that runs in your terminal. You type something. It replies. That is it.

Under the hood it is using **Groq** a platform that lets you call powerful AI models like LLaMA 3.3 through an API. Think of it like texting an AI you send a message, it sends one back.

```mermaid
flowchart LR
    A["🧑 You type\n'What is Python?'"]:::blue --> B["🌐 Groq API\nsends your message to LLaMA 3.3"]:::purple --> C["🤖 AI replies\n'Python is a programming language...'"]:::green --> D["💻 Printed\nin your terminal"]:::teal

    classDef blue fill:#1e3a5f,stroke:#4a8fd4,color:#a8d4ff
    classDef purple fill:#2d1b4e,stroke:#8b5cf6,color:#c4b5fd
    classDef green fill:#1a3a2a,stroke:#34d399,color:#6ee7b7
    classDef teal fill:#0f3030,stroke:#2dd4bf,color:#99f6e4
```

---

## What you need first

Before running the code you need two things installed and one account set up.

**Install these with pip:**
```bash
pip install groq python-dotenv
```

**Create a `.env` file** in the same folder as your code and put this inside it:
```
GROQ_API_KEY=your_key_here
```

Get your free API key from [console.groq.com](https://console.groq.com). It takes two minutes.

---

## The full code

```python
from groq import Groq
from dotenv import load_dotenv
import os

# Load environment variables
load_dotenv()

# Create client
client = Groq(
    api_key=os.getenv("GROQ_API_KEY")
)

print("Chatbot Started (type 'exit' to quit)\n")

while True:
    user_input = input("You: ")

    if user_input.lower() == "exit":
        print("Bot: Goodbye!")
        break

    response = client.chat.completions.create(
        model="llama-3.3-70b-versatile",
        messages=[
            {
                "role": "user",
                "content": user_input
            }
        ]
    )

    print("Bot:", response.choices[0].message.content)
```

---

## Line by line - what each part does

### Part 1 - Imports

```python
from groq import Groq
from dotenv import load_dotenv
import os
```

These three lines bring in the tools we need.

| Line | What it does |
|------|-------------|
| `from groq import Groq` | brings in the Groq library so we can talk to the AI |
| `from dotenv import load_dotenv` | lets us read our secret API key from a file |
| `import os` | lets Python read things from that file |

Think of imports like plugging in a charger before using your phone. You need them ready before anything else works.

---

### Part 2 - API Key setup

```python
load_dotenv()

client = Groq(
    api_key=os.getenv("GROQ_API_KEY")
)
```

```mermaid
flowchart LR
    A["📄 .env file\nGROQ_API_KEY=abc123"]:::blue -->|load_dotenv reads it| B["🐍 Python memory\nkey is now loaded"]:::purple -->|os.getenv gets it| C["🔑 Groq client\nnow has the key"]:::green

    classDef blue fill:#1e3a5f,stroke:#4a8fd4,color:#a8d4ff
    classDef purple fill:#2d1b4e,stroke:#8b5cf6,color:#c4b5fd
    classDef green fill:#1a3a2a,stroke:#34d399,color:#6ee7b7
```

**Why not just write the key directly in the code?**

Because if you share your code on GitHub, everyone can see your key. People steal keys and use them to send thousands of API calls on your account. Always put keys in a `.env` file and add `.env` to your `.gitignore`.

`load_dotenv()` reads the `.env` file and loads everything inside it into Python's memory.

`os.getenv("GROQ_API_KEY")` then fetches the key by name and passes it to the Groq client.

The `client` is now your connection to the Groq API. Ready to use.

---

### Part 3 - The chat loop

```python
while True:
    user_input = input("You: ")

    if user_input.lower() == "exit":
        print("Bot: Goodbye!")
        break
```

```mermaid
flowchart TD
    A["🔁 Loop starts\nwhile True"]:::blue --> B["💬 Wait for you to type\ninput 'You: '"]:::purple --> C{"Did you type 'exit'?"}:::teal
    C -->|yes| D["🛑 Print 'Goodbye'\nand stop the loop"]:::red
    C -->|no| E["✅ Continue\nsend to AI"]:::green --> A

    classDef blue fill:#1e3a5f,stroke:#4a8fd4,color:#a8d4ff
    classDef purple fill:#2d1b4e,stroke:#8b5cf6,color:#c4b5fd
    classDef teal fill:#0f3030,stroke:#2dd4bf,color:#99f6e4
    classDef red fill:#3a0f0f,stroke:#ef4444,color:#fca5a5
    classDef green fill:#1a3a2a,stroke:#34d399,color:#6ee7b7
```

`while True` means - keep running forever until we say stop.

`input("You: ")` shows the text "You: " on screen and waits for you to type something. Whatever you type gets saved in `user_input`.

`user_input.lower() == "exit"` - the `.lower()` part converts what you typed to lowercase first. So whether you type "EXIT" or "Exit" or "exit" it will all match. When it matches - `break` stops the loop.

---

### Part 4 - Sending the message

```python
response = client.chat.completions.create(
    model="llama-3.3-70b-versatile",
    messages=[
        {
            "role": "user",
            "content": user_input
        }
    ]
)

print("Bot:", response.choices[0].message.content)
```

This is the main part. This is where you actually talk to the AI.

```mermaid
flowchart TD
    A["📤 client.chat.completions.create\nsend message to Groq API"]:::blue
    A --> B["🧠 model: llama-3.3-70b-versatile\nwhich AI model to use"]:::purple
    A --> C["💬 messages list\nyour chat history"]:::teal
    C --> D["role: user\ncontent: what you typed"]:::gray
    B & D --> E["⏳ Groq processes it\nLLaMA thinks and replies"]:::purple
    E --> F["📥 response object\ncomes back"]:::blue
    F --> G["response.choices[0].message.content\nthe actual reply text"]:::green
    G --> H["🖨️ print to screen"]:::green

    classDef blue fill:#1e3a5f,stroke:#4a8fd4,color:#a8d4ff
    classDef purple fill:#2d1b4e,stroke:#8b5cf6,color:#c4b5fd
    classDef teal fill:#0f3030,stroke:#2dd4bf,color:#99f6e4
    classDef gray fill:#252535,stroke:#4a4a6a,color:#c0c0d8
    classDef green fill:#1a3a2a,stroke:#34d399,color:#6ee7b7
```

**Breaking down `response.choices[0].message.content`**

This looks confusing but it is just going deeper into an object step by step.

| Part | What it means |
|------|--------------|
| `response` | the full reply from Groq |
| `.choices` | a list of possible replies (usually just 1) |
| `[0]` | take the first one |
| `.message` | the message inside that choice |
| `.content` | the actual text of the reply |

Think of it like opening a package. `response` is the box. `.choices[0]` is the item inside. `.message.content` is the thing written on the item.

---

## How it all flows

```mermaid
flowchart TD
    START["▶️ Code starts running"]:::gray
    START --> LOAD["📄 .env file loaded\nAPI key ready"]:::blue
    LOAD --> CLIENT["🔑 Groq client created\nconnection ready"]:::blue
    CLIENT --> PRINT["🖨️ 'Chatbot Started' printed"]:::teal
    PRINT --> LOOP["🔁 Loop begins\nwaiting for input"]:::purple
    LOOP --> INPUT["💬 You type something"]:::gray
    INPUT --> CHECK{"exit?"}:::teal
    CHECK -->|yes| BYE["👋 'Goodbye' printed\nchatbot stops"]:::red
    CHECK -->|no| API["🌐 Message sent to Groq API\nwith your text and model name"]:::purple
    API --> WAIT["⏳ AI thinks\n..."]:::gray
    WAIT --> REPLY["📥 Reply comes back\nas a response object"]:::blue
    REPLY --> SHOW["🖨️ response.choices[0].message.content\nprinted on screen"]:::green
    SHOW --> LOOP

    classDef gray fill:#252535,stroke:#4a4a6a,color:#c0c0d8
    classDef blue fill:#1e3a5f,stroke:#4a8fd4,color:#a8d4ff
    classDef purple fill:#2d1b4e,stroke:#8b5cf6,color:#c4b5fd
    classDef teal fill:#0f3030,stroke:#2dd4bf,color:#99f6e4
    classDef green fill:#1a3a2a,stroke:#34d399,color:#6ee7b7
    classDef red fill:#3a0f0f,stroke:#ef4444,color:#fca5a5
```

---

## Word List

| Word | Simple meaning |
|------|--------------|
| API | a way for two programs to talk to each other |
| Groq | a platform that runs AI models really fast |
| LLaMA 3.3 70B | a powerful open-source AI model made by Meta |
| `.env` file | a hidden file where you store secret keys |
| `load_dotenv()` | reads your `.env` file and loads the keys |
| `os.getenv()` | fetches a specific key from memory by name |
| `client` | your connection to the Groq API |
| `while True` | a loop that runs forever until you say break |
| `break` | stops the loop |
| `input()` | pauses the code and waits for you to type |
| `response` | the full reply object that comes back from Groq |
| `.choices[0]` | the first reply option in the response |
| `.message.content` | the actual text of the AI's reply |
| `model` | which AI brain you are using to answer |

---

## What is missing from this chatbot?

This chatbot does not remember anything. Every message you send starts fresh. The AI does not know what you said two messages ago.

```mermaid
flowchart LR
    A["You: What is Python?"]:::blue --> B["Bot: Python is a language..."]:::green
    C["You: Give me an example"]:::blue --> D["Bot: Example of what? 🤔\nit forgot the first message"]:::red

    classDef blue fill:#1e3a5f,stroke:#4a8fd4,color:#a8d4ff
    classDef green fill:#1a3a2a,stroke:#34d399,color:#6ee7b7
    classDef red fill:#3a0f0f,stroke:#ef4444,color:#fca5a5
```

This is called having no **memory**. The next guide will show how to fix this by sending the full chat history with every message.

---

## What's Next?

```mermaid
flowchart TD
    A["✅ You finished this guide\ngreat job!"]:::done
    A --> B["🧠 Next - Add Memory to your Chatbot\nlearn about chat history and multi-turn conversations"]:::blue

    classDef done fill:#1a3a2a,stroke:#34d399,color:#6ee7b7
    classDef blue fill:#1e3a5f,stroke:#4a8fd4,color:#a8d4ff
```

---

*Made by Abdul Samad*
