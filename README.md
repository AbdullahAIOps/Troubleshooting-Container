# 🛠️ Lab 13: Troubleshooting Containers with Podman

## 🎯 Objectives

By the end of this lab, you will be able to:

- Diagnose container failures using logs and inspect commands
- View container logs with filters
- Inspect container state
- Use `exec` to debug inside a running container

---

## 📋 Prerequisites

- Basic understanding of command-line interface (CLI)
- Podman or Docker installed (Podman preferred for OpenShift compatibility)
- A running container (we will create an Nginx container)

---

## ⚙️ Lab Setup

### 🔹 Install Podman (if not installed)

**For RHEL/CentOS/Fedora:**

```bash
sudo dnf install -y podman
```

**For Debian/Ubuntu:**

```bash
sudo apt-get install -y podman
```

---

### 🔹 Pull and Run Nginx Container

```bash
podman pull docker.io/library/nginx:alpine

podman run -d --name nginx-test -p 8080:80 nginx:alpine
```

✅ *This creates a lightweight Nginx container for testing.*

---

## 🧪 Task 1: View Container Logs with Filters

### 🔹 Subtask 1.1: View Basic Logs

```bash
podman logs nginx-test
```

✅ *Expected Output*: Nginx access and error logs.

**Real-time log monitoring:**

```bash
podman logs -f nginx-test
```

⏳ *Press `Ctrl+C` to stop the log stream.*

---

### 🔹 Subtask 1.2: Filter Logs by Time

**Show logs from the last 5 minutes:**

```bash
podman logs --since 5m nginx-test
```

**Display the last 10 lines of logs:**

```bash
podman logs --tail 10 nginx-test
```

📌 *If no logs appear, check if the container is running:*

```bash
podman ps
```

---

## 🧾 Task 2: Inspect Container State

### 🔹 Subtask 2.1: Check Container Status

**List running containers:**

```bash
podman ps
```

✅ *Should show `nginx-test` container.*

**Inspect detailed state:**

```bash
podman inspect nginx-test
```

🔍 *Key fields to observe:*
- `"State"`: status, exit code, error
- `"Config"`: environment variables, entrypoint
- `"NetworkSettings"`: IP address, exposed ports

---

### 🔹 Subtask 2.2: Check Resource Usage

```bash
podman stats nginx-test
```

📊 *Displays live CPU, memory, and network usage.*

💡 *If the container is stuck or unresponsive:*

```bash
podman stop nginx-test
podman start nginx-test
```

---

## 🐚 Task 3: Use `exec` to Debug Inside Container

### 🔹 Subtask 3.1: Enter Container Shell

```bash
podman exec -it nginx-test /bin/sh
```

✅ *This gives you a shell inside the container.*

**Check running processes:**

```bash
ps aux
```

🧠 *Use this to verify if Nginx is running inside.*

---

### 🔹 Subtask 3.2: Debug Nginx Issues

**View Nginx configuration:**

```bash
cat /etc/nginx/nginx.conf
```

**Test HTTP server from inside the container:**

```bash
curl localhost
```

✅ *Expected Output*: Default Nginx welcome page HTML.

🛠 *If `/bin/sh` doesn’t work, try:*

```bash
podman exec -it nginx-test /bin/bash
```

---

## ✅ Summary

| Task            | Action Performed                        | Outcome                             |
|-----------------|------------------------------------------|--------------------------------------|
| View Logs       | `podman logs`, filters, `--since`, `-f` | Real-time & filtered container logs |
| Inspect State   | `podman inspect`, `podman stats`        | Detailed status and resource usage  |
| Shell Access    | `podman exec -it <name> /bin/sh`        | Debug container processes directly  |
| Test Services   | `curl`, `cat` config                    | Validate running app from inside    |

---

## 👨‍💻 Author

**Abdullha Saleem** – DevOps | Linux | Docker | Podman | Kubernetes | AIOps  
📫 *Find me on GitHub or LinkedIn for more labs and DevOps content.*
