# 🔥 Ethical Hacking Firewall Simulation (Docker + Python)

A complete ethical hacking mini-project demonstrating:

- DoS-style attack simulation  
- Python-based firewall with IP rate limiting  
- Real-time traffic monitoring dashboard  
- Docker-based microservice architecture  
- Attacker container executed manually  

---

## 🚀 Project Overview

This project simulates a **real-world attack and defense scenario**:

1. A **Firewall container** monitors and forwards requests.
2. A **Victim Flask app** sits behind the firewall.
3. An **Attacker container** sends 1000 fast requests.
4. Firewall analyzes traffic and:
   - Allows first few requests
   - Blocks abusive IPs (403)
   - Visualizes results on dashboard

Dashboard URL:  
👉 **http://localhost:8080/dashboard**

---

## 📁 Directory Structure

```
docker-firewall-lab/
├── docker-compose.yml
├── firewall_app/
│   ├── Dockerfile
│   ├── app.py
│   ├── requirements.txt
│   └── templates/
│       └── dashboard.html
├── victim_app/
│   ├── Dockerfile
│   ├── app.py
│   └── requirements.txt
└── attacker/
    ├── Dockerfile
    ├── attack.py
    └── requirements.txt
```

---

## 🧩 Architecture

```
Browser ──> Firewall (8080) ──> Victim Server (8000)
                │
                └──> Dashboard (Traffic Monitoring)

Attacker Container ──> Firewall (Flood Requests)
```

Firewall calculates:

- Requests per IP  
- Allowed vs Blocked  
- Temporary banning  
- Live stats  

---

## ⚙️ How to Run the Project

### **1. Build all containers**
```bash
docker compose build
```

### **2. Start firewall + victim**
```bash
docker compose up -d
```

### **3. Check running containers**
```bash
docker ps
```

You should see:

- `firewall_app`
- `victim_app`

---

## 🖥️ Access Dashboard

Open:

👉 **http://localhost:8080/dashboard**

The dashboard shows:

- Pie chart: Allowed vs Blocked
- Bar chart: Per-IP stats
- Table: IP → Count, Allowed, Blocked, Status

Dashboard refreshes every 2 seconds.

---

## 🔎 Test Normal Traffic

Open:

👉 **http://localhost:8080/**

Refresh a few times.

You will see your IP in dashboard as:

```
ACTIVE
Allowed: few
Blocked: 0
```

---

## 🔥 Run Attacker (Manual Trigger)

### Attack with self-deleting container
```bash
docker compose --profile manual run --rm attacker
```

### Attack but keep container for logs
```bash
docker compose --profile manual run attacker
```

You will see:

```
[ATTACKER] Total requests: 1000
200: 50
403: 950
```

Dashboard marks attacker IP as **BLOCKED**.

---

## 📜 View Logs

### Firewall logs
```bash
docker logs -f firewall_app
```

### Victim logs
```bash
docker logs -f victim_app
```

### Attacker logs (if container kept)
```bash
docker ps -a
docker logs -f <attacker-container-name>
```

---

## 🧹 Cleanup & Docker Maintenance Commands

#### Stop & remove containers
```bash
docker compose down
```

#### Remove all unused containers, networks, images
```bash
docker system prune -f
```

#### Remove unused networks
```bash
docker network prune -f
```

#### Remove specific container
```bash
docker rm -f <container>
```

---

## 🛠 Useful Docker Commands

Enter a container shell:
```bash
docker exec -it firewall_app sh
```

Show last 100 log lines:
```bash
docker logs --tail 100 firewall_app
```

Restart services:
```bash
docker compose restart
```

---

## 📌 Expected Behavior Summary

| Scenario | Result |
|----------|---------|
Normal browsing | IP = ACTIVE |
Repeat refresh | Eventually BLOCKED |
Run attacker | 950+ 403 blocks |
Dashboard open | Real time stats |

---

## 🎯 What You Learn

- How DoS attacks work  
- How firewalls mitigate floods  
- Docker microservice networking  
- Flask-based reverse proxy logic  
- Visualizing cyber attacks  
- Ethical hacking simulation  

---

## 🏁 Conclusion

This project is a complete **Attack + Defense Lab** suitable for:

- PBL submissions  
- Cybersecurity demonstrations  
- Ethical hacking learning  
- Portfolio showcase  
- Interviews  
