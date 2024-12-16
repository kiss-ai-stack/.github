<div style="text-align: left; margin-bottom: 20px;">
  <img src="https://kiss-ai-stack.github.io/kissaistack.svg" alt="KISS AI Stack Banner" style="max-width: auto; height: 260px">
</div>

# **KISS AI Stack**  

Welcome to **KISS AI Stack** - a simple way to build AI assisted solutions with an AI agent. Inspired by the “Keep It Simple, Stupid” (KISS) principle, this stack focuses on delivering powerful AI agents avoiding boilerplate code.  

---

## **Why KISS AI Stack?**  
- **For Chatbots:** Build interactive agents fast.  
- **For Query Systems:** Handle questions from users effortlessly.  
- **For Secure AI:** Create private AI systems with Retrieval-Augmented Generation (RAG). 

---

## **Key Features**  
- **Autonomous Decision-Making:** The AI agent can automatically classify and select the right tools for queries or document tasks.  
- **REST & WebSocket Clients:** Interact with your agents through RESTful APIs or real-time WebSockets.  
- **Zero Boilerplate:** Set up your AI agent with just a YAML configuration file.  
- **Dynamic Tooling:** Automatically loads tools like RAG for document retrieval or prompt-based models for queries.
- **Muti-tenant** Session based document storing and AI agent communications.

---

## **How It Works**  
### **Core Workflow**  
1. **Load Configuration:** Define your AI agent in a YAML file.  
2. **Bootstrap Tools:** RAG tools and prompt models are set up based on the configuration.  
3. **Document Processing:** Store and retrieve chunks of knowledge in a vector database.  
4. **Smart Query Handling:** The AI agent selects the best tool for the task—fetching documents or generating direct answers.  

---

## **What Makes It Special?**  
- **Built-in RAG and Prompt Models:** No need to worry about complex logic and boilerplate code.  
- **Integrated Vector Database:** Store embeddings for private document retrieval.  
- **Supports REST & WebSockets:** Choose between structured API calls or event-driven real-time communication.  
- **Flexible AI Services:** Connect to providers like OpenAI seamlessly.  

---

## **Get Started**  
### **1. Using standalone core**  
```bash  
pip install kiss-ai-stack-core  
```  

### **2. Define Your Agent (YAML Example)**  
```yaml  
agent:  
  decision_maker:  
    name: decision_maker  
    role: classify tools for given queries  
    kind: prompt  
    ai_client:  
      provider: openai  
      model: gpt-4  
      api_key: <your-api-key>  
  tools:  
    - name: general_queries  
      role: process generic queries  
      kind: prompt  
      ai_client:  
        provider: openai  
        model: gpt-4  
        api_key: <your-api-key>  
    - name: document_tool  
      role: handle documents and RAG queries  
      kind: rag  
      embeddings: text-embedding-ada-002  
      ai_client:  
        provider: openai  
        model: gpt-4  
        api_key: <your-api-key>  
  vector_db:  
    provider: chroma  
    kind: in-memory  
```  

### **3. Run Your Agent standalone**  
```python  
from kiss_ai_stack import AgentStack  

async def main():  
    await AgentStack.bootstrap_agent(agent_id="my_agent", temporary=True)  
    response = await AgentStack.generate_answer(agent_id="my_agent", query="What is KISS AI Stack?")  
    print(response.answer)  

import asyncio  
asyncio.run(main())  
```  

---

## **KISS AI Stack Server**  
Run your AI agents on a server with REST and WebSocket APIs.  

### **Server Features**  
- **Lifecycle Management:** Control agent sessions (init, query, close).  
- **WebSocket Support:** Real-time event-driven communication.  
- **REST API:** Simple, structured API calls.  

### **Install the Server**  
```bash  
pip install kiss-ai-stack-server  
```

### **Add .env file**

```bash
ACCESS_TOKEN_SECRET_KEY = "your-secure-random-secret-key"
ACCESS_TOKEN_ALGORITHM = "HS256"
ACCESS_TOKEN_EXPIRE_MINUTES = 30

SESSION_DB_URL="sqlite://sessions.db"
```

### Define Your Agent (YAML Example)
Add the stack.yaml file same as in standalone core mode.

### **Start the Server**  
```python  
import asyncio  
from kiss_ai_stack_server import bootstrap_session_schema, agent_server, get_agent_server_app  
import uvicorn  

async def start_server():  
    await bootstrap_session_schema()  
    server_app = get_agent_server_app()  
    await agent_server(  
        config=uvicorn.Config(app=server_app, host='0.0.0.0', port=8080)  
    ).serve()  

asyncio.run(start_server())  
```  

---

## **REST and WebSocket Clients**  
### **REST Client**  
- Manage agent sessions, run queries, and store documents with simple API calls.  

```python  
from kiss_ai_stack_client import RestEvent  

client = RestEvent(hostname="localhost:8080", secure_protocol=False)
session = await client.authorize_agent(scope="temporary")
response = await client.bootstrap_agent(data="Hello, Agent!")
response = await client.generate_answer(data="How is the weather today?")
print(response.answer)  
```  

### **WebSocket Client**  
- Enjoy real-time interactions with your AI agent.  

```python  
from kiss_ai_stack_client import WebSocketEvent  

client = WebSocketEvent(hostname="localhost:8080", secure_protocol=False)  
session = await client.authorize_agent(scope="temporary")
response = await client.bootstrap_agent(data="Hello, Agent!")
response = await client.generate_answer(data="How is the weather today?")
print(response.answer)  
```  

---

## **Let’s Keep It Simple**  
KISS AI Stack takes care of the heavy lifting, so you can focus on delivering value. Start building your AI-assited solutions today! 🛠️
