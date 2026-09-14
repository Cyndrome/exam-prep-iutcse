# Archway Finance (Pure C SP Lab Project)

**Archway Finance** is a 100% Pure C Personal Finance Ledger, Cash-Flow Monitor, and **Ledger Engine (Running Balance & Daily Tally)** built specifically for the Structured Programming (SP) Lab Course evaluation.

---

## 👥 3-Member Architecture & Responsibility Division

The repository is modularly structured into 3 decoupled components so that 3 team members can independently defend their assigned module:

```
                                +---------------------------------------+
                                |         Archway Finance App           |
                                +---------------------------------------+
                                                    |
          +-----------------------------------------+-----------------------------------------+
          |                                         |                                         |
          v                                         v                                         v
  +-----------------------+                 +-----------------------+                 +-----------------------+
  |  Member 1: BACKEND    |                 |  Member 2: DATABASE   |                 |  Member 3: FRONTEND   |
  | (src/core/ & ledger)  |                 |     (src/storage/)    |                 |      (src/ui/)        |
  +-----------------------+                 +-----------------------+                 +-----------------------+
  | - Transaction Logic   |                 | - Binary Serialization|                 | - Native Desktop GUI  |
  | - Ledger Engine       |                 | - File System I/O     |                 | - Dark Theme Layout   |
  | - Search Filter (C)   |                 | - CSV Import/Export   |                 | - Editable Balances   |
  | - Running Balances    |                 | - Data Recovery       |                 | - Daily Tally Banner  |
  | - Recursive Goals Math|                 | - File Integrity      |                 | - Transaction Tables  |
  +-----------------------+                 +-----------------------+                 +-----------------------+
```

---

## 📚 Module Documentation & Developer Guides

Each team member has an exhaustive line-by-line architecture guide written specifically for their module:

- 🧠 **Backend Core Coder**: See [`BACKEND_CODE_EXPLANATION.md`](BACKEND_CODE_EXPLANATION.md) for line-by-line explanations of `models.h`, `core_engine.c`, `ledger_engine.c`, and `search_filter.c`.
- 🗄️ **Database Storage Coder**: See [`DATABASE_CODE_EXPLANATION.md`](DATABASE_CODE_EXPLANATION.md) for binary serialization (`storage.c`) and CSV reporting.
- 🎨 **Frontend GUI Coder**: See [`FRONTEND_CODE_EXPLANATION.md`](FRONTEND_CODE_EXPLANATION.md) for line-by-line explanations of all 1,517 lines in `src/gui/main_gui.cpp`.

---

## 🛠️ Developer Setup & Git Workflow

### 1. Clone the Repository on your PC
Open your terminal or command prompt and run:
```bash
git clone https://github.com/reza-05/archway_dummy.git
cd archway_dummy
```

### 2. Install Compiler & Graphics Dependencies

#### **macOS:**
```bash
brew install glfw
```

#### **Linux (Ubuntu / Debian):**
```bash
sudo apt update
sudo apt install build-essential libglfw3-dev libgl1-mesa-dev libx11-dev
```

#### **Windows (MSYS2 / MinGW-w64):**
Install GCC/Clang via MSYS2 or use MinGW-w64 compiler suite with GLFW.

---

## 💻 Member Workflows & Development Roles

### 🧠 **Member 1: Backend Core Coder**
- **Files to Edit:**
  - `include/core/core_engine.h`
  - `include/core/ledger_engine.h`
  - `include/core/search_filter.h`
  - `src/core/core_engine.c`
  - `src/core/ledger_engine.c`
  - `src/core/search_filter.c`
- **Testing Commands:**
  ```bash
  ./build.sh
  ./archway_cli --test
  ```

### 🗄️ **Member 2: Database & Storage Coder**
- **Files to Edit:**
  - `include/storage/storage.h`
  - `src/storage/storage.c`
- **Testing Commands:**
  ```bash
  ./build.sh
  ./archway_cli
  ```

### 🎨 **Member 3: Frontend & GUI Coder**
- **Files to Edit:**
  - `src/gui/main_gui.cpp`
  - `src/ui/gui_raylib.c`
  - `src/ui/ui_engine.c`
  - `include/ui/ui_engine.h`
- **Testing Commands:**
  ```bash
  ./build.sh
  ./archway_imgui_gui
  ```

---

## ⚡ How to Build & Run Applications

### 1. Compile All Targets
```bash
./build.sh
```
*(Or use `make` if installed on your OS).*

### 2. Launch Dear ImGui Modern Desktop Window App
```bash
./archway_imgui_gui
```

### 3. Launch Pure C Raylib Desktop Window App
```bash
./archway_raylib_gui
```

### 4. Launch Pure C Terminal App
```bash
./archway_cli
```

---

## 🌿 Git Push & Contribution Commands for Team Members

When working on your changes, follow these standard Git commands:

### **Step 1: Pull latest changes before editing**
```bash
git pull origin main
```

### **Step 2: Check your status & add modified files**
```bash
git status
git add .
```

### **Step 3: Commit your changes with a clear message**
```bash
git commit -m "Update transaction calculation logic in core_engine.c"
```

### **Step 4: Push your commits to GitHub**
```bash
git push origin main
```

---

## 📁 Repository Directory Structure

```text
archway_dummy/
├── build.sh                       <-- One-click build script
├── Makefile                       <-- Multi-target Makefile
├── README.md                      <-- Project documentation & team setup guide
├── BACKEND_CODE_EXPLANATION.md    <-- Member 1: Backend architecture guide
├── DATABASE_CODE_EXPLANATION.md   <-- Member 2: Database architecture guide
├── FRONTEND_CODE_EXPLANATION.md   <-- Member 3: Frontend architecture guide
├── include/
│   ├── models.h                   <-- Data structs & enumerations
│   ├── core/
│   │   ├── core_engine.h          <-- Member 1: Core Engine Header
│   │   ├── ledger_engine.h        <-- Member 1: Ledger Engine Header
│   │   └── search_filter.h        <-- Member 1: Search Filter Header
│   ├── storage/
│   │   └── storage.h              <-- Member 2: Storage Engine Header
│   └── ui/
│       └── ui_engine.h            <-- Member 3: UI Engine Header
├── src/
│   ├── core/
│   │   ├── core_engine.c          <-- Member 1: Core Engine Logic
│   │   ├── ledger_engine.c        <-- Member 1: Running Balance & Daily Tally
│   │   └── search_filter.c        <-- Member 1: String Search & Filter Engine
│   ├── storage/
│   │   └── storage.c              <-- Member 2: Binary File I/O & CSV Export
│   ├── ui/
│   │   ├── gui_raylib.c           <-- Member 3: Pure C Raylib Desktop GUI Window
│   │   └── ui_engine.c            <-- Member 3: Terminal UI Engine
│   ├── gui/
│   │   └── main_gui.cpp           <-- Member 3: Dear ImGui Desktop Window Wrapper
│   └── main.c                     <-- Main Entry Point
```
