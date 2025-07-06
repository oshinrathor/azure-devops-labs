# Understanding Kubernetes and Docker Basics

---

## 1. What are Pods, Deployments, and Services in Kubernetes?

- Pod: Smallest unit in Kubernetes, encapsulates one or more containers sharing the same network and storage.
- Deployment: Manages multiple identical Pods, ensuring desired replicas, rolling updates, and automatic restarts.
- Service: Provides a stable IP and DNS to expose Pods, enabling load balancing and discovery.

---

## 2. What is a Dockerfile and how is it used for an HTML-only application?

- A Dockerfile is a text file with instructions to build a Docker image containing your app and its environment.
- For static HTML apps, Dockerfile packages the HTML files with a lightweight web server (e.g., NGINX) to serve them.
- Example Dockerfile for static HTML app:

    FROM nginx:alpine
    RUN rm -rf /usr/share/nginx/html/*
    COPY . /usr/share/nginx/html
    EXPOSE 80

- Build and run commands:

    docker build -t my-html-app .
    docker run -d -p 8080:80 my-html-app

---

## 3. What are dependencies, runtime, and environment configuration? Examples?

Concept              | Example         | Role
-------------------- | --------------- | -------------------------------
Dependency           | nginx           | Software your app needs
Runtime              | Alpine Linux    | Platform where app runs
Environment Config   | PORT=80, ENV=prod | Settings like ports, variables

---

## 4. Why is a server needed to serve HTML/CSS/JS?

- Browsers can open files locally but a web server:
  - Enables access over HTTP on the network.
  - Serves multiple files properly (HTML, CSS, JS, images).
  - Ensures correct behavior with browser security (CORS, fetch, etc.).

"Serve themselves" means HTML files can’t respond to web requests; a server like NGINX is needed.

---

## 5. What’s special about NGINX? How is it different from Apache?

Feature       | NGINX                    | Apache
------------- | ------------------------ | ------------------------
Performance   | High (event-driven)      | Good (process/thread)
Memory Usage  | Low                      | Higher
Configuration | Simpler                  | More complex but flexible
Use case      | Static content, proxy    | Dynamic sites, PHP etc.

NGINX is lightweight and fast, ideal for static content.

---

## 6. What does a Dockerfile do overall?

- Packages your app code with dependencies, runtime, and environment config into a Docker image.
- This image can run anywhere as a container, ensuring portability and consistency across environments.

---

## 7. Visual Diagram (Text-based)


Your HTML Code (index.html, style.css, etc.)
   │
   >
Dockerfile
   │
   >
Docker Build
 (Creates Docker Image)
   │
   >
Docker Image
 (App + NGINX + Runtime)
   │
   >
Docker Run
 (Starts Container)
   │
   >
Docker Container
 (Running NGINX server)
   │
   >
Access via: http://localhost:8080
---

## 8. Does the container keep running forever? Can we stop and store it?

- When you run a container, it starts and keeps running (e.g., NGINX serving your HTML).
- It runs "forever" until you stop it manually (e.g., `docker stop <container_id>`).
- Containers are ephemeral runtime instances; stopping pauses it but data/state remain unless deleted.
- To store your app permanently, use a Docker image.
- Docker image: read-only package of your app + runtime + dependencies.
- Container: running instance of an image.
- Workflow:
    1. Build Docker image from Dockerfile.
    2. Push image to Docker registry (Docker Hub, etc.).
    3. Pull image and run containers anywhere anytime.

---

## 9. Summary

- Kubernetes Pods: smallest deployable units with containers.
- Deployments: manage Pods replicas and updates.
- Services: expose Pods with stable network access.
- Dockerfile: blueprint to build Docker images.
- Images: package app + dependencies + runtime + config.
- Containers: running instances of images.
- Servers (like NGINX) needed to serve static files over HTTP.
- NGINX is lightweight, fast, event-driven vs Apache’s process-based model.
- Docker enables portability and consistent runtime environments.

---
