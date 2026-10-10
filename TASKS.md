# TASKS.md — Rejestr Zadań Projektu

## Faza 1: Inicjalizacja i Konfiguracja Środowiska (W toku)
- [x] Inicjalizacja lokalnego repozytorium git (`main`)
- [x] Przygotowanie konfiguracji `.gitignore`
- [x] Stworzenie dokumentacji pamięci projektu (`AGENTS.md`, `PROJECT_BRIEF.md`, `CURRENT_STATE.md`, `DECISIONS.md`, `TASKS.md`, `.docs/ENGINEERING_BRIEF.md`)
- [x] Dodanie wpisu projektu do centralnego indeksu wiedzy (`INDEX.md`)
- [ ] Utworzenie zdalnego repozytorium GitHub (`wojgww/image_generation`) i pierwszy push

## Faza 2: Szkielet Architektury i Kontrakt API
- [ ] Konfiguracja `pyproject.toml` / `requirements.txt`
- [ ] Implementacja struktury katalogowej (`src/api`, `src/models`, `src/data`, `src/utils`, `tests`)
- [ ] Implementacja FastAPI z endpointami:
  - `GET /health`
  - `GET /api/artworks` (lista dzieł z miniaturami, 5-kolorowymi paletami i ciekawostkami)
  - `POST /api/depixelate` (przyjmuje obraz wejściowy użytkownika + artwork_id, zwraca zrekonstruowany obraz + kontekst historyczny)
- [ ] Przygotowanie trybu mock/fallback (natychmiastowe wsparcie dla aplikacji mobilnej)
- [ ] Testy jednostkowe endpointów API (pytest + httpx)

## Faza 3: Pipeline Danych i Przetwarzanie Obrazów
- [ ] Skrypt pobierania wybranych dzieł polskiego malarstwa z wolnych źródeł (Polona / Cyfrowe MNW / Wikimedia Commons)
- [ ] Skrypt degradacji obrazu:
  - Skalowanie w dół / pikseloza (np. do siatki 32x32 lub 64x64)
  - Kwantyzacja kolorów do 5 dominujących barw (K-Means / Median Cut)
  - Zapisywanie par [pixel_5color, original_hd]
- [ ] Generator metadanych kulturowych (autor, rok, anegdoty, znaczenie w kulturze polskiej) w formacie JSON

## Faza 4: Model AI & Rekonstrukcja Dzieł
- [ ] Baseline modelu Super-Resolution (np. Real-ESRGAN / SwinIR / Pix2Pix)
- [ ] Eksperyment z kondycjonowaniem na bazie ID dzieła i palety barw użytkownika
- [ ] Optymalizacja czasu inferencji (ONNX / TensorRT / kwantyzacja wag do FP16)
- [ ] Ewaluacja jakości (metryki wizualne, czas odpowiedzi)

## Faza 5: Wdrożenie i Integracja Hackathonowa
- [ ] Konteneryzacja (Dockerfile / docker-compose)
- [ ] Wdrożenie serwera API (np. VPS Hetzner / HuggingFace Spaces / Modal / ngrok do testów mobilnych)
- [ ] Testy end-to-end z aplikacją mobilną TikTok-style
