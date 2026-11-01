# **Docker FastAPI Demo**

A lightweight FastAPI application containerized with Docker. This project demonstrates how to:

- Install Docker  
- Build a Docker image  
- Run containers  
- Map ports using `-p`  
- Understand container behavior (files, multiple instances, etc.)

---

## **📁 Project Structure**

```
.
├── app.py
└── Dockerfile
```

---

## **🚀 FastAPI Application**

`app.py` contains a minimal FastAPI service with two endpoints:

- `GET /` → Returns a simple JSON message  
- `GET /health` → Returns a health status  

Example:

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def root():
    return {"message": "Hello from Docker!", "status": "running"}

@app.get("/health")
def health():
    return {"status": "healthy"}
```

---

## **🐳 Dockerfile**

The Dockerfile defines how the FastAPI app is packaged into a container:

```
FROM python:3.11-slim
WORKDIR /app
RUN pip install fastapi uvicorn
COPY app.py .
CMD ["uvicorn", "app:app", "--host", "0.0.0.0", "--port", "8000"]
```

### **What this does:**

- Uses Python 3.11 slim image  
- Sets `/app` as the working directory  
- Installs FastAPI + Uvicorn  
- Copies your application file  
- Starts the server on port 8000  

---

## **🔨 Build the Docker Image**

Run this inside the project folder:

```
docker build -t my-fastapi .
```

The `.` at the end is required — it tells Docker to use the current directory as the build context.

---

## **▶️ Run the Container**

Expose the FastAPI app to your host machine:

```
docker run -p 8000:8000 my-fastapi
```

Visit:

```
http://localhost:8000
```

---

## **🔌 Port Mapping Explained (`-p`)**

Docker uses:

```
-p HOST_PORT:CONTAINER_PORT
```

Example:

```
docker run -p 8001:8000 my-fastapi
```

This runs the same container but exposes it on a different host port:

- Container listens on **8000**
- Host exposes it on **8001**

You can run multiple containers from the same image by changing the host port.

---

## **📦 Running Multiple Containers**

Each `docker run` creates a **new container**.

Example:

```
docker run -p 8000:8000 my-fastapi
docker run -p 8001:8000 my-fastapi
```

You now have:

- Container #1 → http://localhost:8000  
- Container #2 → http://localhost:8001  

Both isolated, both running the same app.

---

## **🗂️ What Happens to Files Inside a Container?**

- **Stopping a container:** files remain  
- **Restarting a container:** files remain  
- **Removing a container (`docker rm`)**: files are deleted  
- **Running a new container from the same image:** files do *not* carry over  
- **Using volumes:** files persist across containers  

---

## **🧰 Useful Docker Commands**

List running containers:

```
docker ps
```

List all containers:

```
docker ps -a
```

List images:

```
docker images
```

Remove a container:

```
docker rm <container_id>
```

Remove an image:

```
docker rmi <image_name>
```

---

## **📚 Summary**

This assignment demonstrates:

- How Docker images and containers work  
- How to build and run a FastAPI app inside Docker  
- How port mapping allows external access  
- How container files behave  
- How multiple containers can run from the same image  

It’s a foundational exercise for deploying Python applications in real-world environments.

