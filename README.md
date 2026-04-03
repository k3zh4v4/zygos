# Zygos API

Zygos is a lightweight, containerized API service that leverages a local AI model to provide intelligent verdicts on text content. Built with FastAPI for high performance, it runs its AI inference via an Ollama model (gemma3 - 4 Billion parameters) hosted in a Podman container, and uses ngrok to securely expose the local endpoint to the web.

###
I have particulary used gemma3 because of its intelligent classification compared to other slms like qwen or llama and support of over 100+ languages which is great when i think about analyzing spam samples by being multilinguistic
###

## 🚀 Architecture overview
* **Web Framework:** FastAPI (Python)
* **Containerization:** Podman
* **AI Engine:** ollama - **gemma3** 4B parameters (Local SLM)
* **Tunneling:** ngrok

## 📋 Prerequisites

Before you begin, ensure you have the following installed on your system:
* [Python 3.8+](https://www.python.org/downloads/)
* [Podman](https://podman.io/getting-started/installation)
* [ngrok](https://ngrok.com/download)
* An active ngrok account/authtoken

## 🛠️ Installation & Setup

**1. Clone the repository**
```bash
git clone [https://github.com/yourusername/zygos-api.git](https://github.com/yourusername/zygos-api.git)
cd zygos-api
```

**2. Set up the Python Virtual Environment**
It's recommended to isolate your Python dependencies.
```
Bash
python -m venv venv
source venv/bin/activate  # On Windows use `venv\Scripts\activate`
```

**3. Install Dependencies**
```
Bash
pip install fastapi uvicorn requests
(Note: If you have a requirements.txt, use pip install -r requirements.txt instead).
```

**4. Spin up the Ollama Container**
Start the local AI engine using Podman. Make sure your desired model is pulled and ready.
```
Bash
podman run -d -v ollama:/root/.ollama -p 11434:11434 --name ollama ollama/ollama
```

⚙️ Running the Service
You will need two separate terminal windows for this step.

Terminal 1: Start the FastAPI Server
Run the Python script to start the local API server.

Bash
uvicorn main:app --reload --host 127.0.0.1 --port 8000
The API is now running locally at http://127.0.0.1:8000.

Terminal 2: Start the ngrok Tunnel
Expose your local port 8000 to the internet.

Bash
ngrok http 8000
Copy the forwarding URL provided by ngrok (e.g., https://<random-id>.ngrok-free.app).

📖 Usage
You can test the API by sending a POST request to the ngrok URL.

Using cURL:
Replace <YOUR_NGROK_URL> with your actual forwarding address.
```
Bash
curl -X POST "<YOUR_NGROK_URL>/analyze" \
     -H "Content-Type: application/json" \
     -d '{"text": "Enter the text content you want the AI to analyze and provide a verdict on here."}'
Expected Response:

JSON
{
  "verdict": "The model's generated response goes here...",
  "status": "success"
}
```
🛑 Stopping the Environment
When you are done, you can cleanly shut down the environment:

Stop the FastAPI server: Ctrl + C in Terminal 1.

Stop ngrok: Ctrl + C in Terminal 2.

Stop the Podman container: podman stop ollama
