# 🚀 Technical Blog Writing Assistant

An advanced, multi-agent AI system designed to transform a simple topic into a comprehensive, research-backed technical blog post. Built with **LangGraph**, this assistant automates the entire content creation lifecycle—from real-time web research to parallel section writing and automated diagram generation.



## ✨ Key Features

* **🧠 Intelligent Routing:** Automatically detects if a topic requires real-time web research (Open-Book) or can be handled via internal LLM knowledge (Closed-Book).
* **🔍 Autonomous Research:** Integrated with **Tavily Search** to gather high-signal data, deduplicate sources, and filter for recency (7-day windows for news, 45-day for hybrid topics).
* **⚡ Parallel Writing Engine:** Utilizing LangGraph’s `Send` API to "fan-out" tasks. Multiple **Worker Agents** write different sections of the blog simultaneously, significantly reducing generation time.
* **📊 Integrated Multi-Modal Output:**
    * **Text:** High-quality Markdown following specific editorial guidelines.
    * **Code:** Automated generation of technical code snippets where required.
    * **Diagrams:** A specialized "Visual Editor" node identifies complex concepts and generates technical diagrams using **Gemini 2.5 Flash-Image**.
* **🛠️ Production-Ready Reducer:** Stitches parallel outputs into a cohesive document, sanitizes filenames, and handles graceful fallbacks if image generation fails.
* **💻 Streamlit Dashboard:** A clean UI to track graph progress, preview Markdown in real-time, and download the final "Blog Bundle" (Markdown + Images).

---

## 🏗️ System Architecture

The project follows an **Orchestrator-Worker** design pattern managed by a state machine:

1.  **Router Node:** Analyzes the topic and sets the research mode.
2.  **Research Node:** Gathers and deduplicates "Evidence Items" from the web.
3.  **Orchestrator Node:** Creates a detailed `Plan` with 5–9 specific tasks.
4.  **Worker Nodes (Parallel):** Multiple agents draft sections including code and citations.
5.  **Reducer Subgraph:**
    * **Merge Content:** Orders and joins sections.
    * **Decide Images:** Places `[[IMAGE_PLACEHOLDERS]]` in the text.
    * **Generate Images:** Calls Gemini to create `.png` assets and updates the Markdown links.

---

## 🛠️ Tech Stack

* **Framework:** [LangGraph](https://github.com/langchain-ai/langgraph) & [LangChain](https://github.com/langchain-ai/langchain)
* **LLMs:** GPT-4o-mini (Orchestration/Writing), Gemini 2.5 Flash-Image (Visuals)
* **Search Engine:** [Tavily AI](https://tavily.com/)
* **Frontend:** Streamlit
* **Data Validation:** Pydantic (Strict Schema Enforcement)

---

## 🚀 Getting Started

### 1. Prerequisites
Ensure you have API keys for:
* OpenAI (GPT models)
* Tavily (Search)
* Google AI Studio (Gemini Image generation)

### 2. Installation
```bash
git clone https://github.com/your-username/technical-blog-writing-assistant.git
cd technical-blog-writing-assistant
pip install -r requirements.txt
```

### 3. Environment Setup
Create a `.env` file in the root directory:
```env
OPENAI_API_KEY=your_openai_key
TAVILY_API_KEY=your_tavily_key
GOOGLE_API_KEY=your_google_key
```

### 4. Run the Application
```bash
streamlit run bwa_frontend.py
```

---

## 📂 Project Structure

* `bwa_backend.py`: The core logic containing the LangGraph definition, nodes, and state schemas.
* `bwa_frontend.py`: The Streamlit interface for user interaction and output visualization.
* `/images`: Automatically created directory to store generated diagrams and charts.

---

## 📝 Example Output

The system generates a **"Blog Bundle"** consisting of:
1.  **`sanitized_title.md`**: The full blog post with headers, citations, and code blocks.
2.  **`images/`**: A folder containing generated PNG files referenced in the Markdown.

> **Note:** If an image fails to generate due to safety filters or API limits, the system inserts a detailed text description block to keep the blog useful and readable.

---
