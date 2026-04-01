<!--
File: README.md
Project: AeroNerve 2.0

All rights reserved.
© 2024–2026 Siergej Sobolewski
Commercial enterprise software – no part may be copied, modified, distributed or used without explicit written permission.
-->

<div align="center">

# AeroNerve 2.0 — AN-CC-02

**Enterprise drone command & control platform**

AeroNerve 2.0 is an integrated enterprise platform for intelligent drone
operations, combining GPS-free visual navigation, real-time command & control,
high-fidelity simulation, and high-assurance safety architecture.

It is designed for organisations that require operational reliability,
deterministic control, auditability, and a product architecture prepared for
regulated and mission-critical deployment.

<p align="center">
  <img src="[img/an_cc2.jpg](https://aeronerve.co/images/logo_dr1.png)" alt="AeroNerve 2.0 – Command & Control Core" width="720">
  <br><em>AN-CC-02 — current commercial product baseline</em>
</p>

[![Product](https://img.shields.io/badge/Product-AeroNerve%202.0-blueviolet)](docs/ARCHITECTURE.md)
[![Java](https://img.shields.io/badge/Java-21-orange)](https://openjdk.org/projects/jdk/21/)
[![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.4.7-brightgreen)](https://spring.io/projects/spring-boot)
[![License](https://img.shields.io/badge/License-Commercial-red)](LICENSE)

</div>

## Why AeroNerve?

Most drone platforms are built around isolated use cases or fragmented tooling.
AeroNerve 2.0 is built as a **full operational command core** for enterprise
and institutional drone fleets.

The platform is intended for environments where operators need:

- Reliable operation in GPS-denied and mixed environments
- Deterministic command & control with auditable outcomes
- Strong observability and operational evidence
- High-fidelity simulation aligned with production contracts
- Security-first trust boundaries for operators, devices, and runtime flows
- A technical baseline suitable for assurance, certification, and customer-specific deployment programmes

## What AeroNerve delivers

| Area | Product capability |
|------|--------------------|
| Runtime baseline | Java 21, Spring Boot 3.4.7, Spring MVC, Tomcat, WebSocket/STOMP |
| Navigation core | GPS-free telemetry architecture, VIO integration path, MAVLink optical-flow bridge |
| Real-time control | Command routing, ACK lifecycle, idempotent control flow, Redis-backed dispatch |
| Observability | Prometheus metrics, OpenTelemetry tracing, immutable audit log |
| Governance | RBAC, evidence-pack export, SBOM-backed release governance |
| Simulation | Gazebo SITL integration with production-aligned contracts |
| Device trust | Device credential lifecycle, mTLS-ready trust architecture, PKI-driven expansion path |
| Supply chain | CycloneDX SBOM, signed artifacts, trusted release-chain baseline |
| Safety architecture | Built-in Ada/SPARK bounded high-assurance safety kernel path |
| Deployment model | Enterprise-ready backend architecture for customer, partner, and regulated deployment scenarios |

## Product architecture at a glance

- **Runtime** → Java 21 + Spring Boot 3.4.7, Spring MVC + embedded Tomcat
- **Messaging** → Spring WebSocket + STOMP
- **Operator authentication** → JWT Bearer with controlled session lifecycle
- **Device trust** → hashed credentials, revocation / expiry enforcement, mTLS architecture path
- **Telemetry** → locked VIO contract and production-compatible simulator telemetry path
- **State & routing** → Redis for command routing, ACK locking, and event propagation
- **Persistence** → PostgreSQL 14 + Spring Data JPA + Flyway
- **Observability** → Micrometer Prometheus + tracing + append-only audit
- **Simulation** → high-fidelity simulator subsystem with scenario, replay, and synchronised event model
- **Safety boundary** → bounded Ada/SPARK safety kernel integrated behind the Java orchestration boundary
- **Transport security** → Nginx reverse-proxy TLS termination for production deployments

Detailed architecture → [`docs/ARCHITECTURE.md`](docs/ARCHITECTURE.md)  
Code structure → [`docs/structure.md`](docs/structure.md)

## Safety and assurance posture

AeroNerve 2.0 is built with a strong separation between orchestration logic,
runtime control, simulator subsystem behaviour, and bounded safety enforcement.

The product architecture includes:

- Deterministic command lifecycle and audit correlation
- Immutable operational evidence surfaces
- Production-compatible simulator contracts
- A bounded Ada/SPARK high-assurance safety kernel for command admissibility and hard-boundary validation
- SBOM-backed release governance and trusted supply-chain baseline
- Security controls for operator identity, device trust, and session handling

This architecture is designed to support customer deployment programmes,
assurance work, and certification-oriented engagement without exposing internal
engineering workflows in the public product surface.

## Simulation as a first-class product capability

AeroNerve does not treat simulation as a demo-only side feature.

The simulator subsystem is a first-class product capability used to support:

- operational validation,
- scenario-driven mission rehearsal,
- replay and determinism checks,
- operator training flows,
- controlled data-generation and evaluation workflows.

The simulator architecture is aligned with production telemetry and control
contracts, reducing divergence between simulated behaviour and operational
deployment paths.

## Security and governance

| Area | Product posture |
|------|-----------------|
| Secrets | environment-based secret handling |
| TLS | production deployment through reverse-proxy TLS termination |
| Operator identity | JWT-based operator authentication with controlled session lifecycle |
| Device trust | lifecycle-managed device credentials with revocation and expiry enforcement |
| RBAC | role-separated operator access model |
| Audit | immutable append-only audit evidence |
| Observability | metrics, traces, and incident-response visibility |
| SBOM | CycloneDX-based software bill of materials |
| Release assurance | signed artifact and trusted build-chain baseline |

## Documentation

Core product documentation:

- Architecture → [`docs/ARCHITECTURE.md`](docs/ARCHITECTURE.md)
- Roadmap → [`docs/ROADMAP.md`](docs/ROADMAP.md)
- Audit baseline → [`docs/AUDIT_REPORT.md`](docs/AUDIT_REPORT.md)
- Engineering task register → [`docs/ENGINEERING_TASKS.md`](docs/ENGINEERING_TASKS.md)

Contract location:

- Versioned product and simulator contracts → [`docs/contracts/`](docs/contracts/)

## Commercial positioning

AeroNerve 2.0 is intended for:

- enterprise drone fleet operators,
- industrial and infrastructure operators,
- public-sector and regulated-environment deployments,
- partners building advanced drone capabilities on top of a governed command core,
- investors and sponsors interested in high-integrity drone platform infrastructure.

## Access and licensing

This repository represents a commercial product baseline.

- Source code and implementation rights are not granted through public repository visibility
- No commercial, derivative, or redistribution rights are granted without explicit written permission
- Product access, pilot deployments, customer integrations, and strategic partnerships are handled directly

## Contact & demo

→ **www.aeronerve.co**  
→ **contact@aeronerve.co**

---

**AeroNerve 2.0** — enterprise drone command core  
Built for reliable operations, high-assurance control, and governed deployment.

© 2024–2026 Siergej Sobolewski — commercial product — all rights reserved.
