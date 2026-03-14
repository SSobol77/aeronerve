<!-- 
All rights reserved.
© 2024–2026 Siergej Sobolewski
This is a commercial product. No part of this software may be copied, modified, distributed, or used without explicit written permission from the copyright holder.
-->

# AeroNerve 2.0 — Unified Intelligent Drone Operations Platform

> **A unified platform for intelligent drone operations.**  
> AeroNerve 2.0 combines GPS-free visual navigation, real-time control, and high-fidelity simulation.  
> It delivers the reliability, observability, and governance enterprises expect — indoors and outdoors.

[![Java](https://img.shields.io/badge/Java-21-blue.svg)](https://openjdk.org/projects/jdk/21/)
[![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.4-brightgreen.svg)](https://spring.io/projects/spring-boot)
[![Build](https://img.shields.io/badge/Build-Maven-red.svg)](https://maven.apache.org/)

---

## Table of Contents

1. [Project Overview](#1-project-overview)
2. [Key Features](#2-key-features)
3. [System Architecture Overview](#3-system-architecture-overview)
4. [Hardware Requirements](#4-hardware-requirements)
5. [Software Prerequisites](#5-software-prerequisites)
6. [Installation](#6-installation)
7. [Quick-Start Guide](#7-quick-start-guide)
   - 7.1 Indoor Operations
   - 7.2 Outdoor Operations
8. [Configuration Reference](#8-configuration-reference)
9. [API Reference](#9-api-reference)
10. [Development](#10-development)
11. [License](#11-license)

---

## 1. Project Overview

**AeroNerve 2.0** is an enterprise-grade drone command-and-control platform designed for organizations that need reliable, observable, and governable drone operations across both indoor (GPS-denied) and outdoor environments.

The platform is built around three pillars:

| Pillar | Description |
|---|---|
| 🛰️ **GPS-Free Visual Navigation** | Camera-based VIO and optical flow for indoor/GPS-denied environments |
| ⚡ **Real-Time Control** | STOMP/WebSocket-based command channel with sub-100ms latency target |
| 🎮 **High-Fidelity Simulation** | Physics-accurate drone simulation for pre-flight mission validation |

### Current Release: Command Core (Phase 1 MVP)

The current codebase implements the **Command Core** — the backend platform that manages device registration, authentication, telemetry ingestion, real-time status broadcasting, and a debug operations dashboard.

---

## 2. Key Features

### ✅ Implemented (v0.1 MVP)
- **Device Registry** — Create, manage, and authenticate drone devices via REST API
- **Token-Based Device Authentication** — Cryptographically secure 256-bit Bearer tokens for each device
- **Real-Time Telemetry** — STOMP/WebSocket telemetry ingestion (lat, lon, altitude, battery)
- **Live Status Dashboard** — Browser-based debug dashboard with real-time device status
- **Operator Authentication** — BCrypt-hashed credentials with Spring Security session management
- **Structured Logging** — Daily-rotating file logs with Logback + console output
- **Docker Deployment** — Multi-stage Dockerfile + Docker Compose for local development

### 🔜 Planned (v0.2 — v1.0)
- GPS-free Visual Inertial Odometry (VIO) pipeline
- Redis-backed distributed state (horizontal scaling)
- Command dispatch channel (drone actuation)
- Mission planning API + waypoint management
- Prometheus metrics + Grafana dashboards
- OpenTelemetry distributed tracing
- High-fidelity Babylon.js/Gazebo simulation layer
- Enterprise RBAC and audit trail

---

## 3. System Architecture Overview

```
┌──────────────────────────────────────────────────────────────────┐
│                        AeroNerve 2.0                             │
│                                                                  │
│  ┌─────────────┐    ┌──────────────┐    ┌────────────────────┐  │
│  │  Drone Fleet │    │  Web Browser │    │  External Systems  │  │
│  │  (devices)  │    │  (operators) │    │  (GIS, Weather)    │  │
│  └──────┬──────┘    └──────┬───────┘    └────────┬───────────┘  │
│         │                  │                     │              │
│  ┌──────▼──────────────────▼─────────────────────▼───────────┐  │
│  │              Nginx (TLS Termination + Load Balancer)       │  │
│  └──────────────────────────┬──────────────────────────────── ┘  │
│                             │                                    │
│  ┌──────────────────────────▼──────────────────────────────── ┐  │
│  │       AeroNerve Command Core (Spring Boot 3.4 / Java 21)   │  │
│  │  ┌────────────┐  ┌──────────────┐  ┌──────────────────┐   │  │
│  │  │  Security  │  │  WebSocket   │  │   REST API        │   │  │
│  │  │  Filter    │  │  STOMP Layer │  │   Controllers     │   │  │
│  │  └────────────┘  └──────────────┘  └──────────────────┘   │  │
│  │  ┌────────────────────────────────────────────────────┐   │  │
│  │  │              Service Layer                          │   │  │
│  │  │  DeviceStateService │ DeviceTokenService           │   │  │
│  │  └────────────────────────────────────────────────────┘   │  │
│  └──────────────────┬──────────────────────┬─────────────────┘  │
│                     │                      │                     │
│  ┌──────────────────▼──────┐  ┌────────────▼─────────────────┐  │
│  │  PostgreSQL 14           │  │  Redis 6                     │  │
│  │  (Device Registry, Users)│  │  (Live State, Telemetry)     │  │
│  └─────────────────────────┘  └──────────────────────────────┘  │
└──────────────────────────────────────────────────────────────────┘
```

For the full architecture diagram and component descriptions, see [ARCHITECTURE.md](./ARCHITECTURE.md).

---

## 4. Hardware Requirements

### Minimum (Development / Simulation)

| Component | Minimum | Recommended |
|---|---|---|
| CPU | 2-core x86_64 | 4-core x86_64 |
| RAM | 4 GB | 8 GB |
| Disk | 10 GB SSD | 20 GB SSD |
| Network | 100 Mbps LAN | 1 Gbps LAN |
| OS | Ubuntu 22.04 LTS | Ubuntu 24.04 LTS |
| Java | JDK 21 | JDK 21 (Temurin) |
| Docker | 24.x | Latest stable |

### Supported Drone Hardware (Integration-Ready)

| Hardware Class | Interface | Notes |
|---|---|---|
| Multirotor (Agri sprayer) | WebSocket / STOMP | Primary target (`agro-drone-01` profile) |
| Fixed-wing survey | WebSocket / STOMP | Planned Phase 2 |
| Indoor micro-drone | WebSocket / STOMP + VIO | Planned Phase 3 (GPS-free) |
| ESP32/ESP8266 MCU drones | WebSocket raw | Python simulator compatible |

### Network Requirements

| Environment | Protocol | Port | Notes |
|---|---|---|---|
| Indoor (local) | WS `ws://` | 8080 | Dev mode |
| Outdoor (production) | WSS `wss://` | 443 | Nginx TLS termination |
| Telemetry stream | STOMP over WS | same | `/app/telemetry` destination |
| Status stream | STOMP over WS | same | `/app/status` destination |

---

## 5. Software Prerequisites

| Prerequisite | Version | Install |
|---|---|---|
| Java JDK | 21 (Temurin) | `sudo apt install temurin-21-jdk` |
| Maven | 3.9+ | Bundled (`./mvnw`) |
| Docker | 24+ | [docs.docker.com](https://docs.docker.com/engine/install/) |
| Docker Compose | 2.x | Bundled with Docker Desktop |
| Python | 3.12 | For simulators only |
| Git | 2.x | For source checkout |

---

## 6. Installation

### 6.1 Clone the Repository

```bash
git clone https://github.com/SSobol77/aeronerve-cc.git
cd aeronerve-cc
```

### 6.2 Configure Environment Variables

> ⚠️ **Never commit real credentials to source control.**

Copy the example configuration and fill in your values:

```bash
cp src/main/resources/application.properties.example \
   src/main/resources/application-local.properties
```

Minimum required environment variables:

```bash
export DB_HOST=localhost
export DB_PORT=5432
export DB_NAME=aeronerve_db
export DB_USERNAME=aeronerve_user
export DB_PASSWORD=<your-secure-password>
export REDIS_HOST=localhost
export REDIS_PORT=6379
export ALLOWED_ORIGINS=http://localhost:8080
```

### 6.3 Start Infrastructure Services

```bash
# Start PostgreSQL and Redis via Docker Compose
docker compose up -d

# Verify services are healthy
docker compose ps
```

Expected output:
```
NAME                   STATUS
aeronerve_postgres     Up (healthy)
aeronerve_redis        Up
```

### 6.4 Build the Application

```bash
# Using Maven wrapper (no local Maven required)
./mvnw clean package

# Build output
ls -la target/aeronerve-0.0.1-SNAPSHOT.jar
```

### 6.5 Run the Application

```bash
# Development mode (uses application.properties)
./mvnw spring-boot:run

# Production mode (specify profile)
java -jar target/aeronerve-0.0.1-SNAPSHOT.jar \
  --spring.profiles.active=prod \
  --spring.datasource.password="${DB_PASSWORD}"
```

Application will start at `http://localhost:8080`.

### 6.6 Full Docker Build (Optional)

```bash
# Build Docker image
docker build -t aeronerve:latest .

# Run with environment variables
docker run -p 8080:8080 \
  -e DB_PASSWORD="${DB_PASSWORD}" \
  -e SPRING_DATASOURCE_URL="jdbc:postgresql://postgres:5432/aeronerve_db" \
  --network aeronerve_default \
  aeronerve:latest
```

---

## 7. Quick-Start Guide

### 7.1 Indoor Operations (GPS-Denied Environment)

Indoor mode uses visual navigation. In the current MVP, GPS coordinates are still accepted via telemetry. Full GPS-free VIO will be available in Phase 3.

**Step 1: Register a new indoor drone device**
```bash
curl -u admin:admin123 \
  -X POST http://localhost:8080/api/devices \
  -H "Content-Type: application/json" \
  -d '{"name": "indoor-drone-01", "type": "Micro Indoor", "description": "Warehouse inspection drone"}'
```

Response:
```json
{
  "id": "1",
  "name": "indoor-drone-01",
  "accessToken": "xK9mN2pQ...",
  "status": "offline"
}
```

**Step 2: Store the access token on your drone firmware**
```bash
export AN_DEVICE_TOKEN="xK9mN2pQ..."
export AN_DEVICE_NAME="indoor-drone-01"
```

**Step 3: Connect the drone simulator (or firmware)**
```bash
AN_DEVICE_TOKEN="${AN_DEVICE_TOKEN}" \
AN_DEVICE_NAME="${AN_DEVICE_NAME}" \
  python3 tools/drone_simulator.py
```

**Step 4: Open the dashboard**  
Navigate to `http://localhost:8080` — log in with `admin` / `admin123` and observe real-time telemetry.

---

### 7.2 Outdoor Operations

Outdoor mode uses standard GPS telemetry. The drone must send valid lat/lon/altitude/battery fields.

**Step 1: Register an outdoor drone**
```bash
curl -u admin:admin123 \
  -X POST http://localhost:8080/api/devices \
  -H "Content-Type: application/json" \
  -d '{"name": "agro-drone-01", "type": "Multirotor Sprayer", "description": "Agricultural field sprayer"}'
```

**Step 2: Configure the drone to send telemetry**  
The drone must connect to `ws://<server>:8080/ws` with STOMP and send to `/app/telemetry`:

```json
{
  "lat": 51.507351,
  "lon": -0.127758,
  "altitude": 35.5,
  "battery": 87.3,
  "speed": 12.4,
  "heading": 270
}
```

**Step 3: Retrieve the device token when needed**
```bash
curl -u admin:admin123 \
  http://localhost:8080/api/devices/agro-drone-01/token
```

**Step 4: Monitor via dashboard or integrate with your GIS**  
Dashboard: `http://localhost:8080`  
WebSocket API: `ws://localhost:8080/ws` → subscribe to `/topic/telemetry.agro-drone-01`

---

## 8. Configuration Reference

| Property | Default | Description |
|---|---|---|
| `server.port` | `8080` | HTTP/WS server port |
| `spring.datasource.url` | `jdbc:postgresql://localhost:5432/aeronerve_db` | PostgreSQL JDBC URL |
| `spring.datasource.username` | `aeronerve_user` | DB username |
| `spring.datasource.password` | *(set via env)* | DB password |
| `spring.redis.host` | `localhost` | Redis hostname |
| `spring.redis.port` | `6379` | Redis port |
| `spring.jpa.hibernate.ddl-auto` | `update` | Schema management (use `validate` in prod) |
| `spring.jpa.show-sql` | `false` | SQL debug logging (set `false` in prod) |
| `spring.thymeleaf.cache` | `true` | Template caching (set `true` in prod) |

---

## 9. API Reference

### Authentication
All API endpoints require HTTP Basic authentication (operator) or Bearer token (device).

### Device Management

| Method | Endpoint | Auth | Description |
|---|---|---|---|
| `POST` | `/api/devices` | Basic (ADMIN) | Create a new device and issue token |
| `GET` | `/api/devices/{name}/token` | Basic (ADMIN) | Retrieve token for a device |

### WebSocket / STOMP

| Direction | Destination | Description |
|---|---|---|
| Device → Server | `/app/telemetry` | Send telemetry data |
| Device → Server | `/app/status` | Send status update (`Online`/`Offline`) |
| Server → Client | `/topic/status` | Broadcast all device statuses |
| Server → Client | `/topic/telemetry.{deviceName}` | Per-device telemetry stream |

### Dashboard

| Method | Endpoint | Auth | Description |
|---|---|---|---|
| `GET` | `/` | Session | Main operator dashboard |
| `GET` | `/login` | Public | Login page |
| `POST` | `/logout` | Session | Logout |

---

## 10. Development

### Running the Python Simulator

```bash
# Install dependencies
cd tools
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt

# Run the STOMP simulator
AN_DEVICE_TOKEN="<token>" python3 drone_simulator.py

# Run the WebSocket connectivity test
python3 websocket_test.py
```

### Running Tests

```bash
./mvnw test
```

### Code Style

- Java: Google Java Style Guide
- Python: PEP 8
- Commit messages: Conventional Commits (`feat:`, `fix:`, `chore:`)

---

## 12 ⚠️ License & Copyright

© 2024–2026 Siergej Sobolewski. All rights reserved.

This is a **proprietary commercial product**. No part of this software may be 
copied, modified, distributed, or used without explicit written permission 
from the copyright holder. See [LICENSE](LICENSE) for details.
