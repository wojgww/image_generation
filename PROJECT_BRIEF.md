# PROJECT_BRIEF.md — Podsumowanie Projektu: image_generation

## 1. Kontekst Biznesowy i Hackathonowy
Projekt powstaje na potrzeby hackathonu **Code for Culture**.
Stanowi komponent sztucznej inteligencji współpracujący z mobilną aplikacją w stylu **TikTok** („Scroll & Paint Culture”).

### Idea Aplikacji:
- Użytkownicy scrollują pionowy feed dzieł sztuki (kanon polskiego malarstwa i kultury).
- Zamiast pasywnego oglądania, użytkownik angażuje się w interakcję: otrzymuje rozpikselowany obraz z ograniczoną paletą (np. 5 kolorów).
- Użytkownik koloruje obraz (lub dotyka fragmentów, by wypełnić segmenty barwami).
- Po ukończeniu zadania moduł AI wykonuje **rozpikselowanie / rekonstrukcję** (depixelation / super-resolution), odsłaniając pełną jakość dzieła.
- Jednocześnie prezentowane są ciekawostki historyczne, anegdoty oraz rola, jaką to dzieło odegrało w kulturze i tożsamości narodowej.

---

## 2. Cele Systemu AI (`image_generation`)
1. **Model / Pipeline Rozpikselowywania**:
   - Przyjmowanie na wejściu obrazu o niskiej rozdzielczości / pikselozy skwantyzowanej do 5 kolorów.
   - Generowanie / rekonstrukcja obrazu o wysokiej rozdzielczości (High Definition) odpowiadającego oryginalnemu dziełu, z uwzględnieniem stylistyki barw nałożonych przez użytkownika.
2. **Serwis API**:
   - Szybki, bezawaryjny interfejs REST API (FastAPI) umożliwiający integrację z aplikacją mobilną (React Native / Flutter / Swift).
   - Obsługa metadanych dzieł sztuki (opis, autor, rok, kontekst kulturowy, paleta bazowa 5 kolorów, ciekawostki).
3. **Pipeline Danych**:
   - Zautomatyzowane pobieranie i przygotowanie zestawu polskich dzieł malarstwa (np. Jan Matejko, Józef Chełmoński, Stanisław Wyspiański, Aleksander Gierymski, Olga Boznańska, Zdzisław Beksiński).
   - Algorytm degradacji: High-Res -> Pixelate (np. 32x32 / 64x64) -> Color Quantization (K-Means / Median Cut do 5 kolorów) tworzący syntetyczne pary uczące.

---

## 3. Kluczowe Metryki Sukcesu
- **Czas odpowiedzi inferencji (P95)**: < 1.5 sekundy (dla zachowania dynamiki feedu a la TikTok).
- **Jakość wizualna (PSNR / SSIM / LPIPS / ocena subiektywna)**: Wysoka estetyka zrekonstruowanego obrazu bez widocznych artefaktów kompresji.
- **Gotowość integracyjna**: Prosty kontrakt API (wejście: Base64/Multipart + ID dzieła -> wyjście: Base64 HD + metadane historyczne).
