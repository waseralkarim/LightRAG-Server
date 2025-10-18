# LightRAG-Server
The LightRAG Server is designed to provide Web UI and API support. The Web UI facilitates document indexing, knowledge graph exploration and a simple RAG query interface.

## Default Installation
Follow other documents of this repo in order to gain more production ready setup.

### **Step 1: Install python3-venv**

```bash
sudo apt update
sudo apt install python3-venv -y
```

- This installs the modules required to create Python virtual environments.

---

### **Step 2: Move LightRAG to a user-writable location (recommended)**

```bash
mkdir -p ~/LightRAG
cp -r /opt/LightRAG/* ~/LightRAG/
cd ~/LightRAG
```

> Now you have full permissions as the devops user.
> 

---

### **Step 3: Create the virtual environment**

```bash
python3 -m venv venv
```

- It will create the venv in `~/LightRAG/venv`.

---

### **Step 4: Activate the virtual environment**

```bash
source venv/bin/activate
```

- Your shell prompt should now show `(venv)`.

---

### **Step 5: Install LightRAG dependencies**

```bash
pip install --upgrade pip
pip install -e ".[api]"
```

---

 After this, you can continue with **`.env` setup** and running `lightrag-server`.

### **Step 6: Configure Environment**

1. Copy the example environment file:

```bash
cp env.example .env
```

1. Edit `.env` to configure your setup:

```bash
nano .env
```

Key settings to configure:

- `LLM_MODEL` → the LLM you want to use (ex: OpenAI or local LLM)
- `EMBEDDING_MODEL` → ex: `BAAI/bge-m3` or `text-embedding-3-large`
- `RERANKER_MODEL` → optional, improves query quality
- `OPENAI_API_KEY` → if using OpenAI API
- Adjust ports or storage paths if needed

> The embedding model must match for both indexing and querying.
> 

---

### **Step 7: Add Your Documents**

1. Create a folder for your documents:

```bash
mkdir ./my_documents
```

1. Copy your documents (PDF, TXT, DOCX, CSV, etc.) into this folder:

```bash
cp ~/source_docs/* ./my_documents/
```

---

### **Step 8: Pre-Build Document Index (Optional but Recommended)**

- Pre-indexing saves time for devs using the server later.

```bash
python examples/lightrag_openai_demo.py
```

- Replace demo documents with your `./my_documents`.
- Make sure `.env` has the correct embedding model and API key (if using OpenAI).
- After this step, LightRAG stores **embeddings and vector data** in its `data/` directory.

---

### **Step 9: Start the LightRAG Server**

With the virtual environment activated:

```bash
lightrag-server
```

### **Step 10: Test the Server**

Example curl query:

```bash
curl -X POST "http://<VM_IP>:8000/query" \
-H "Content-Type: application/json" \
-d '{"question": "Summarize document X"}'
```

- You should get an AI-generated response based on your indexed documents.

---

### **Step 11: Make the Server Persistent (Optional)**

- To run the server automatically on VM boot, create a **systemd service**:

```
# /etc/systemd/system/lightrag.service
[Unit]
Description=LightRAG Server
After=network.target

[Service]
User=devops
WorkingDirectory=/home/devops/LightRAG
ExecStart=/home/devops/LightRAG/venv/bin/lightrag-server
Restart=always

[Install]
WantedBy=multi-user.target
```

Enable & start the service:

```bash
sudo systemctl daemon-reload
sudo systemctl enable lightrag
sudo systemctl start lightrag
```
<img width="1923" height="914" alt="Screenshot (22)" src="https://github.com/user-attachments/assets/d33ecbbb-4bee-4324-8651-eed3faa084ca" />

