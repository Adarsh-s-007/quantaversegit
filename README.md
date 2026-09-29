# QuantaVerse

**An AI-powered, interactive web platform for learning, designing, simulating and visualizing quantum algorithms.**
Theory, circuits, code, simulation, visualization, grading and an AI tutor, all in one browser tab.

| Smart India Hackathon 2026 | |
| --- | --- |
| Problem Statement ID | **SIH26140** |
| Title | AI-Based Interactive Quantum Algorithm Learning Platform |
| Theme | Smart Education |
| Category | Software |
| Team | **Bloch 'n roll_A0** (Team ID 177014) |

**Live prototype:** https://quantaverse-nu.vercel.app/
**Demo video:** https://www.youtube.com/watch?v=-_SifbaUqAA
**API reference:** [`backend/README.md`](backend/README.md)

---

## The problem

Quantum computing is hard to learn. Qubits, superposition, entanglement and measurement are abstract. Most resources are static and theory-heavy, with little hands-on practice. Access to real quantum hardware is limited. Learners also have to piece together separate tools: a textbook, a circuit composer, an SDK, a simulator and a forum.

## What QuantaVerse does

One platform that takes students, professors, researchers and working professionals from their first qubit to Shor's algorithm.

**Interactive circuit synthesis.** Learners build a circuit on a drag-and-drop canvas and edit its Qiskit code in an integrated Monaco editor. The two views stay in sync in both directions: change either one and the other follows.

**Real-time simulation and visualization.** A statevector simulator runs directly in the browser, so feedback is instant even when the API is offline. The platform shows statevectors, measurement probabilities and shot histograms, with 3D visuals built on Three.js.

**Interactive algorithms.** Standard quantum algorithms are shown step by step at the gate level, so learners can watch the state evolve through superposition, interference and measurement.

**Multi-engine quantum execution.** Every circuit is stored in one canonical circuit IR, which framework adapters translate for **Qiskit Aer**, **Cirq** and **PennyLane**. All three return byte-identical statevectors, and qubit ordering is verified with an asymmetric test circuit. A **qBraid** adapter shares the same interface and is currently a stub (see [Status](#status)).

**Context-aware AI tutor and debugger.** Every answer is grounded in the learner's actual circuit. The API rebuilds the circuit and computes its statevector, outcome probabilities and **per-qubit purity** before the model sees anything. A purity below 1 means that qubit is **entangled** with the rest of the register. Answers stream in real time from **GPT-OSS-120B via Groq**. If no model key is configured, the tutor falls back to a clearly labelled local read-out, so it never breaks. The tutor appears through a persistent conversational mascot.

**Quantum-native autograding.** Submissions are graded on physics rather than text matching. The grader checks **state fidelity** against the target, then **unitary equivalence up to global phase**, scored with process fidelity so a near miss shows as a number. Each result comes with a hint naming the first difference: an extra gate, a depth mismatch, or correct gates on the wrong wires.

**Guided learning path with personalized recommendations.** The course has **eight structured modules, from the qubit to Shor's algorithm**. Each module has lessons, a typeset theory PDF, a formula sheet and graded circuit labs. From each learner's own attempts, the platform works out mastery, skill axes and a personalized recommendation for what to do next. Finishing a module earns a badge.

**Instructor console.** Professors choose the modules they teach and accept or decline students who ask to join a class. For each module they see accepted students ranked by progress, and they can upload their own notes, which only their students can see.

**Academic and research hub.** Students discover professors and join their classes, which connects learners with mentors.

## Status

We label every feature honestly. "Stub" and "Planned" items are on the roadmap, not in the current build.

| Capability | Status |
| --- | --- |
| Drag-and-drop circuit builder ↔ Monaco code editor, bi-directional sync | ✅ Live |
| In-browser statevector simulator | ✅ Live |
| Qiskit Aer, Cirq, PennyLane execution through the canonical IR | ✅ Live |
| Autograder: state fidelity + unitary equivalence + hints | ✅ Live |
| AI tutor grounded in circuit state, streaming, offline fallback | ✅ Live |
| Eight-module curriculum, theory PDFs, graded labs, badges | ✅ Live |
| Progress, mastery, skill axes and next-step recommendations | ✅ Live |
| Professor console: classes, rankings, notes upload | ✅ Live |
| Student–professor hub | ✅ Live |
| qBraid adapter | 🟡 Stub: returns a uniform distribution, hidden from the engine picker |
| Live qBraid execution with credentials | 🔜 Planned |
| AI circuit/code generation from plain-language prompts, checked by the autograder before display | 🔜 Planned |
| Retrieval-augmented tutor over course material (PostgreSQL + pgvector) | 🔜 Planned |
| Dedicated researcher role | 🔜 Planned (current roles: student, professor) |

## Architecture

```mermaid
flowchart LR
  U["Students · Professors"] --> FE["Next.js frontend<br/>circuit builder · Monaco editor · 3D views"]
  FE -->|"instant run"| JS["In-browser statevector simulator"]
  FE -->|"HTTPS / JSON"| API["FastAPI"]
  API --> IR["Canonical circuit IR"]
  IR --> Q["Qiskit Aer"]
  IR --> C["Cirq"]
  IR --> P["PennyLane"]
  IR -.-> QB["qBraid (stub)"]
  API --> G["Autograder<br/>state fidelity · unitary equivalence"]
  API --> T["AI tutor<br/>GPT-OSS-120B via Groq"]
  API --> DB[("SQLite or PostgreSQL")]
```

In words: the Next.js site handles circuit building, code editing and 3D visualization, and runs small circuits in the browser. The FastAPI service converts circuits to a canonical IR, runs them on Qiskit Aer, Cirq or PennyLane, grades submissions, streams tutor answers and stores accounts and progress. The IR supports 26 gates, up to 12 qubits and 512 operations.

## Tech stack

**Frontend:** Next.js, React, TypeScript, Tailwind CSS, Three.js (React Three Fiber), Monaco Editor, Framer Motion
**Backend:** FastAPI, SQLAlchemy, SQLite / PostgreSQL
**Quantum:** Qiskit, Qiskit Aer, Cirq, PennyLane, qBraid (stub)
**AI:** GPT-OSS-120B via Groq (any OpenAI-compatible endpoint works)
**Deployment:** Vercel (site), Docker / Render (API)

## Quick start

**Prerequisites:** Node 20 or newer; Python 3.11 or 3.12. Qiskit, Cirq and PennyLane do not publish wheels for every newer Python release yet, so 3.13+ may fail to install.

```bash
npm install
```

```bash
python -m venv backend/.venv
backend/.venv/Scripts/pip install -r backend/requirements.txt
```

On macOS or Linux, the venv path is `backend/.venv/bin/pip`.

Run the site and the API in two terminals:

```bash
npm run dev
```

```bash
backend/.venv/Scripts/python -m uvicorn app.main:app --app-dir backend --reload
```

Open http://localhost:3000. To confirm the API is up and see which quantum frameworks it found, check http://localhost:8000/api/health.

The site works with no configuration at all: circuits simulate in the browser, and the API creates its own SQLite database on first run.

## Turn on the live AI tutor (free, about two minutes)

```bash
cp backend/.env.example backend/.env
```

On Windows: `copy backend\.env.example backend\.env`

Get a free key at console.groq.com (API Keys → Create API Key), then set:

```bash
OPENAI_API_KEY=<your groq key>
OPENAI_BASE_URL=https://api.groq.com/openai/v1
QUANTAVERSE_TUTOR_MODEL=openai/gpt-oss-120b
QUANTAVERSE_TUTOR_MAX_TOKENS=1200
```

Restart the API afterwards; settings are read once at startup. `http://localhost:8000/api/tutor/status` should then report `"live": true`.

The model name must match the provider. The default, `gpt-4o-mini`, is not served by Groq. Groq's model list also changes over time, so confirm the model still exists before a demo.

## Configuration

| Variable | Why you would set it |
| --- | --- |
| `QUANTAVERSE_JWT_SECRET` | **Set this for any shared deployment.** If left empty, a new signing key is created on every boot. |
| `QUANTAVERSE_DATABASE_URL` | Point at PostgreSQL to share one record across machines, e.g. `postgresql+psycopg://USER:PASSWORD@HOST:5432/postgres` |
| `QUANTAVERSE_ALLOWED_ORIGINS` | Narrow CORS before deploying. |
| `NEXT_PUBLIC_API_URL` | In `.env.local`, only if the API is not on the default port. |

The full list, including every endpoint, the circuit IR format and the account model, is in [`backend/README.md`](backend/README.md).

## Project layout

```
src/                    Next.js site (App Router)
  app/                  curriculum, algorithms, sandbox, lab, dashboard,
                        network (hub), professor, login, register
  components/           UI by area: ai, mascot, three, sandbox, algorithms, ...
  lib/                  API client, auth, circuit IR, quantum maths
scripts/notes/          theory notes source and the script that prints them
public/notes/           printed theory PDFs, one per module
backend/                FastAPI service
  app/api/routes/       simulate, grade, tutor, auth, progress, notes, teaching
  app/services/         framework adapters, grader, sandbox, tutor, accounts
  app/db/               SQLAlchemy models and session
```

To rebuild the theory PDFs: `npm run notes`

## Security note

`/api/introspect` runs learner code in a separate process with a 5-second hard kill, restricted built-ins and an import whitelist. These are layers of defence, not full isolation. Run it on localhost or behind an authenticated gateway, and do not expose it to the open internet.

## Deployment

The site deploys to Vercel. The API's dependencies are around 790 MB, which exceeds Vercel's serverless limit, so the API runs as a container instead. See [`DEPLOY.md`](DEPLOY.md).

## Research grounding

- McKagan, S. B., Perkins, K. K., Dubson, M., Malley, C., Reid, S., LeMaster, R., & Wieman, C. E. (2008). Developing and researching PhET simulations for teaching quantum mechanics. *American Journal of Physics*, 76, 406. https://doi.org/10.1119/1.2885199
- Schroeder, N. L., Adesope, O. O., & Gilbert, R. B. (2013). How effective are pedagogical agents for learning? A meta-analytic review. *Journal of Educational Computing Research*, 49(1), 1–39. https://doi.org/10.2190/EC.49.1.a
- Alkhatlan, A., & Kalita, J. K. (2018). Intelligent tutoring systems: A comprehensive historical survey with recent developments. https://arxiv.org/abs/1812.09628
- Kim, H., Jeng, M. J., & Smith, K. N. (2025). Toward Human-Quantum Computer Interaction: Interface techniques for usable quantum computing. *CHI 2025*. https://doi.org/10.1145/3706598.3713370
- The Quantum Education Ecosystem: A Review of Global Initiatives, Methods, and Challenges (2026). https://arxiv.org/abs/2604.06293

## License

Code is MIT licensed. Curriculum content is CC BY-SA.

Built by **Bloch 'n roll_A0** for Smart India Hackathon 2026.
