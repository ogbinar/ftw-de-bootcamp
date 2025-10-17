# FTW DE BOOTCAMP

The [For the Women Foundation](https://www.ftwfoundation.org/) Data Engineering Bootcamp delivers six Saturdays of hands-on ELT for ~30 scholars. This repo jump-starts your environment so you can focus on concepts and code.

# Bootcamp 6-Week Timeline

*All sessions are Saturdays, each with a morning lecture (3 hrs) and afternoon lab (3 hrs).*

|  Week | Focus                               | Morning Lecture                                 | Afternoon Lab                                                                    |
| :---: | ----------------------------------- | ----------------------------------------------- | -------------------------------------------------------------------------------- |
| **1** | Foundations & First Pipeline        | Intro to Data Eng, ETL vs ELT, dlt basics       | Ingest Auto MPG → ClickHouse, query in SQL, clean + first Metabase viz           |
| **2** | Data Modeling & Testing             | Database design, normalization, Chinook ERDs    | Load Chinook → dbt setup, schema tests |
| **3** | Time Series & Incremental Loads     | Temporal SQL, window functions, Meteo dataset   | dbt temporal tests + incremental Bitcoin ingestion & validations                 |
| **4** | Web & API Pipelines                 | Web scraping ethics, APIs, staging raw data     | Ingest Lazada + Pokémon API → dbt transforms, custom tests, Metabase dashboards  |
| **5-6** | Capstone Activities  |     Casptone Development     |        Documentation & Presentation Practice                |

# Hardware Setup
## 💻 Minimum PC Specs for the Data Engineering Bootcamp

Running our bootcamp environment means using **Docker containers** (tiny virtual computers) for databases (ClickHouse + Postgres), a dashboard tool (Metabase), and job runners (dlt + dbt).

This isn’t as heavy as gaming or video editing, but it still needs enough resources so your laptop doesn’t slow down or crash.

---

### ✅ Local Setup (everything on your laptop)

**Minimum (it will work, but might feel slow):**

* **CPU:** 4 cores (Intel i5 / Ryzen 5 or newer)
* **RAM:** 8 GB
* **Storage:** 50 GB free space on SSD
* **OS:** Linux (Ubuntu, Fedora) or Windows 10/11 with WSL2 (Ubuntu)
* **Tip:** Mac M1/M2 is fine, but may need an extra setting in Docker.

**Recommended (for a smoother experience):**

* **CPU:** 4–6 cores
* **RAM:** 16 GB
* **Storage:** 100 GB free SSD
* **Reason:** Chrome + VSCode + Docker + databases running together can easily use 10+ GB memory.

---

### 🌐 Remote Hybrid Setup (server runs core, students run jobs)

Sometimes we’ll run the **databases + Metabase** on a **remote server** (e.g., AWS), and you’ll only run the **dlt/dbt jobs** on your laptop.

**Server specs (teacher side):**

* **CPU:** 4 vCPU (8 vCPU if 10+ students connect at once)
* **RAM:** 16 GB minimum (32 GB for bigger classes)
* **Storage:** 100–200 GB SSD
* **Network:** Open ports `8123`, `9000`, `3001`

**Student laptops (lighter load in this mode):**

* **CPU:** 2–4 cores
* **RAM:** 8 GB (still better with 16 GB)
* **Storage:** 20–50 GB free SSD

---

### 📝 TL;DR (Quick Checklist)

* **Will 8 GB RAM work?** → Yes, but it will feel tight.
* **Best for smooth experience?** → 16 GB RAM, 4 cores, SSD storage.
* **Server for class:** → Start with 8 vCPU + 16 GB RAM + 200 GB SSD.

---

⚡ **Tip:** If you only have 8 GB RAM, close heavy apps (Chrome tabs, video calls, Spotify, etc.) before starting Docker.

# Environment Setup

## ✅ What You’ll Install

* **Git** – version control
* **Docker** – containers (Desktop or Engine)
* **DBeaver** – SQL database GUI
* **GitHub** – account + basic config

Note: Make sure to run the commands one line at a time!
---

## Windows (with WSL: Ubuntu)

### 1) Install WSL + Ubuntu

* Guide: [https://learn.microsoft.com/en-us/windows/wsl/install](https://learn.microsoft.com/en-us/windows/wsl/install)
* Or quick install (run in **Windows PowerShell** as Admin):

```powershell
wsl --install
```

After reboot, open **Ubuntu** (from Start Menu) and set a username/password.

### 2) Inside WSL (Ubuntu): update base system

```bash
sudo apt update && sudo apt upgrade -y
```

### 3) Install Git (inside WSL)

```bash
sudo apt install -y git
git --version
```

### 4) Install Docker (choose ONE)

**Option A — Docker Desktop (recommended for most users)**

1. Install: [https://docs.docker.com/desktop/install/windows-install/](https://docs.docker.com/desktop/install/windows-install/)
2. In Docker Desktop → **Settings → Resources → WSL integration**, toggle **your Ubuntu** distro ON.
3. Test in WSL:

   ```bash
   docker run hello-world
   ```

**Option B — Native Docker Engine inside WSL (no Docker Desktop)**

1. **Enable systemd in WSL**
   Create/modify `/etc/wsl.conf`:

   ```bash
   sudo tee /etc/wsl.conf >/dev/null <<'EOF'
   [boot]
   systemd=true
   EOF
   ```

   Then in **Windows PowerShell**:

   ```powershell
   wsl --shutdown
   ```

   Reopen Ubuntu.

2. **Install Docker Engine**

   ```bash
   # prerequisites
   sudo apt install -y ca-certificates curl gnupg

   # keyring
   sudo install -m 0755 -d /etc/apt/keyrings
   sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
   sudo chmod a+r /etc/apt/keyrings/docker.asc

   # repo
   echo \
     "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
     $(. /etc/os-release && echo ${UBUNTU_CODENAME:-$VERSION_CODENAME}) stable" \
     | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

   sudo apt update
   sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
   ```

3. **Enable + use Docker without sudo**

   ```bash
   sudo systemctl enable --now docker
   sudo usermod -aG docker $USER
   # pick up new group without reopening the terminal:
   newgrp docker
   docker run hello-world
   ```

> 🛠️ If you see `permission denied` on `/var/run/docker.sock`, open a new terminal (or run `newgrp docker`).
> 🛠️ If the daemon isn’t running, check: `systemctl status docker`.

### 5) Install DBeaver (Windows app)

* Download & install: [https://dbeaver.io/download/](https://dbeaver.io/download/) (Windows installer)

> Tip: Run DBeaver on Windows; it connects fine to databases running in WSL containers.

---

## macOS

### 1) Install Git

* Easiest: run `git` once and accept the **Xcode Command Line Tools** prompt:

```bash
git --version
```

* Or with Homebrew:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
brew install git
```

### 2) Install Docker Desktop (macOS)

* [https://docs.docker.com/desktop/setup/install/mac-install/](https://docs.docker.com/desktop/setup/install/mac-install/)
* Test:

```bash
docker run hello-world
```

### 3) Install DBeaver (macOS)

* [https://dbeaver.io/download/](https://dbeaver.io/download/)
* Or via Homebrew:

```bash
brew install --cask dbeaver-community
```

---

## Linux (Ubuntu/Debian)

### 1) Install Git

```bash
sudo apt update
sudo apt install -y git
git --version
```

### 2) Install Docker Engine

```bash
# prerequisites
sudo apt install -y ca-certificates curl gnupg

# keyring
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# repo
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo ${UBUNTU_CODENAME:-$VERSION_CODENAME}) stable" \
  | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# enable + run without sudo
sudo systemctl enable --now docker
sudo usermod -aG docker $USER
# re-login or:
newgrp docker

# test
docker run hello-world
```

### 3) Install DBeaver (Linux)

* **.deb package** (Ubuntu/Debian):

  ```bash
  wget https://dbeaver.io/files/dbeaver-ce_latest_amd64.deb
  sudo apt install -y ./dbeaver-ce_latest_amd64.deb
  ```
* **Flatpak** (alternative):

  ```bash
  sudo apt install -y flatpak
  flatpak install flathub io.dbeaver.DBeaverCommunity
  ```

---

## Continuation (All Platforms)

### 1) Create a GitHub account

* [https://docs.github.com/en/get-started/start-your-journey/creating-an-account-on-github](https://docs.github.com/en/get-started/start-your-journey/creating-an-account-on-github)

### 2) Configure Git (name, email, default branch, line endings)

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
git config --global init.defaultBranch main
git config --global pull.rebase false
git config --global core.autocrlf input   # macOS/Linux
# On Windows (PowerShell): git config --global core.autocrlf true
```

> (Optional) SSH setup:

```bash
ssh-keygen -t ed25519 -C "you@example.com"
# Add the public key (~/.ssh/id_ed25519.pub) to GitHub: Settings → SSH and GPG keys
```

### 3) Clone the repository

```bash
git clone https://github.com/ogbinar/ftw-de-bootcamp.git
cd ftw-de-bootcamp
```

(Or use SSH if you added keys:

```bash
git clone git@github.com:ogbinar/ftw-de-bootcamp.git
```

)

---

## Quick Verification Checklist

* `git --version` shows a version ✔️
* `docker run hello-world` prints the “Hello from Docker!” message ✔️
* DBeaver launches  ✔️

---


# Lesson Proper

## Technical Instructions
- Lecture slides will be provided separately
- Technical overview and guide can be found [here](TECHNICAL-README.md).
- Configuration for Local vs Remote setup can be found [here](LOCAL-REMOTE-SETUP.md).
- Example of first exercise pipeline logs can be found [here](Example.md).

## Exercises & Docs
- For Dimensional Modeling Exercises, Read [this](MODELING-EXERCISES.md).
- How do you document & present your DE project? Read [this](DOC-GUIDE.md).
- Other documentation tips. Read [this](TECHNICAL-DOCS.md).
- Data Quality checks using DBT? Read [this](DQ-TESTS.md).
- Learn how to connect Tableau Desktop to ClickHouse server [here](tableau2clickhouse.md).

# Track Your Learnings

A minimal learning journal to capture learnings, reflections, vocabulary, and the **DE mindset**—especially for documenting, communicating, and presenting. Journal repo can be found [here](https://github.com/ogbinar/ftw-de-journal).

> clone and rename:  
> `git clone https://github.com/ogbinar/ftw-de-journal.git`

