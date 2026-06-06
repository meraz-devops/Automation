# 🧲 Foundit Recruiter Bot

Job description paste karo → bot Foundit pe human jaisa search karta hai → har candidate ka **naam + phone number** Google Sheet (ya local CSV) me daal deta hai.

## Kya-kya hai
- **Frontend** (browser page) — JD paste, live logs, results table
- **Groq (Llama 3.3)** primary + **Gemini** fallback — JD se search filters nikaalta hai
- **Playwright** — real Chrome window, tumhara saved login, human-like delays (block-safe)
- **Google Sheets / CSV** output

---

## ⚙️ Setup (ek baar)

### 1. Python install karo
[python.org](https://www.python.org/downloads/) se Python 3.11+ install karo. Install ke time **"Add Python to PATH"** zaroor tick karna.

### 2. Project setup (PowerShell me, is folder me)
```powershell
cd c:\Users\meraz\Automation
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
playwright install chromium
```

### 3. Keys daalo
`.env` file pehle se bani hai. Usme bas **Foundit ka password** daalo:
```
FOUNDIT_PASSWORD=tumhara_password
```
(Groq + Gemini keys already daali hui hain.)

### 4. (Optional) Google Sheet
Abhi ke liye kuch karne ki zaroorat nahi — data automatically `output/candidates.csv` me jaayega.
Jab Google Sheet chahiye ho, mujhe batao, 5 min me service-account setup kara dunga.

---

## ▶️ Chalana

```powershell
cd c:\Users\meraz\Automation
.\.venv\Scripts\Activate.ps1
uvicorn main:app --reload --app-dir backend --port 8000
```

Phir browser me kholo: **http://localhost:8000**

1. Job Description paste karo
2. "Profiles per run" = 15 (default)
3. **Start** dabao
4. Ek Chrome window khulega — pehli baar khud login kar lena (session save ho jaayega)
5. Bot search karega, numbers nikaalega, table + CSV/Sheet me bhar dega

> **Pehli run me login window khule to manually login kar lo.** Uske baad bot khud yaad rakhega (chrome_profile folder me).

---

## 🛡️ Block na ho — built-in safety
- Real Chrome + saved profile (baar-baar login nahi)
- Har action pe random pause, mouse move, scroll
- Ek run = 15 profile + beech me breaks
- **Rozana 3-4 run se zyada mat karo**, aur har run ke beech 20-30 min gap rakho

## 🧰 Kuch toot jaye to
Bot har error pe `debug/` folder me screenshot + HTML save karta hai.
Wo file mujhe do, main selector turant theek kar dunga.

## 📂 Structure
```
Automation/
├─ .env                 # keys + password
├─ requirements.txt
├─ backend/
│  ├─ main.py           # FastAPI server
│  ├─ foundit_bot.py    # Playwright automation (selectors yaha)
│  ├─ jd_parser.py      # JD -> filters (Groq/Gemini)
│  ├─ sheets.py         # Sheet/CSV writer
│  ├─ human.py          # human-like delays
│  └─ config.py
├─ frontend/index.html  # UI
├─ output/              # candidates.csv yaha
└─ debug/               # error screenshots
```
