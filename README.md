# Secure PKI & TOTP Microservice

![Python](https://img.shields.io/badge/Python-3.11-blue?style=for-the-badge&logo=python&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-0.95+-009688?style=for-the-badge&logo=fastapi&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-Container-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Security](https://img.shields.io/badge/RSA-4096-red?style=for-the-badge)

## Project Overview

This project is a secure, containerized microservice designed to demonstrate **Public Key Infrastructure (PKI)** and **Time-based Two-Factor Authentication (2FA)**. 

It solves the challenge of securely transmitting authentication seeds over an insecure network by using asymmetric RSA encryption. The service is built with **FastAPI**, containerized with **Docker**, and includes automated background auditing via **Cron**.

### Key Features
* **Secure Seed Exchange:** Uses RSA-OAEP (4096-bit) to decrypt seeds received from an external authority.
* **TOTP Generation:** Generates standard 6-digit, 30-second time-based codes (RFC 6238).
* **Drift Tolerance:** Verifies codes with a ±1 period window (30 seconds) to account for network latency.
* **Persistence:** Docker volumes ensure the seed and audit logs survive container restarts.
* **Automated Auditing:** A self-healing Cron job logs a valid 2FA code every minute for verification.

---

## Technology Stack

* **Language:** Python 3.11
* **Framework:** FastAPI + Uvicorn
* **Containerization:** Docker & Docker Compose
* **Cryptography:** `cryptography` (RSA-PSS, RSA-OAEP), `pyotp`
* **Orchestration:** Docker Compose (Multi-stage builds)
* **Scheduling:** Linux Cron (System V)

---

## Project Structure

```text
pki-totp-microservice/
├── app/
│   ├── main.py              # FastAPI application entry point
│   ├── models.py            # Pydantic data models for validation
│   ├── crypto_utils.py      # RSA decryption logic (OAEP + SHA256)
│   └── totp_utils.py        # TOTP generation & verification logic
├── cron/
│   └── 2fa-cron             # Cron job definition
├── scripts/
│   ├── generate_keys.py     # Generates RSA Keypair (Student Keys)
│   ├── request_seed.py      # Requests encrypted seed from Instructor API
│   ├── generate_proof.py    # Generates Cryptographic Commit Proof
│   └── log_2fa_cron.py      # Script executed by Cron inside container
├── Dockerfile               # Multi-stage production build
├── docker-compose.yml       # Service definition with volumes
├── requirements.txt         # Project dependencies
└── README.md                # Documentation
