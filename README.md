# Image Generation & Depixelation Service (Code for Culture)

Model i serwis sztucznej inteligencji (AI/ML) do **rozpikselowywania (depixelation / super-resolution / guided restoration)** polskich dzieł sztuki dla aplikacji hackathonowej **Code for Culture**.

---

## 🎨 O Projekcie
Współpracuje z mobilną aplikacją w stylu **TikTok** („Scroll & Paint Culture”):
1. Użytkownik przegląda feed dzieł polskiego malarstwa.
2. Otrzymuje rozpikselowane dzieło z dedykowaną 5-kolorową paletą barw do pokolorowania.
3. Po ukończeniu zadania moduł AI rekonstruuje pełną jakość obrazu (High Definition).
4. Aplikacja odsłania odnowione dzieło wraz z ciekawostkami historycznymi i kontekstem kulturowym.

---

## 🛠️ Architektura i Stack
- **Python 3.10+**, **PyTorch**, **Torchvision**
- **FastAPI / Uvicorn** — szybki serwis REST API
- **OpenCV / Pillow / NumPy** — przetwarzanie i degradacja obrazów (kwantyzacja K-Means)
- **Model Baseline**: Super-Resolution / Guided Restoration (SwinIR / Real-ESRGAN / Conditional U-Net)

---

## 🚀 Szybki Start
```bash
# Klonowanie repozytorium
git clone https://github.com/wojgww/image_generation.git
cd image_generation

# Środowisko wirtualne
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt

# Uruchomienie API (FastAPI)
uvicorn src.api.main:app --host 0.0.0.0 --port 8000 --reload
```

---

## 📖 Pamięć Projektu
- [`AGENTS.md`](file:///home/ai_workspace/Projekty/image_generation/AGENTS.md) — Wytyczne i zasady operacyjne
- [`PROJECT_BRIEF.md`](file:///home/ai_workspace/Projekty/image_generation/PROJECT_BRIEF.md) — Kontekst biznesowy i hackathonowy
- [`CURRENT_STATE.md`](file:///home/ai_workspace/Projekty/image_generation/CURRENT_STATE.md) — Aktualny stan i postępy
- [`DECISIONS.md`](file:///home/ai_workspace/Projekty/image_generation/DECISIONS.md) — Rejestr decyzji architektonicznych (ADR)
- [`TASKS.md`](file:///home/ai_workspace/Projekty/image_generation/TASKS.md) — Plan i backlog zadań
- [`.docs/ENGINEERING_BRIEF.md`](file:///home/ai_workspace/Projekty/image_generation/.docs/ENGINEERING_BRIEF.md) — Kontrakty API i specyfikacja techniczna
