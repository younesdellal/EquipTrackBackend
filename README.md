# 🛰️ ErcTrack — Backend API

### 🔐 Secure REST API for IoT-Based Telecom Equipment Tracking & Mission Management

![Node.js](https://img.shields.io/badge/Node.js-20-339933?style=for-the-badge&logo=node.js&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-5.x-3178C6?style=for-the-badge&logo=typescript&logoColor=white)
![Express.js](https://img.shields.io/badge/Express.js-REST_API-000000?style=for-the-badge&logo=express&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-16-4169E1?style=for-the-badge&logo=postgresql&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-Cache-DC382D?style=for-the-badge&logo=redis&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-Ready-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![MQTT](https://img.shields.io/badge/MQTT-HiveMQ-660066?style=for-the-badge&logo=mqtt&logoColor=white)
![Socket.IO](https://img.shields.io/badge/Socket.IO-Real--time-010101?style=for-the-badge&logo=socket.io&logoColor=white)
![OpenAPI](https://img.shields.io/badge/OpenAPI-3.0-6BA539?style=for-the-badge&logo=openapiinitiative&logoColor=white)
![License](https://img.shields.io/badge/License-Academic_Project-yellow?style=for-the-badge)

> **Role:** Backend Developer  
> **Project:** ErcTrack — IoT-Based Telecom Equipment Tracking and Management Platform  
> **Academic Year:** 2025–2026  
> **University:** USTHB — University of Science and Technology Houari Boumediene, Algeria  
> **Specialty:** Information Systems and Software Engineering — ISIL

---

## 📌 Table of Contents

- [Overview](#-overview)
- [Key Features](#-key-features)
- [System Architecture](#-system-architecture)
- [Technology Stack](#-technology-stack)
- [Backend Responsibilities](#-backend-responsibilities)
- [Getting Started](#-getting-started)
- [Environment Variables](#-environment-variables)
- [API Documentation](#-api-documentation)
- [Main API Endpoints](#-main-api-endpoints)
- [Security](#-security)
- [Real-Time GPS Tracking](#-real-time-gps-tracking)
- [Testing the Backend](#-testing-the-backend)
- [Docker](#-docker)
- [Project Structure](#-project-structure)
- [Limitations](#-limitations)
- [Future Improvements](#-future-improvements)
- [Author](#-author)

---

## 🧭 Overview

**ErcTrack** is a multi-platform IoT system designed to digitize and automate telecom equipment delivery operations.

The platform connects:

- 🚚 **Drivers**, who deliver telecom equipment to operational sites.
- 🧑‍🔧 **Technicians**, who receive equipment and submit field reports.
- 🧑‍💼 **Administrators**, who manage missions, users, equipment, sites, and live tracking.
- 📦 **IoT GPS trackers**, which provide near real-time equipment location.
- 🤖 **AI validation service**, which verifies technician-uploaded images.

The backend is the central component of the system. It exposes a secure REST API used by the React Native mobile application, the React admin dashboard, the Python MQTT bridge, and the Python AI validation service.

---

## ✨ Key Features

| Feature | Description |
|---|---|
| 🔐 Authentication | JWT-based login with role-based access control |
| 👥 Roles | Admin, Technician, Driver |
| 📦 Mission Management | Create, update, delete, list, and import missions |
| 📥 JSON Mission Import | Import missions from structured JSON files |
| 📱 QR Delivery Confirmation | Dual QR scan workflow between driver and technician |
| 📍 Real-Time GPS Tracking | GPS data received via MQTT and pushed with Socket.IO |
| 🧠 AI Image Validation | Integration with Python CLIP/OpenCV validation service |
| ☁️ Image Storage | Technician photos stored via Cloudinary |
| 🔔 Push Notifications | Firebase Cloud Messaging token management |
| 📚 API Documentation | OpenAPI 3.0 / Swagger available at `/api/docs` |
| 🐳 Docker Support | PostgreSQL and Redis run using Docker Compose |
| 🧾 Immutable Delivery Logs | Delivery confirmation records are append-only |

---

## 🏗️ System Architecture

```text
┌─────────────────────┐         ┌─────────────────────┐
│  React Native App   │         │  React Admin Web    │
│  Driver/Technician  │         │  Dashboard          │
└─────────┬───────────┘         └─────────┬───────────┘
          │                               │
          │ HTTPS / JWT                   │ HTTPS / JWT
          ▼                               ▼
┌─────────────────────────────────────────────────────┐
│                 ErcTrack Backend API                │
│                                                     │
│  Node.js + TypeScript + Express.js                  │
│  JWT Auth | RBAC | REST API | Socket.IO             │
│  PostgreSQL | Sequelize ORM | Redis | Docker        │
└──────┬──────────────┬──────────────┬───────────────┘
       │              │              │
       ▼              ▼              ▼
┌────────────┐ ┌────────────┐ ┌────────────────┐
│ PostgreSQL │ │ Redis      │ │ AI Validation  │
│ Database   │ │ Cache      │ │ Python Service │
└────────────┘ └────────────┘ └────────────────┘

┌─────────────────────┐
│ ESP32 GPS Tracker   │
│ NEO-6M + SIM800L    │
└─────────┬───────────┘
          │ MQTT over TLS
          ▼
┌─────────────────────┐
│ HiveMQ Cloud Broker │
└─────────┬───────────┘
          │ gps/+
          ▼
┌─────────────────────┐
│ Python MQTT Bridge  │
└─────────┬───────────┘
          │ POST /api/gps/ingest
          ▼
┌─────────────────────┐
│ ErcTrack Backend    │
│ Socket.IO update    │
└─────────────────────┘
```

---

## 🧰 Technology Stack

| Layer | Technology | Why It Was Used |
|---|---|---|
| Runtime | Node.js 20 | Non-blocking I/O, suitable for real-time APIs |
| Language | TypeScript | Static typing, safer backend development |
| Framework | Express.js | Lightweight and modular REST API framework |
| Database | PostgreSQL 16 | Strong relational integrity and UUID support |
| ORM | Sequelize | Database model management and migrations |
| Authentication | JWT | Stateless authentication for API security |
| Password Hashing | bcrypt | Secure password storage with cost factor 12 |
| Real-Time | Socket.IO | Live GPS updates without polling |
| API Docs | OpenAPI 3.0 / Swagger | Automatic API documentation |
| Containerization | Docker Compose | Consistent PostgreSQL + Redis environment |
| Cache / Infra | Redis | Development support and future caching usage |
| IoT Protocol | MQTT | Lightweight publish/subscribe protocol |
| MQTT Broker | HiveMQ Cloud | Managed broker with TLS and access control |
| GPS Simulation | Python + gpxpy | Realistic GPS route simulation |
| AI Integration | Python + CLIP + OpenCV | Image quality and equipment relevance validation |
| Image Storage | Cloudinary | Cloud media storage and optimization |
| Push Notifications | Firebase Cloud Messaging | Reliable cross-platform notifications |

---

## 🧑‍💻 Backend Responsibilities

As the backend developer, my work focused on designing and implementing the core API used by the mobile application, web dashboard, IoT bridge, and AI module.

### 🔐 Authentication and Authorization

- User login with email and password.
- JWT token generation with a seven-day expiry.
- Role-based access control for Admin, Technician, and Driver.
- Authentication middleware to protect API routes.
- Authorization middleware to enforce role permissions.
- Password hashing using bcrypt with cost factor 12.
- Rate limiting on `/auth/login`: 10 requests per 15 minutes per IP.

### 📦 Mission Management

- Full CRUD operations for missions.
- Mission assignment to drivers and technicians.
- Equipment and site management.
- Mission creation through JSON file import.
- Validation of imported mission structure.
- Notification triggering after mission assignment.

### 📥 QR Code Delivery Workflow

The delivery process uses a secure two-scan workflow:

1. The driver scans the package QR code to start the delivery.
2. The technician scans the same QR code after receiving the package.
3. The backend validates the scanning order.
4. The delivery status is updated.
5. The delivery confirmation record is stored immutably.

Delivery status flow:

```text
PENDING
  ↓
DRIVER_SCANNED
  ↓
DELIVERED
```

If the technician scans before the driver, the backend returns:

```json
{
  "message": "Awaiting driver scan"
}
```

HTTP status:

```text
409 Conflict
```

### 📍 GPS Ingestion and Real-Time Tracking

- GPS data is published to MQTT topic `gps/+`.
- A Python MQTT subscriber validates the payload.
- The bridge sends the GPS data to `/api/gps/ingest`.
- The backend stores the GPS log in PostgreSQL.
- The backend emits a Socket.IO event to connected clients.
- The admin dashboard updates the map in near real time.

Measured average GPS pipeline latency during testing:

```text
≈ 1.4 seconds
```

### 🤖 AI Image Validation Integration

When a technician uploads a photo:

1. The mobile app sends the image to the backend.
2. The backend forwards the image to the AI validation service.
3. The AI module checks blur, brightness, resolution, and equipment relevance.
4. If valid, the image is accepted and stored via Cloudinary.
5. If invalid, the technician is asked to retake the photo.

---

## 🚀 Getting Started

### ✅ Prerequisites

Make sure the following tools are installed:

- Node.js 20 or higher
- npm or yarn
- Docker
- Docker Compose
- Python 3.10 or higher, if using the GPS simulator
- MQTT Explorer, optional but useful for debugging MQTT topics

---

### 📦 Installation

Clone the repository:

```bash
git clone https://github.com/younesdellal/erctrack-backend.git
cd erctrack-backend
```

Install dependencies:

```bash
npm install
```

Create your environment file:

```bash
cp .env.example .env
```

Start PostgreSQL and Redis:

```bash
docker compose up -d
```

Run database migrations:

```bash
npm run migrate
```

Optionally, seed the database with test data:

```bash
npm run seed
```

Start the development server:

```bash
npm run dev
```

The API should be available at:

```text
http://localhost:3000
```

---

## 🔑 Environment Variables

Create a `.env` file based on `.env.example`.

Example:

```env
NODE_ENV=development
PORT=3000

DATABASE_URL=postgres://user:password@localhost:5432/erctrack

JWT_SECRET=your_jwt_secret
JWT_EXPIRES_IN=7d

MQTT_BROKER_URL=mqtt://your-broker-url
MQTT_USERNAME=your_mqtt_username
MQTT_PASSWORD=your_mqtt_password

AI_SERVICE_URL=http://localhost:8000
CLOUDINARY_URL=your_cloudinary_url
FIREBASE_SERVICE_ACCOUNT_PATH=./firebase-service-account.json

CLIENT_URL=http://localhost:5173
INTERNAL_GPS_TOKEN=your_internal_gps_token
```

> ⚠️ Never commit real secrets to GitHub. Use `.env.example` for public repositories.

---

## 📚 API Documentation

Once the server is running, Swagger documentation is available at:

```text
http://localhost:3000/api/docs
```

The API is documented using OpenAPI 3.0 annotations.

---

## 🧩 Main API Endpoints

### Authentication

| Method | Endpoint | Description |
|---|---|---|
| POST | `/api/auth/login` | Authenticate user and return JWT |
| POST | `/api/auth/register` | Create a new user, usually admin-only |

### Missions

| Method | Endpoint | Description |
|---|---|---|
| GET | `/api/missions` | List missions |
| GET | `/api/missions/:id` | Get mission details |
| POST | `/api/missions` | Create a mission |
| PUT | `/api/missions/:id` | Update a mission |
| DELETE | `/api/missions/:id` | Delete a mission |
| POST | `/api/missions/import` | Import missions from JSON file |

### Deliveries

| Method | Endpoint | Description |
|---|---|---|
| POST | `/api/deliveries/confirm` | Confirm delivery using QR code scan |

### GPS Ingestion

| Method | Endpoint | Description |
|---|---|---|
| POST | `/api/gps/ingest` | Internal endpoint used by the MQTT bridge |

### Users

| Method | Endpoint | Description |
|---|---|---|
| POST | `/api/users/push-token` | Store Firebase/Expo push token |

### Reports

| Method | Endpoint | Description |
|---|---|---|
| POST | `/api/reports` | Submit technician report with image validation |

---

## 🔒 Security

| Security Area | Implementation |
|---|---|
| Authentication | JWT Bearer token |
| Token Expiry | 7 days |
| Password Hashing | bcrypt, cost factor 12 |
| Authorization | Role-based access control |
| Login Protection | Rate limiting: 10 requests / 15 min / IP |
| Transport Security | TLS 1.2+ recommended for production |
| MQTT Security | TLS port 8883 with HiveMQ Cloud |
| Data Integrity | Immutable delivery confirmation records |
| Internal Endpoints | Token validation for GPS ingestion |

---

## 📡 Real-Time GPS Tracking

The backend uses Socket.IO to push GPS updates to the admin dashboard.

Namespace:

```text
/tracking
```

Event:

```text
gps:update
```

Example payload:

```json
{
  "deviceId": "ESP32_A3F7",
  "latitude": 36.7125,
  "longitude": 3.1853,
  "timestamp": "2026-05-20T10:30:00Z"
}
```

The admin dashboard subscribes to this event and updates the Leaflet map without polling.

---

## 🧪 Testing the Backend

### 1️⃣ Health Check

Check if the API is running:

```bash
curl http://localhost:3000/api/health
```

Expected response:

```json
{
  "status": "ok"
}
```

---

### 2️⃣ Login and Get JWT Token

```bash
curl -X POST http://localhost:3000/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "admin@erctrack.local",
    "password": "your_password"
  }'
```

Expected response:

```json
{
  "token": "jwt_token_here",
  "user": {
    "id": "uuid",
    "email": "admin@erctrack.local",
    "role": "admin"
  }
}
```

Save the token:

```bash
export TOKEN="jwt_token_here"
```

---

### 3️⃣ Access a Protected Route

```bash
curl http://localhost:3000/api/missions \
  -H "Authorization: Bearer $TOKEN"
```

---

### 4️⃣ Test QR Delivery Confirmation

Driver scan:

```bash
curl -X POST http://localhost:3000/api/deliveries/confirm \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $DRIVER_TOKEN" \
  -d '{
    "qrCode": "PACKAGE-12345",
    "role": "driver"
  }'
```

Expected status:

```text
DRIVER_SCANNED
```

Technician scan after driver:

```bash
curl -X POST http://localhost:3000/api/deliveries/confirm \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $TECHNICIAN_TOKEN" \
  -d '{
    "qrCode": "PACKAGE-12345",
    "role": "technician"
  }'
```

Expected status:

```text
DELIVERED
```

Technician scan before driver:

```bash
curl -X POST http://localhost:3000/api/deliveries/confirm \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $TECHNICIAN_TOKEN" \
  -d '{
    "qrCode": "PACKAGE-12345",
    "role": "technician"
  }'
```

Expected response:

```json
{
  "message": "Awaiting driver scan"
}
```

Expected HTTP status:

```text
409 Conflict
```

---

### 5️⃣ Test GPS Ingestion

The GPS ingestion endpoint is internal and should be called by the Python MQTT bridge.

Manual test:

```bash
curl -X POST http://localhost:3000/api/gps/ingest \
  -H "Content-Type: application/json" \
  -H "X-Internal-Token: your_internal_gps_token" \
  -d '{
    "deviceId": "ESP32_A3F7",
    "latitude": 36.7125,
    "longitude": 3.1853,
    "timestamp": "2026-05-20T10:30:00Z"
  }'
```

Expected behavior:

- GPS data is saved in PostgreSQL.
- Socket.IO emits a `gps:update` event.
- The admin dashboard updates the map position.

---

### 6️⃣ Test MQTT GPS Simulation

The physical ESP32 tracker was simulated for academic testing.

The simulation uses:

- Python
- gpxpy
- HiveMQ Cloud
- MQTT Explorer

Run the GPS simulator:

```bash
python scripts/gps_simulator.py
```

The simulator publishes GPS coordinates to the MQTT broker.

The Python MQTT bridge subscribes to:

```text
gps/+
```

It then forwards the data to:

```text
POST /api/gps/ingest
```

You can monitor MQTT messages using MQTT Explorer.

---

### 7️⃣ Test AI Image Validation

Submit a technician report with an image:

```bash
curl -X POST http://localhost:3000/api/reports \
  -H "Authorization: Bearer $TECHNICIAN_TOKEN" \
  -F "missionId=your_mission_uuid" \
  -F "image=@./test-images/equipment.jpg"
```

Expected AI response:

```json
{
  "validated": true,
  "qualityScore": 0.92,
  "equipmentDetected": true
}
```

If the image is blurry, too dark, too low-resolution, or does not contain telecom equipment, the AI service returns a rejection result.

---

## 🐳 Docker

The backend development environment uses Docker Compose to run PostgreSQL and Redis.

Start services:

```bash
docker compose up -d
```

Stop services:

```bash
docker compose down
```

If the backend is also containerized:

```bash
docker build -t erctrack-backend .
docker run -p 3000:3000 --env-file .env erctrack-backend
```

---

## 🗂️ Project Structure

```text
erctrack-backend/
├── src/
│   ├── config/
│   ├── controllers/
│   ├── middlewares/
│   ├── models/
│   ├── routes/
│   ├── services/
│   ├── sockets/
│   ├── utils/
│   └── app.ts
├── docs/
├── postman/
├── scripts/
├── docker-compose.yml
├── .env.example
├── package.json
└── README.md
```

> Adapt the folder structure to your real repository if needed.

---

## 📸 Screenshots

Add screenshots in a `docs/screenshots` folder and display them like this:

<div align="center">
  <img src="docs/screenshots/swagger.png" width="45%" alt="Swagger API Documentation" />
  <img src="docs/screenshots/postman.png" width="45%" alt="Postman API Testing" />
</div>

<div align="center">
  <img src="docs/screenshots/mqtt-explorer.png" width="45%" alt="MQTT Explorer" />
  <img src="docs/screenshots/socket-io.png" width="45%" alt="Real-Time GPS Update" />
</div>

---

## ⚠️ Limitations

- GPS tracking was simulated using Python and GPX files.
- Physical ESP32 + NEO-6M + SIM800L hardware was not fully deployed in production.
- AI validation was tested on an internal dataset.
- Load testing was performed under academic/lab conditions.

These limitations are part of the academic scope of the project and are described in the final-year project report.

---

## 🚀 Future Improvements

- Deploy physical ESP32 GPS trackers.
- Replace basic blur detection with a deep learning image quality model.
- Add offline-first mobile support using local SQLite storage.
- Add predictive analytics for mission planning.
- Improve observability with logging, monitoring, and tracing.
- Add automated integration tests.
- Deploy the backend to a cloud provider.

---

## 👤 Author

**Younes Dellal**  
Backend Developer — ErcTrack Project  
USTHB, Algeria

GitHub: [github.com/younesdellal](https://github.com/younesdellal)  
Email: younesdellal54@gmail.com

---

## 📄 License

This project is provided for academic demonstration purposes.
