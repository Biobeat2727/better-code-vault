# Docker

**Docker** is a tool that packages applications into containers for consistent environments across development, testing, and production.

---

## 🚀 Why Docker?

- Works the same on every machine
- Avoids "works on my machine" bugs
- Great for deploying microservices and APIs
- Easy to share environments

---

## 🐳 Basic Dockerfile

```Dockerfile
FROM node:18

WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .

CMD ["npm", "start"]
```

---

## 🧪 Common Commands

```bash
docker build -t my-app .
docker run -p 3000:3000 my-app
docker-compose up
```

---

## 📚 Resources

- https://docs.docker.com
