# 🚀 Dockerized Web App to Kubernetes Using Ansible

This project demonstrates full-stack DevOps automation — from building a web app, containerizing it with Docker, deploying it on a Kubernetes cluster, and automating the entire pipeline using Ansible.

---

## 📌 Project Description

This end-to-end automation includes:
- Building a Docker image for a Python Flask web application.
- Pushing the image to Docker Hub.
- Deploying the app to a Kubernetes cluster (Minikube).
- Automating all steps using an Ansible playbook.

---

## 🛠️ Tech Stack

| Layer        | Tools & Technologies            |
|-------------|----------------------------------|
| Frontend     | HTML, CSS                       |
| Backend      | Python Flask                    |
| Containerization | Docker                    |
| Orchestration | Kubernetes (via Minikube)     |
| Automation   | Ansible                         |

---

## 🌐 Web App Structure

```

my-webapp/
├── app.py
├── requirements.txt
├── templates/
│   └── index.html

````

### `app.py`
A basic Flask application that returns the internal pod IP and public IP via an API route `/api/info`.

---

## 🐋 Dockerfile

```dockerfile
FROM python:3.9-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install -r requirements.txt

COPY . .

EXPOSE 80

CMD ["python", "app.py"]
````

---

## ☸️ Kubernetes Manifests

### `deployment.yml`

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: webapp-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: webapp
  template:
    metadata:
      labels:
        app: webapp
    spec:
      containers:
      - name: webapp
        image: dipakrasal/app1:latest
        ports:
        - containerPort: 80
```

### `service.yml`

```yaml
apiVersion: v1
kind: Service
metadata:
  name: webapp-service
spec:
  type: NodePort
  selector:
    app: webapp
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30080
```

---

## 🤖 Ansible Playbook

### `playbook.yml`

```yaml
- name: Build Docker Image and Deploy to Kubernetes
  hosts: localhost
  connection: local
  vars:
    image_name: dipakrasal/app1:latest
    app_path: /home/hppc/Desktop/github/DevOps-Projects/Project3/app
    k8s_path: /home/hppc/Desktop/github/DevOps-Projects/Project3/k8s
    dockerhub_username: YOUR_USERNAME
    dockerhub_password: YOUR_PASSWORD

  tasks:
    - name: Login to Docker Hub
      docker_login:
        username: "{{ dockerhub_username }}"
        password: "{{ dockerhub_password }}"

    - name: Build Docker Image
      docker_image:
        name: "{{ image_name }}"
        build:
          path: "{{ app_path }}"
        source: build
        push: yes

    - name: Apply Deployment
      command: kubectl apply -f {{ k8s_path }}/deployment.yml
      args:
        chdir: "{{ k8s_path }}"

    - name: Apply Service
      command: kubectl apply -f {{ k8s_path }}/service.yml
      args:
        chdir: "{{ k8s_path }}"
```

---

## 🧪 Running the Project

1. Start Minikube:

   ```bash
   minikube start
   ```

2. Run the Ansible Playbook:

   ```bash
   ansible-playbook playbook.yml
   ```

3. Access the Web App:

   ```bash
   minikube service webapp-service --url
   ```

---

## ✅ Key Learnings

* Automating Docker + Kubernetes with Ansible simplifies DevOps workflows.
* Manual testing before automation helps ensure stability.
* Local Kubernetes via Minikube is great for learning and development.

---

## 📁 Project Repository

👉 [GitHub Link](https://github.com/abhi-gadge1773/Dockerized-Web-App-to-Kubernetes-Using-Ansible.git)


## 🙌 Acknowledgements

Inspired by real-world CI/CD automation pipelines and DevOps best practices.

---

## 🧑‍💻 Author

**Abhijeet Gadge**
[LinkedIn](https://www.linkedin.com/in/abhijeetgadge/) | [GitHub](https://github.com/abhi-gadge1773)


---

Let me know if you'd like a downloadable version (PDF or `.md` file), or if you'd like it tailored for a **LinkedIn blog**, **Dev.to**, or **Hashnode** format too!
```
