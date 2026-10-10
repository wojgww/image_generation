# AGENTS.md — Specyfikacja Operacyjna Agenta: image_generation

## 1. Cel i Rola Projektu
Projekt **image_generation** realizuje moduł sztucznej inteligencji (AI/ML) dedykowany dla hackathonu **Code for Culture**.
Głównym zadaniem jest model oraz serwis inferencyjny służący do **rozpikselowywania (depixelation / super-resolution / guided restoration)** obrazów polskich dzieł sztuki.

Aplikacja kliencka (projekt mobilny w stylu TikTok):
1. Użytkownik przegląda dzieła kultury w formie feedu.
2. Dostaje uproszczony, rozpikselowany obraz z paletą 5 kolorów do pokolorowania.
3. Po ukończeniu kolorowania obraz jest przesyłany do modułu `image_generation`.
4. Model rekonstruuje/odpikselowuje dzieło do pełnej jakości, odsłaniając oryginał, ciekawostki historyczne oraz kontekst kulturowy w polskim dziedzictwie narodowym.

---

## 2. Architektura i Stack Technologiczny
- **Język**: Python 3.10+
- **Framework ML/DL**: PyTorch, Torchvision, Hugging Face Diffusers / Real-ESRGAN / SwinIR / ControlNet / ONNX Runtime (w zależności od etapu eksperymentów).
- **Przetwarzanie obrazów**: OpenCV, Pillow (PIL), NumPy, Albumentations.
- **API Serwisowe**: FastAPI, Uvicorn, Pydantic (REST API dla aplikacji mobilnej z endpointami `/api/depixelate`, `/api/artworks`, `/api/health`).
- **Zarządzanie środowiskiem**: `venv` / `poetry` / `requirements.txt`.
- **Testy**: pytest, httpx (testy endpointów API i pipeline'u inferencji).

---

## 3. Kluczowe Zasady i Ograniczenia
1. **Lekkość i Czas Reakcji (Latency)**: Na hackathonie kluczowy jest płynny UX w aplikacji mobilnej. Czas odpowiedzi endpointu inferencyjnego powinien być optymalizowany (dążenie do < 1.5s na zapytanie).
2. **Hybrydowa Architektura (Robustness)**:
   - Tryb Mock/Fallback: Dostępny natychmiast dla frontendowców (szybki powrót wysokiej rozdzielczości z blendingiem kolorów na podstawie ID dzieła).
   - Tryb Modelu ML: Rzeczywista rekonstrukcja i super-resolution dla pikselozy i palety barw użytkownika.
3. **Reprodukowalność Danych**: Skrypty pobierania i przetwarzania dzieł (Polona, Cyfrowe Muzeum Narodowe, domena publiczna) muszą być modularne i zautomatyzowane.
4. **Zasady Git**: Zawsze commitować tylko pliki powiązane z danym zadaniem. Nie wrzucać do repozytorium wag modeli (>50MB) ani surowych zbiorów danych (używać `.gitignore` i chmury/skryptów downloadu).

---

## 4. Standardowe Polecenia

```bash
# Aktywacja środowiska wirtualnego
source .venv/bin/activate

# Uruchomienie serwera deweloperskiego API
uvicorn src.api.main:app --host 0.0.0.0 --port 8000 --reload

# Uruchomienie testów jednostkowych i integracyjnych
pytest tests/ -v

# Uruchomienie skryptu generowania syntetycznych par danych (pixelation + 5-color quant)
python -m src.data.generate_dataset --input_dir data/raw --output_dir data/processed
```

---

## 5. Definition of Done (DoD)
Zadanie uznaje się za zakończone, gdy:
- [ ] Kod źródłowy jest w pełni sformatowany, bez błędów składniowych i lintingu.
- [ ] Testy jednostkowe / testy regresji dla nowych funkcji przechodzą pomyślnie.
- [ ] Endpointy API zwracają poprawne formaty odpowiedzi (Base64 lub multipart/form-data) wraz z walidacją typów.
- [ ] Pamięć projektu (`CURRENT_STATE.md`, `DECISIONS.md`, `TASKS.md`) została zaktualizowana.
