# Chat with SkySQL Semantic DB Agents

The examples here demonstrate how one can use SkySQL DB agents as tools within popular GenAI Frameworks  - LangChain, LlamaIndex and CrewAI. 

### Why use SkySQL DB Agents ? 

While it is possible to generate SQL given some schema description to a LLM or use APIs in frameworks, it is rather challenging to generate accurate SQL for real world DBs with high complexity. You need the correct context, allow humans to add missing semantic information(e.g. map common terms to columns), store some relevant context (Schema, categorical values, etc) in Vector stores and more. 
SkySQL DB Agents provides a No-Code UI to autonomously learn the DB context. It is also secure and reliable. 

### How do I create these DB Agents ? 
Here are the steps:

- Visit the SkySQL portal at app.skysql.com , sign up and click 'SkyAI Agents'. 
- We need a DB to work with. You could launch a SkySQL free Serverless DB. It will literally only take a second or two. Click 'Dashboard' --> Launch.
- OR, If you already have access to your DB (can only be MySQL or MariaDB), add a 'data source'. Enter the DB credentials (these are safely managed in SkySQL). 
- Click 'Agent --> Create' and type some goal/objectives for your DB agent. 
- SkySQL will discover the schema info, relationships, peek at the data and automatically select the appropriate tables and generate the context. 
- You can tweak this context (IMPORTANT, especially for complex DBs or tables) - select/deselect columns. Stick to no more than 10 tables. 
- Use the playground to test it out. Iteratively make the context for the tables and Agent better (i.e. edit the text)
- That is it. It is now accessible using the REST API  (tools to external Agents like Lanchain agents)

### Why integrate with AI frameworks

SkySQL provides "DB level" Agents. Your application may still need to work with disparate other sources, have its own orchestration across multiple agents and so on. 
Moreover, a high level agent often improves the quality of the responses. It will rewrite the user question to be more appropriate and synthesize good results. 

## Examples here

The Simple examples here provide a conversational agent that can interact with backend database agents to answer your queries, show SQL, and suggest next steps. You can use it as a web app (Streamlit) or from the command line.

---

## Setup Instructions

Follow setup instructions from the SkySQL Demo repo's [README.md](https://github.com/skysqlinc/skysql-demos/blob/main/README.md)

---

## Running the App

### Option 1: Run the CLI Version

You can try the CLI agent using any of the following frameworks:

**LlamaIndex:**
```sh
python db_chat_agent.py
```

**LangGraph:**
```sh
python db_chat_agent_langgraph.py
```

**CrewAI:**
```sh
python db_chat_agent_crewai.py
```

- Interact with the agent in your terminal.
- All versions support listing available DB agents, chatting with them, and showing SQL for transparency.

### Option 2: Run the Streamlit Web App (uses LlamaIndex)
From the `chat-with-db-agents` directory, run:
```sh
streamlit run db_chat_streamlit_app.py
```

- Open the provided local URL in your browser.
- Chat with the agent in the web UI.

### Option 3: Use the Embeddable Chat Widget

You can embed a simple chat widget in any web page. This widget allows your users to chat with the SkySQL DB Agent directly from your web UI, just like Intercom or Drift.

**How it works:**

- The chat widget is a JavaScript snippet you add to your web page.
- When a user clicks the chat icon, a chat window opens and connects to your backend API server.
- All chat messages are sent to your FastAPI backend, which handles the conversation and returns responses (including SQL, if relevant). The backend uses the LlamaIndex implementation. You can easily change to LangGraph or CrewAI. 
- The widget preserves session state for each user in their browser.

**How to use:**

1. **Launch the API server** (must be accessible to your users' browsers):
   ```sh
   python chat-with-db-agents/db_chat_api.py
   ```
      - By default, this runs on `http://localhost:8000`. For production, deploy it to a public server and ensure CORS is enabled.

2. **Serve the static files** (for local testing):
   ```sh
   cd chat-with-db-agents/static
   python3 -m http.server 9000
   ```

3. **Test locally:**
      - Open your browser to: [http://localhost:9000/test_chat_widget.html](http://localhost:9000/test_chat_widget.html)
      - The chat icon will appear in the bottom right. Click it to chat with the agent.

4. **Embed in your own app:**
      - Add the following to your HTML:
        ```html
        <script src="URL_TO/chat_widget.js"></script>
        ```
      - Make sure your backend API server is running and accessible from the user's browser (CORS enabled).
      - You can customize the widget or backend URL as needed for your deployment.

---

## Features
- Uses OpenAI LLM and backend DB agents.
- Lists available DB agents and chats with them.
- Synthesizes responses, includes SQL, and suggests follow-up questions.
- Works as both a web app and CLI.

---

## Troubleshooting
- Ensure your API keys are correct and active.
- If you see missing package errors, re-run `pip install -r requirements.txt`.
- For Streamlit issues, try upgrading: `pip install --upgrade streamlit`.

---

## Credits
- Built with [SkySQL](https://skysql.com), [Streamlit](https://streamlit.io/), [LangGraph](https://github.com/langchain-ai/langgraph), [CrewAI](https://github.com/crewAIInc/crewAI) and [LangChain](https://github.com/langchain-ai/langchain).