# Project Sanri 🛡️

**Next-Generation Multi-Layered Cyber-Deception Framework**

Sanri is an advanced, multi-layered cyber-deception platform (honeypot architecture) engineered to detect, confuse, and analyze malicious actors in real time. Unlike traditional static honeypots, Sanri provides a high-interaction, dynamic deception surface powered by Rust, Python, and AI.

## 🚀 Key Features

*   **Deep Packet Inspection (DPI) & Heuristic Routing:** Written in Rust (`core_proxy`), the gateway analyzes structural byte sequences at Layer 7 to automatically categorize threats (SSH, HTTP, MySQL, TLS) without breaking the protocol handshake.
*   **Tarpit & Artificial Latency Injection:** Deliberately injects asynchronous delays (e.g., 1.5s for SSH, 2.5s for DB) during connection establishment. This severely slows down automated scanners (Nmap, Masscan) and buys time for dynamic containers.
*   **Dynamic Orchestration (Docker Sandbox):** Spawns fresh, isolated high-interaction Docker containers on-demand upon connection detection. It minimizes baseline resource utilization and ensures attackers are trapped in secure, capped environments.
*   **Generative Web Interfaces (LLM Engine):** Uses an asynchronous Python/FastAPI microservice integrated with Gemini AI to generate contextually relevant, realistic web interfaces (e.g., admin panels) on the fly, prolonging attacker engagement.
*   **Automated Rule Synthesis (Z3 SMT Solver):** Analyzes recorded exploit payloads (e.g., buffer overflows) using the Z3 Theorem Prover. It mathematically verifies memory violations and autonomously synthesizes production-ready **YARA** and **Snort/Suricata** signatures for 0-day exploits.
*   **Sarmal P2P Distributed Defense Layer:** Built on a peer-to-peer mesh network, independent Sanri nodes exchange synthesized threat telemetry instantly.
*   **Advanced Gossip Protocol & SID Tracking:** Sarmal utilizes a Gossip Protocol with Signature ID (SID) tracking. It silently drops previously seen packets to prevent infinite routing loops while broadcasting novel 0-day signatures to peer nodes in milliseconds.

## 🏗️ Architecture

Sanri relies on a decoupled, polyglot microservices approach communicating via lightweight TCP/IPC:

1.  **Ingress:** `core_proxy` (Rust) handles DPI and Tarpitting.
2.  **Deception:** Traffic is routed to either the LLM Engine (Python/FastAPI) for HTTP or isolated Docker Sandboxes for protocols like SSH/DB.
3.  **Analysis:** Telemetry from sandboxes is sent to the Z3 SMT Engine (Python/FastAPI) for formal verification.
4.  **Distribution:** Generated YARA/Snort rules are gossiped across the Sarmal P2P Mesh.

## 🛠️ Technology Stack

*   **Network/Proxying:** Rust, TCP/IPC
*   **Microservices/AI:** Python, FastAPI, Google Gemini API
*   **Formal Verification:** Z3 Theorem Prover (SMT Solver)
*   **Isolation:** Docker Engine API, Linux Namespaces/cgroups

## 🚀 Deployment

Sanri is designed for rapid deployment via Docker Compose. The host system exposes only the `core_proxy` ports, completely segregating the LLM pipeline, Z3 engine, and Sarmal P2P control plane from public access.
