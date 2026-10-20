# Gemini Assistant — Desktop AI Study Copilot (macOS & Windows)

An offline-licensed, multi-model AI desktop copilot built with Python to help students analyze screen context, generate flashcards, solve complex math/science problems, parse large study materials, and construct automated exam sets in real time.

Designed for commercial distribution as a standalone `.app` (macOS) and `.exe` (Windows) binary with device-locked offline licensing.

---

## 📌 System Architecture
                              +---------------------------------------+
                              |     Desktop Screen & Input Capture    |
                              |   (pyobjc / Win32 / Hotkey Tracker)   |
                              +---------------------------------------+
                                                  |
                                                  v
                              +---------------------------------------+
                              |     Core GUI Engine (CustomTkinter)   |
                              |    Selection & Clipboard Controller   |
                              +---------------------------------------+
                                                  |
                                                  v
                              +---------------------------------------+
                              | Dynamic AI Model Routing (routing.py) |
                              |  - Token Limit & Cost Optimization    |
                              |  - Automatic Fallback Engine          |
                              +---------------------------------------+
                                      /                       \
                                     v                         v
                   +----------------------------------+ +-------------------------------+
                   |           Google Gemini          | |           Groq API            |
                   | (3.1 Flash-Lite / 3.8 Flash)     | | (OpenAI-compatible GPT-OSS) |
                   +----------------------------------+ +-------------------------------+
                                     \                         /
                                      v                       v
                              +---------------------------------------+
                              |      LaTeX & Math Renderer Engine     |
                              |  (mathtext.py / Matplotlib Support)   |
                              +---------------------------------------+
                                                  |
                                                  v
                              +---------------------------------------+
                              |   Local SQLite Vault & Study Tools    |
                              |  (Flashcards, Wrong Notebook, Exams)  |
                              +---------------------------------------+

---

## ✨ Key Technical Features & Capabilities

### 1. Live Screen Context Capture & Instant AI Actions
* **Multi-OS Capture:** Captures selected screen regions, active windows, or clipboard text using native platform bindings (`pyobjc` on macOS, `Win32 API` & `pynput` on Windows).
* **Instant Actions:** Performs one-click Translation, Summarization, In-depth Explanations, Step-by-step Problem Solving, and Desmos graph guidance.
* **Study Tools Integration:** Directly converts active screen context into Flashcards, Wrong-Answer Notebook entries (`Yanlış Defteri`), or Similar Problem derivations.

### 2. Multi-Model Dynamic Routing (`routing.py`)
* **Context-Aware Model Selection:** Automatically determines the optimal LLM/VLM model based on task complexity, vision requirements, document size, and rate limits.
* **API Providers:**
  * **Google Gemini:** `3.1 Flash-Lite`, `3.5 Flash-Lite`, `3.8 Flash` for multimodal vision tasks and large document context processing.
  * **Groq API:** Fast OpenAI-compatible endpoints (`gpt-oss-120b`, `gpt-oss-20b`) with automated fallback handling for 429 rate limits and reasoning effort tuning.

### 3. Ed25519 Hardware-Locked Offline Licensing (`licensing.py`)
* **Zero-Server Dependency:** Validates client license keys completely offline using asymmetric cryptography (**Ed25519**).
* **Device Fingerprinting:** Binds license keys to specific hardware IDs (`IOPlatformUUID` on macOS, `MachineGuid` on Windows, fallback MAC address).
* **Vendor Admin Panel:** Includes an internal CLI and GUI license generation panel for the seller to manage key pairs and issue signed activation keys (`GA1-` prefix).

### 4. Advanced Math & LaTeX Rendering Engine (`mathtext.py`)
* **Resilient Parsing:** Automatically wraps raw LaTeX expressions, repairs malformed JSON escapes, and recovers truncated JSON responses from LLM outputs.
* **Dual Rendering Mode:** Renders full mathematical formulas using Matplotlib image rendering when enabled, or falls back to clean Unicode (`x²`, `√`, fractions, Greek characters) when disabled.

### 5. Large Document Context Engine (`sources.py`)
* **Map-Reduce Document Processing:** Ingests large PDF and text documents, splitting content into ~80K–250K character chunks for Gemini/Groq processing.
* **Cached Document Summaries:** Generates and caches structured summaries locally to minimize repetitive API calls.

### 6. Automated Exam & Drill Generator (`exams.py` / `drill.py`)
* **Topic-Based Exam Synthesis:** Generates full exam papers based on user-defined subject distributions (e.g., `"Derivatives:10, Limits:5"`).
* **TF-IDF Relevant Chunking:** Uses TF-IDF vectorization to extract the most relevant notes from local study resources.

---

## 🗂️ Module Architecture

.
├── main.py             # Application entry point & thread initializers
├── ui.py               # Main CustomTkinter GUI layout & theme configuration
├── routing.py          # Dynamic LLM/VLM route decision engine
├── providers.py        # Gemini & Groq API client wrappers
├── vault.py            # Local encrypted data persistence & storage
├── sources.py          # PDF parsing & Map-Reduce document summarization
├── exams.py            # Exam generator with TF-IDF context retrieval
├── study.py            # Study plan builder & progress tracker
├── drill.py            # Quick flash-repeat quiz engine
├── mathtext.py         # LaTeX parser, JSON repair & Unicode converter
├── latex.py            # Matplotlib LaTeX image renderer
├── watcher.py          # Clipboard & screen selection monitoring service
├── licensing.py        # Ed25519 offline license verification & hardware ID reader
├── activation.py       # GUI license activation modal
├── platform_win.py     # Windows-specific API hooks (Win32, pynput, clipboard)
└── admin_panel.py      # Vendor-only license generation tool (excluded from client build)


---

## ⌨️ Global Hotkeys

| Action | macOS Hotkey | Windows Hotkey |
| :--- | :--- | :--- |
| **Screen Capture (Snipping)** | `⌘ + Shift + S` | `Ctrl + Alt + S` |
| **Toggle Copilot Panel** | `⌘ + Shift + A` | `Ctrl + Alt + A` |
| **Open Rapid Drill Mode** | `⌘ + Shift + D` | `Ctrl + Alt + D` |

---

## 🛠️ Tech Stack & Dependencies

* **GUI Framework:** CustomTkinter, Pillow
* **AI Libraries:** `google-genai`, `groq`, `requests`, `certifi`
* **Document & Math Processing:** `pypdf`, `matplotlib`, `reportlab`
* **Cryptography:** `cryptography` (Ed25519 imzalama)
* **OS Interoperability:** `pyobjc` (macOS Cocoa/Quartz), `pynput`, `pyperclip`, `ctypes` (Windows Win32 API)
* **Packaging:** PyInstaller, GitHub Actions CI/CD for Windows `.exe` builds

---

## 🚀 Roadmap & Upcoming Features

- [ ] **Video / Lecture Recording Ingestion (Priority):** Support for processing full lecture videos (via FFmpeg chunking). Automatic extraction of speech, whiteboard notes, and formulas using Gemini VLM with timestamped transcripts.
- [ ] **Automated Onboarding Guide:** First-launch interactive setup wizard for API key registration and OS accessibility permissions.
- [ ] **Spaced Repetition (SM-2):** Integration of SuperMemo-2 algorithm for optimized flashcard review scheduling.
- [ ] **Apple Developer Signing & Notarization:** Out-of-the-box signed `.app` distribution for macOS Gatekeeper compliance.

---

## 🔒 License & Intellectual Property Notice

This repository contains architecture specifications, UI design patterns, and module definitions. Source code binaries and licensing keys are proprietary and protected under commercial distribution rights.
