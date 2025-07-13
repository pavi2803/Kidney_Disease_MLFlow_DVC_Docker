# Kidney Disease Detection App - Docker + Render Deployment

This repository contains a Streamlit web application to detect signs of **Kidney Disease** using a deep learning model built with **TensorFlow (VGG16 transfer learning)**.

---

## 🔍 App Features

* Upload a kidney CT scan image (JPG/PNG)
* The model predicts:

  * ✅ No Kidney Disease
  * 🚨 Likely Kidney Disease
* Designed as a **supportive AI assistant** for medical professionals — **not a replacement for diagnosis**.

---

## ✅ Live App

🧪 Try the app here → https://kidney-disease-app-latest.onrender.com/

---

## 🧠 Model Overview

* Base Model: `VGG16` pretrained on ImageNet
* Training: Fine-tuned using transfer learning
* Framework: `TensorFlow 2.19.0`
* Input Shape: `224x224x3`
* Output: Binary classification (0: No disease, 1: Disease)

---

## 🐳 Docker Setup

### Dockerfile

```Dockerfile
FROM python:3.12.7
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
CMD ["streamlit", "run", "app.py", "--server.port=8080", "--server.address=0.0.0.0"]
```

### requirements.txt (snippet)

```
tensorflow==2.19.0
streamlit==1.24.0
pillow
numpy
```

---

## 🚀 Deploying to Render using Docker Hub

1. **Build and Push Docker Image to Docker Hub**

   ```bash
   docker build -t pavi2803/kidney-disease-app:latest .
   docker push pavi2803/kidney-disease-app:latest
   ```

2. **Set Up Render Web Service**

   * Go to [Render Dashboard](https://dashboard.render.com/)
   * Click **New Web Service** > **Existing Image**
   * Enter:

     * **Image URL**: `docker.io/pavi2803/kidney-disease-app:latest`
     * **Port**: `8080`
     * **Start Command**: leave blank (Docker CMD handles this)
   * Click **Connect**

---

## 🔁 CI/CD with GitHub Actions

This repo supports GitHub Actions to auto-build and push Docker images on every push to `main` branch.

### .github/workflows/docker.yml

```yaml
name: CI/CD - Docker Push to Docker Hub

on:
  push:
    branches: [ main ]

jobs:
  docker-build-push:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Log in to Docker Hub
        uses: docker/login-action@v2
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}
      - name: Build Image
        run: docker build -t ${{ secrets.DOCKER_USERNAME }}/kidney-disease-app:latest .
      - name: Push Image
        run: docker push ${{ secrets.DOCKER_USERNAME }}/kidney-disease-app:latest
```

---

## ⚠️ Disclaimer

This app is for **educational and prototyping purposes only**. It is not intended for clinical use or diagnostic decision-making. Always consult medical professionals.

---

## 👤 Author

Pavithra Senthilkumar | `pavi2803`

* GitHub: [@pavi2803](https://github.com/pavi2803)
* Email: [pavithrasenthil03@gmail.com](mailto:pavithrasenthil03@gmail.com)
