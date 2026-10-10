# TASKS.md — Rejestr Zadań Projektu (Pixel Coloring Engine)

## Faza 1: Inicjalizacja i Repozytorium (Zakończone)
- [x] Inicjalizacja lokalnego repozytorium git (`main`)
- [x] Utworzenie repozytorium GitHub (`wojgww/image_generation`) i pierwszy push
- [x] Stworzenie i aktualizacja dokumentacji pamięci projektu (`AGENTS.md`, `PROJECT_BRIEF.md`, `CURRENT_STATE.md`, `DECISIONS.md`, `TASKS.md`, `.docs/ENGINEERING_BRIEF.md`)
- [x] Rejestracja w centralnym rejestrze `INDEX.md`

## Faza 2: Środowisko i Silnik Przetwarzania Obrazu (Pixel Coloring Core)
- [ ] Utworzenie `requirements.txt` (FastAPI, uvicorn, pillow, opencv-python-headless, scikit-learn, numpy, pytest, httpx)
- [ ] Implementacja modułu `src/core/pixel_engine.py`:
  - Przeskalowanie dzieła do siatki (np. 32x32 lub 48x48)
  - Ekstrakcja 5 optymalnych kolorów za pomocą K-Means (przestrzeń CIELAB lub RGB)
  - Kwantyzacja i mapowanie pikseli do ID 1–5
  - Eksport macierzy numerycznej oraz palety HEX
  - Generowanie podglądu PNG (kolorowa pikseloza oraz wersja z numerami)

## Faza 3: Baza Dzieł Sztuki i Metadanych Kulturowych
- [ ] Utworzenie bazy `src/data/artworks.json` z polskimi arcydziełami (np. Jan Matejko „Stańczyk”, Józef Chełmoński „Babie lato”, Stanisław Wyspiański „Chochoły”, Aleksander Gierymski „Żydówka z pomarańczami”)
- [ ] Dołączenie ciekawostek historycznych, anegdot oraz kontekstu kulturowego do każdego dzieła
- [ ] Skrypt przetwarzający bazę dzieł na gotowe zestawy kolorowanek

## Faza 4: API REST dla Aplikacji Mobilnej (FastAPI)
- [ ] Endpoint `GET /api/artworks` — lista dzieł do feedu (z paletą 5 kolorów, wymiarami siatki, macierzą i ciekawostkami)
- [ ] Endpoint `GET /api/artworks/{id}` — szczegóły konkretnej kolorowanki
- [ ] Endpoint `POST /api/convert` — możliwość przesłania dowolnego obrazu i konwersji w locie na 5-kolorową kolorowankę
- [ ] Endpoint `GET /health` — health check
- [ ] Konfiguracja CORS (zezwolenie na zapytania z aplikacji mobilnej Expo/React Native)

## Faza 5: Testy i Weryfikacja
- [ ] Testy jednostkowe silnika kwantyzacji (`tests/test_pixel_engine.py`)
- [ ] Testy endpointów API (`tests/test_api.py`)
