# Running the Project

This project consists of two services running in parallel:

- **Spring Boot Application (Port 8080)** – Generates application metrics and simulates system behavior.
- **FastAPI ML Service (Port 8000)** – Collects metrics from Spring Boot, performs anomaly detection and health analysis, and exposes system insights.

Optionally, you can run a **k6 load test** to generate realistic traffic.

## Start the services in this order

1. Spring Boot Application
2. ML Service
3. k6 Load Test (optional)

---

## Step 1: Start the Spring Boot Application

Run the application using:

```bash
./mvnw spring-boot:run
```

or simply run:

```
DemoApplication.java
```

The application starts on:

```
http://localhost:8080
```

Swagger UI:

```
http://localhost:8080/swagger-ui.html
```

---

## Step 2: Start the ML Service

Open another terminal.

### If the virtual environment is already set up

```bash
cd ml-service
source .venv/bin/activate
uvicorn app.main:app --reload --port 8000
```

### First-time setup only

```bash
cd ml-service
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
uvicorn app.main:app --reload --port 8000
```

The ML service starts on:

```
http://localhost:8000
```

---

## Step 3: Generate Traffic (Optional but Recommended)

Open a third terminal:

```bash
k6 run test.js
```

This simulates concurrent user traffic by continuously calling the marketplace APIs.

---

## Verify Everything Is Running

> **Note:** The first metrics snapshot is generated approximately 5 seconds after Spring Boot starts. Until then, `/api/metrics` may return default values and `/latest` may not yet contain an inference.

### Check Spring Boot

Open:

```
http://localhost:8080/api/test
```

Expected response:

```
App is running
```

### Check ML Service

Open:

```
http://localhost:8000/health
```

Expected output:

- Service status
- Poll count
- Number of baseline samples collected
