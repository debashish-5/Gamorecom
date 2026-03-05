<p align="center">
<img src="https://capsule-render.vercel.app/api?type=waving&height=320&color=0:0f2027,50:203a43,100:2c5364&text=GAMORECOM&fontSize=65&fontAlignY=42&desc=Generative%20AI%20Game%20Asset%20Engine&descAlignY=63"/>
</p>

<p align="center">

<img src="https://readme-typing-svg.herokuapp.com?font=Orbitron&size=30&duration=3000&color=00E0FF&center=true&vCenter=true&width=900&lines=AI+Powered+Game+Asset+Generator;LangChain+Based+AI+Pipeline;Local+LLM+Execution+with+Ollama;Creative+Content+Generation+for+Games"/>

</p>

---

# GAMORECOM

<table>
<tr>
<td>

Gamorecom is a **Generative AI platform for game asset creation** built with modern AI engineering practices.

The project demonstrates how **local large language models** can be integrated into a **web application pipeline** to generate creative game content such as:

• fantasy creatures
• magical weapons
• quest narratives
• world lore

The system combines **LangChain orchestration**, **local LLM execution**, and a **modern UI interface** to create a compact yet powerful AI application.

</td>
</tr>
</table>

---

# Technology Stack

<p align="center">

<img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white"/>

<img src="https://img.shields.io/badge/Flask-Web%20Framework-black?style=for-the-badge"/>

<img src="https://img.shields.io/badge/LangChain-AI%20Framework-blue?style=for-the-badge"/>

<img src="https://img.shields.io/badge/Ollama-Local%20LLM%20Runtime-grey?style=for-the-badge"/>

<img src="https://img.shields.io/badge/Mistral-LLM-orange?style=for-the-badge"/>

<img src="https://img.shields.io/badge/HTML5-Frontend-red?style=for-the-badge"/>

<img src="https://img.shields.io/badge/CSS3-UI%20Styling-blue?style=for-the-badge"/>

</p>

---

# System Architecture

```mermaid
flowchart LR

User --> Web_Interface

Web_Interface --> Flask_Backend

Flask_Backend --> LangChain_Engine

LangChain_Engine --> Ollama_Runtime

Ollama_Runtime --> Mistral_Model

Mistral_Model --> Generated_Output

Generated_Output --> Web_Interface
```

---

# LangChain Generation Pipeline

```mermaid
flowchart LR

Prompt --> PromptTemplate

PromptTemplate --> LLM_Model

LLM_Model --> OutputParser

OutputParser --> Final_Output
```

The pipeline demonstrates a simplified **LangChain LCEL workflow** for controlled prompt generation.

---

# Core Features

<table>
<tr>
<td width="50%">

AI Content Generation

Generate creative assets such as:

• mythical dragons
• enchanted weapons
• fantasy locations
• RPG quest lines

</td>

<td width="50%">

Local AI Execution

Runs fully on local machine using:

• Ollama runtime
• Mistral language model
• No external API costs

</td>
</tr>

<tr>
<td>

Interactive Web Interface

Modern web interface including:

• cinematic landing page
• video background
• responsive card layouts

</td>

<td>

AI Chat Interaction

Dedicated chat interface allowing:

• dynamic prompt input
• adjustable parameters
• live AI responses

</td>
</tr>

</table>

---

# Parameter Experiments

The project explores how **LLM parameters influence generated outputs**.

| Parameter        | Description                           |
| ---------------- | ------------------------------------- |
| temperature      | Controls creativity of responses      |
| seed             | Controls reproducibility of outputs   |
| prompt structure | Affects generated style and narrative |

Example experiment:

Prompt:

```
Generate a powerful dragon boss for a fantasy RPG
```

Temperature = 0.2
Output becomes more structured and predictable.

Temperature = 0.9
Output becomes more imaginative and varied.

Seed changes produce different variations of the same concept.

---

# Project Structure

```
GAMORECOM
│
├── app.py
│
├── static
│   └── css
│       └── style.css
│
├── templates
│   ├── index.html
│   └── aichat.html
│
├── uploads
│   ├── Screenshot 2026-03-05 1309.png
│   ├── Screenshot 2026-03-05 1311.png
│   ├── Screenshot 2026-03-05 1314.png
│   └── Screenshot 2026-03-05 1315.png
│
└── .vscode
```

---

# Application Workflow

<table>
<tr>
<td>

Step 1
User opens the landing page.

Step 2
User clicks **Meet Gamorecom**.

Step 3
AI chat interface opens.

Step 4
User submits a creative prompt.

Step 5
LangChain processes the prompt.

Step 6
Ollama runs the Mistral model locally.

Step 7
Generated output is returned to the interface.

</td>
</tr>
</table>

---

# API Endpoint

POST request

```
/api/generate
```

Example request

```
{
 "prompt": "generate a powerful dragon boss",
 "temperature": 0.8,
 "seed": 123
}
```

Example response

```
{
 "output": "An ancient crimson dragon named Zyphorax emerges from volcanic ruins..."
}
```

---

# Interface Preview

<p align="center">

<img src="Screenshot 2026-03-05 1309.png" width="80%"/>

<img src="Screenshot 2026-03-05 1311.png" width="80%"/>

<img src="Screenshot 2026-03-05 1314.png" width="80%"/>

<img src="Screenshot 2026-03-05 1315.png" width="80%"/>

</p>

---

# Installation

Clone repository

```
git clone https://github.com/your-username/gamorecom.git
```

Enter project directory

```
cd gamorecom
```

Install dependencies

```
pip install flask langchain
```

Install Ollama runtime

https://ollama.com

Download model

```
ollama pull mistral
```

Run application

```
python app.py
```

Open browser

```
http://localhost:5000
```

---

# Future Improvements

• 3D asset generation
• AI image generation for game textures
• Blender integration pipeline
• Asset export for Unity / Unreal Engine
• Automatic asset tagging and search

---

# Author

Debashish Parida
Computer Science Engineering

Artificial Intelligence
Data Science
Generative AI
Full Stack Development

