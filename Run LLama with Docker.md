
To run **LLaMA (or any model via Ollama) as a Docker image**, you can use the **official Ollama Docker image**. Ollama provides a lightweight environment to serve LLaMA models (like `llama2`, `llama3`) locally or in containers.

---

## ✅ **Run LLaMA with Ollama via Docker**

### 🔹 Step 1: Pull the Docker Image

```bash
docker pull ollama/ollama
```

---

### 🔹 Step 2: Run Ollama Container

```bash
docker run -d --name ollama \
  -p 11434:11434 \
  -v ollama_data:/root/.ollama \
  ollama/ollama

docker run -d --name ollama  -p 11434:11434 ollama/ollama
```

> 🗂️ `-v ollama_data:/root/.ollama` mounts a volume to persist models across restarts.

`-v ollama_data: D:\Databases\ollama` is where my volume is stored

---

### 🔹 Step 3: Pull LLaMA Model inside Container

To enter the container:

```bash
docker exec -it ollama bash
```

Then pull the model:

```bash
ollama pull llama3
```

Or do it in one step by running:

```bash
docker exec ollama ollama pull llama3
```

---

### 🔹 Step 4: Test LLaMA

Once the model is pulled, you can test via:

```bash
curl http://localhost:11434/api/generate -d '{
  "model": "llama3",
  "prompt": "Explain quantum computing in simple terms."
}'
```

You’ll get a JSON stream response.

---

### 🔹 Step 5: Connect Spring Boot to LLaMA in Docker

In your **Spring Boot WebClient**, call:

```java
WebClient webClient = WebClient.create("http://localhost:11434");
```

Ollama will be reachable at `localhost:11434`.

---

### ✅ Tips for Production

- Use **Docker Compose** to pair Ollama + your Spring Boot service.
    
- Mount persistent volumes for storing downloaded models.
    
- Use **Nginx** as a reverse proxy for security/auth (if needed).
    

---

## 📁 Example Docker Compose (Optional)

```yaml
version: "3.9"
services:
  ollama:
    image: ollama/ollama
    ports:
      - "11434:11434"
    volumes:
      - ollama_data:/root/.ollama

volumes:
  ollama_data:
```

---


The error:

```
{
    "error": "model 'llama3' not found"
}
```

means that the `llama3` model has **not been downloaded** into the Ollama Docker container yet.

---

## ✅ Fix: Pull the `llama3` model **inside the container**

You have two options:

---

### 🔹 Option 1: Pull it manually inside the container

Run this in your terminal:

```bash
docker exec -it ollama ollama pull llama3
```

This downloads the model inside the running container.

---

### 🔹 Option 2: Pull the model automatically on container start

Update your Docker command:

```bash
docker run -d --name ollama \
  -p 11434:11434 \
  -v ollama_data:/root/.ollama \
  ollama/ollama

docker run -d --name ollama  -p 11434:11434  -v ollama_data:/root/.ollama   ollama/ollama

# Then pull the model:
docker exec ollama ollama pull llama3
```

> You only need to pull the model **once** (if you're using a persistent volume like `ollama_data`).

---

## 🔍 Verify Model Exists

Check if `llama3` is downloaded:

```bash
docker exec ollama ollama list
```

You should see something like:

```
NAME      SIZE      MODIFIED
llama3    4.2 GB    2 minutes ago
```

---

## 🔁 Try the API again

Once the model is pulled, run:

```bash
curl http://localhost:11434/api/generate -d '{
  "model": "llama3",
  "prompt": "What is quantum computing?"
}'
```

✅ It should now respond with model output.

---

Would you like to **automate the pull step using Docker Compose** or a **custom Dockerfile** that preloads models?

**REFERENCES** : https://hub.docker.com/r/ollama/ollama