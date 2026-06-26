# 📝 Django Notes App and Jenkins 
> Deployed using Docker on AWS EC2

![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat&logo=docker&logoColor=white)
![Django](https://img.shields.io/badge/Django-092E20?style=flat&logo=django&logoColor=white)
![AWS](https://img.shields.io/badge/AWS-232F3E?style=flat&logo=amazon-aws&logoColor=white)

---

## 🌐 Live Demo
http://35.175.185.252:80/

---

## 🛠️ Tech Stack
- **Backend:** Python, Django
- **Container:** Docker
- **Web Server:** Django Dev Server
- **Cloud:** AWS EC2 (Ubuntu)

---



### Step 2: Dockerfile Banaya
```dockerfile
FROM python:3.10-slim
WORKDIR /app
COPY . .
RUN pip install -r requirements.txt
RUN python manage.py migrate
EXPOSE 8000
CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]
```

### Step 3: Docker Image Build Ki
```bash
docker build -t django-notes-app .
```

### Step 4: Container Chalaya
```bash
docker run -d --name django-app -p 80:8000 django-notes-app
```

### Step 5: AWS Security Group
```
Port 80 aur 8000 open kiya
Inbound Rules mein! ✅
```

---

## ✅ Verify Karo
```bash
# Container chal raha hai?
docker ps

# Logs dekho
docker logs django-app
```

---

## 📌 Lessons Learned
- ✅ Dockerfile likhna
- ✅ Docker image build karna
- ✅ Container deploy karna
- ✅ AWS Security Groups configure karna

---

*Made with ❤️ by Nasir Baloch — DevOps Journey 🚀*
