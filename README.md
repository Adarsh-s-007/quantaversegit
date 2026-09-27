# QuantaVerse

An AI-powered interactive quantum computing learning platform that brings together structured learning, quantum circuit development, simulation, visualization, AI tutoring, assessment, and progress tracking in one environment.

QuantaVerse is designed to help students, researchers, and professionals move from learning quantum concepts to actually building, simulating, debugging, and evaluating quantum circuits.

## What it includes

- **Structured Quantum Learning** — Modules covering quantum computing fundamentals, circuits, and quantum algorithms.
- **Interactive Circuit Sandbox** — Build circuits visually or through code and execute them in the browser.
- **Multi-Framework Quantum Simulation** — Supports **Qiskit Aer, Cirq, PennyLane, and qBraid** through a **Canonical Circuit IR** and framework-specific adapters/transpilers.
- **Quantum Visualization** — Visualize circuits, quantum states, probabilities, measurements, and execution results.
- **AI Quantum Tutor** — Uses **GPT-OSS-120B via Groq** with RAG to provide context-aware explanations, debugging assistance, optimization suggestions, and personalized guidance.
- **Quantum-Native Assessment** — Evaluates submitted circuits using quantum-specific verification such as **state fidelity and unitary equivalence**.
- **Coding Challenges & Assessments** — Interactive exercises, coding challenges, scores, and achievements.
- **Learner Dashboard** — Tracks learning progress, completed lessons, exercises, and performance.
- **Instructor Features** — Module management, student progress tracking, assignments, and instructor dashboards.
- **Research & Collaboration** — Circuit sharing, verified portfolios, mentor discovery, and academic connections.
- ## Tech Stack

- **Frontend:** Next.js, React, TypeScript, Tailwind CSS, Monaco Editor, Three.js / React Three Fiber
- **Backend:** Python, FastAPI, Pydantic, SQLAlchemy
- **AI:** GPT-OSS-120B, Groq, RAG, Vector Retrieval
- **Quantum:** Qiskit Aer, Cirq, PennyLane, qBraid, Canonical Circuit IR, Framework Adapters/Transpilers
- **Database:** PostgreSQL / SQLite
- **Deployment:** Vercel, Render, Docker
- ### Prerequisites

- Node.js 20+
- Python 3.11 or 3.12
- Git

## AI Reasoning

The tutor is designed around a context-aware reasoning loop rather than a standalone chatbot

```text
Learner Context
      +
Circuit / Code
      +
Quantum State & Results
      ↓
RAG Knowledge Retrieval
      ↓
GPT-OSS-120B
      ↓
Quantum-Aware Guidance
      ↓
Explain • Debug • Optimize • Personalize






