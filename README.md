# AWS Beanstalk

### Notion View (https://www.notion.so/AWS-Beanstalk-33e50c8296ee8009b7cdd5b0ede58d51?source=copy_link)

![image.png](https://github.com/Mayank1242/AWS_Beanstalk_Session/blob/main/image.png)

## What is AWS Elastic Beanstalk?

Elastic Beanstalk is a service where you:

⇒ Upload your code

⇒ Choose runtime (Node.js, Python, Java, .NET, PHP, Docker, etc.)

⇒ AWS automatically creates infrastructure

Behind the scenes, Beanstalk automatically sets up:

- EC2 instances
- Load balancer
- Auto Scaling
- Security groups
- Monitoring (CloudWatch)
- Deployment pipeline basics

So instead of managing infrastructure manually, **you focus only on application code** 

---

## Why do we use Elastic Beanstalk?

Main purpose:

> Deploy applications fast without DevOps overhead
> 

Best when:

1. You want quick deployment
2.  You don’t want to configure EC2 manually
3. You want auto-scaling automatically
4. You want production-ready setup fast

Example:

Instead of:

Launch EC2 → install Node → configure Nginx → setup SSL → configure scaling

You simply:

Upload code → Beanstalk deploys app automatically

---

## Real-world use cases of Elastic Beanstalk

### 1.  Startup MVP deployment

Example:

A startup builds:

- React frontend
- Node.js backend
- MongoDB Atlas database

They deploy backend using Beanstalk because:

✔ fast

✔ scalable

✔ minimal DevOps effort

---

### 2.  Student  Projects

For example:

Your MERN stack students build:

- authentication system
- REST APIs
- dashboards

Instead of complex AWS setup:

👉 deploy backend via Beanstalk easily

---

### 3. Internal company tools

Example:

HR portal

Admin dashboards

Automation tools

Companies use Beanstalk because:

No infrastructure maintenance needed

---

### 4. Scalable APIs

Example:

Payment microservice

Notification service

User management API

Beanstalk automatically scales instances based on traffic 

---

## How Elastic Beanstalk works internally

When you deploy app:

```
Upload code
   ↓
Beanstalk creates EC2
   ↓
Configures load balancer
   ↓
Creates Auto Scaling group
   ↓
Monitors health
   ↓
Deploys app automatically
```

So actually:

> Beanstalk = automation layer on top of EC2
> 

---

## Deployment comparison: Beanstalk vs EC2 vs S3

Here’s the most important part:

| Feature | Beanstalk | EC2 | S3 |
| --- | --- | --- | --- |
| Type | PaaS | IaaS | Storage hosting |
| Server setup | Automatic | Manual | No server |
| Scaling | Auto | Manual setup | Not needed |
| Load balancer | Automatic | Manual | No |
| Best for | Backend apps | Full control apps | Static websites |
| DevOps effort | Low | High | Very low |
| Runtime support | Node, Python, Java etc | Any | HTML/CSS/JS only |
|  |  |  |  |

## **Project React:**

# 1. React alone doesn’t work on Beanstalk

1. React projects created with:(If you already have no need to this step)

```
npx create-react-app
```

contain:

```
src/
public/
package.json
```

These are **development files**, not production-ready web files.

Beanstalk expects:

```
something that runs with npm start
```

So React must be **built first**.

---

# 2. Correct way to deploy React on Beanstalk (working method)

## Step 1 — Build React project

Inside your React project:

```
npm install
npm run build
```

This creates:

```
build/
```

This folder contains production website files.

---

# Step 2 — Add a small Node server

Create:

```
server.js
```

Add(If you use Es6 convert this to Es6 & use dist if you use Vite):

```jsx
const express=require("express");
const path=require("path");

const app=express();

app.use(express.static(path.join(__dirname,"build")));

app.get("*", (req,res) => {
res.sendFile(path.join(__dirname,"build","index.html"));
});

app.listen(process.env.PORT||8080);

```

---

**For Es6 with Vite:**

```jsx
import express from "express";
import path from "path";
import { fileURLToPath } from "url";

const app = express();

// Fix __dirname for ES modules
const __filename = fileURLToPath(import.meta.url);
const __dirname = path.dirname(__filename);

// Serve static files from dist folder
app.use(express.static(path.join(__dirname, "dist")));

// React Router support (SPA fallback)
app.get("*", (req, res) => {
res.sendFile(path.join(__dirname, "dist", "index.html"));
});

// Start server
const PORT = process.env.PORT || 8080;
app.listen(PORT, () => {
console.log(`Server running on port ${PORT}`);
});
```

# Step 3 — Update package.json

Change scripts to:

```json
"scripts": {
  "start":"node server.js"
}
```

Beanstalk runs:

```
npm start
```

automatically.

---

# Step 4 — Create ZIP correctly (VERY important)

Zip contents like this:

```
project.zip
 ├── build/
 ├── server.js
 ├── package.json
```

NOT:

```
project.zip
 └── project/
      ├── build/
```

(extra folder breaks deployment)

---

# Step 5 — Upload ZIP to Beanstalk

Choose:

```
Application code → Local file → Upload ZIP
```

Deployment works - yes

---

# Do you need node_modules in ZIP?

No ❌

Beanstalk automatically runs:

```
npm install
```

after extracting your ZIP.

So your ZIP should contain:

```
package.json
```

and Beanstalk installs dependencies itself.

## Later Implementaion

1. 🐳 Install and use Docker if Not.
2. 🖼️ Build a custom container image of your project.
3. 🚢 Run your containerized application locally.
4. ☁️ Deploy your containerized application to AWS Elastic Beanstalk, and access it live on the web.

![image.png](https://github.com/Mayank1242/AWS_Beanstalk_Session/blob/main/dockerproject.png)
