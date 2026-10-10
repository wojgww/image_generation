# CURRENT_STATE.md — Bieżący Stan Projektu: image_generation

## Status Projektu
`Inicjalizacja i Planowanie Architektury`

---

## 1. Działające Elementy
- [x] Repozytorium zainicjalizowane lokalnie i przygotowane do utworzenia na GitHubie.
- [x] Konfiguracja `.gitignore` dla Pythona, PyTorcha, modeli wagowych i zbiorów danych.
- [x] Kompletna dokumentacja pamięci projektu: `AGENTS.md`, `PROJECT_BRIEF.md`, `CURRENT_STATE.md`, `DECISIONS.md`, `TASKS.md`, `.docs/ENGINEERING_BRIEF.md`.
- [x] Rejestracja w centralnym katalogu projektów (`INDEX.md`).

---

## 2. Elementy w Trakcie / Do Zrobienia
- Utworzenie zdalnego repozytorium GitHub (`gh repo create wojgww/image_generation`).
- Wybór architektury bazowej modelu (SwinIR / Real-ESRGAN / ControlNet / U-Net / rozwiązanie hybrydowe z dynamicznym blendingiem).
- Szkielet projektu (`src/api`, `src/models`, `src/data`, `src/utils`, `tests`).
- Endpoint FastAPI do natychmiastowej integracji z aplikacją mobilną (początkowo z trybem mock/fallback, aby frontend mógł testować UI).
- Skrypt pobierający / generujący dataset polskich dzieł sztuki (Polona / Cyfrowe MNW) i tworzący pary: [oryginał HD, rozpikselowany obraz 5-kolorowy].

---

## 3. Aktywne Blokery
- Brak blokerów technicznych.

---

## 4. Najbliższe Kroki
1. Utworzenie repozytorium GitHub i wypchnięcie pierwszego commitu.
2. Przygotowanie struktury katalogów i pliku `pyproject.toml` / `requirements.txt`.
3. Implementacja szkieletu API FastAPI z testami jednostkowymi.
4. Implementacja pipeline'u przetwarzania i degradacji obrazu (Pixelate + 5-color palette quantization).
