# DECISIONS.md — Architectural Decision Records (ADR)

## ADR-001: Inicjalizacja Repozytorium i Architektura Hybrydowa dla Hackathonu Code for Culture
- **Data**: 2026-10-10
- **Status**: Zaakceptowano

### Kontekst
W ramach hackathonu Code for Culture tworzony jest moduł backendowy/AI dla aplikacji mobilnej a la TikTok. Użytkownik koloruje 5-kolorową pikselozę, po czym model ma odsłonić / zrekonstruować pełne dzieło malarstwa polskiego wraz z ciekawostką historyczną. Frontend potrzebuje natychmiastowego kontraktu API, podczas gdy model ML wymaga iteracyjnego treningu/fine-tuningu.

### Decyzja
1. Tworzymy dedykowane repozytorium `image_generation` zintegrowane z profilem GitHub użytkownika (`wojgww`).
2. Przyjmujemy architekturę trójwarstwową:
   - **Warstwa API (FastAPI)**: Udostępnia kontrakt `/api/depixelate` oraz `/api/artworks`. W fazie bootstrapu udostępnia silnik symulacyjny/mock (aby zablokować zależność zespołu frontendowego).
   - **Warstwa Przetwarzania Danych (`src/data`)**: Skrypty automatycznej degradacji obrazów (Pixelate + K-Means / Median Cut 5 barw) tworzące pary treningowe.
   - **Warstwa Modeli (`src/models`)**: Dwuetapowa ścieżka: (1) Szybki baseline Super-Resolution (Real-ESRGAN / SwinIR / Conditional U-Net) dla niskiej latencji, (2) Opcjonalne głębokie dobarwianie dyfuzyjne (ControlNet / SD-Turbo) w zależności od dostępnych zasobów GPU.

### Konsekwencje
- Frontend może natychmiast integrować aplikację mobilną bez czekania na wielogodzinny trening modelu.
- Bezpieczny i elastyczny pipeline danych z pełną kontrolą nad polskimi dziełami sztuki.
