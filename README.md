# ShopAssist 2.0: Enhanced AI Shopping Assistant

## 🚀 Project Overview
**ShopAssist 2.0** is a production-grade AI agent designed to act as a laptop sales engineer. Unlike simple chatbots, this system utilizes the **ReAct (Reason + Act) architecture** and **OpenAI Function Calling** to accurately retrieve data, handle errors, and maintain conversation context safely.

The primary objective was to upgrade the existing heuristic-based model into a resilient agent that ensures accurate data retrieval and network stability.

## 🏗️ System Architecture
The agent operates in a continuous loop following the ReAct pattern:

1.  **Input Layer:** User text is passed through the OpenAI Moderation API to ensure safety.
2.  **Memory Layer:** Conversation history is managed using a "Sliding Window" approach, trimming older context to prevent token overflow.
3.  **Reasoning Layer (LLM):** The model analyzes the request. If specifications are clear, it generates a Function Call; otherwise, it asks clarifying questions.
4.  **Execution Layer:** The system executes the `get_laptops_from_db` tool using strict JSON parameters.
5.  **Response Layer:** The LLM summarizes the structured data into a natural language recommendation.

## ✨ Key Features

### 1. OpenAI Function Calling
Instead of unreliable manual string parsing, this project uses a strict JSON Schema. This ensures that data points like budget ("50k") are automatically converted to integers (50000) for accurate database querying.

### 2. Production-Grade Reliability
To handle potential API timeouts or network instability, the system implements the **Tenacity** library. It uses an auto-retry mechanism (`@retry`) that attempts a request up to 3 times before raising an error.

### 3. Safety Guardrails
A dual-layer moderation system checks both user inputs and AI outputs using `client.moderations.create`, ensuring the bot refuses harmful requests instantly.

### 4. Advanced Prompt Engineering
* **Intent Confirmation:** A "Gatekeeper Rule" prevents the bot from blindly calling tools. It waits for at least 2 specific user preferences (e.g., Budget + Brand) before searching.
* **Hallucination Prevention:** Strict instructions prevent the AI from inventing products if the database returns "No laptop found".

## 🛠️ Tech Stack
* **Language:** Python
* **LLM:** OpenAI GPT Models
* **Libraries:**
    * `openai` (API interaction)
    * `tenacity` (Error handling/Retries)
    * `pandas` (Data manipulation)
    * `python-dotenv` (Environment management)

## 📂 Project Structure
* `ShopAssist_Project_Nikhil_Kalra.ipynb`: Main source code containing the agent logic, tool definitions, and interactive loop.
* `laptop_data.csv`: The dataset used for the inventory database.
* `Project Report- ShopAssist 2.0.pdf`: Detailed documentation on challenges, architecture, and solutions.

## 🚀 How to Run
1.  Clone the repository.
2.  Install dependencies:
    ```bash
    pip install openai pandas tenacity python-dotenv
    ```
3.  Set up your `.env` file with your `OPENAI_API_KEY`.
4.  Run the notebook or Python script to enter the interactive chat loop.

---
*This project was developed to demonstrate the implementation of robust AI Agents using Function Calling and Context Management.*
