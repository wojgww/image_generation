# DECISIONS.md — Architectural Decision Records (ADR)

## ADR-001: Inicjalizacja Repozytorium i Architektura Hybrydowa dla Hackathonu Code for Culture
- **Data**: 2026-10-10
- **Status**: Zastąpione przez ADR-002

---

## ADR-002: Zmiana Założeń — Silnik Kolorowanki Pikselowej zamiast Generatywnego AI
- **Data**: 2026-10-10
- **Status**: Zaakceptowano

### Kontekst
Użytkownik sprecyzował, że serwis nie będzie generować obrazów za pomocą modeli generatywnych (Diffusion/GAN). Oryginalne obrazy wysokiej jakości pochodzą z darmowych, otwartych baz dziedzictwa narodowego (Polona, MNW itp.).
Celem projektu jest przekształcenie autentycznego dzieła sztuki w **prostą kolorowankę pikselową (color-by-number)** z siatką pikseli i dokładnie 5 kolorami do pokolorowania w aplikacji mobilnej w stylu TikTok.

### Decyzja
1. **Odrzucenie ciężkich modeli generatywnych AI**: Zamiast sieci dyfuzyjnych/super-resolution stosujemy szybkie, deterministyczne algorytmy wizji komputerowej (OpenCV, PIL, scikit-learn K-Means).
2. **Architektura silnika kolorowanki**:
   - Moduł `PixelColoringGenerator`:
     - Zmniejszenie obrazu do siatki (np. 32x32 lub 48x48) metodą Box/Area downsampling.
     - Klasteryzacja K-Means ($K=5$) w celu wyodrębnienia 5 dominujących barw.
     - Wygenerowanie numerycznej macierzy pikseli (wartości 1..5) oraz palety barw HEX (`#RRGGBB`).
3. **Kontrakt API**:
   - Backend serwuje gotowe obiekty JSON zawierające: paletę 5 barw, macierz kolorowanki, URL do oryginału HD oraz ciekawostki kulturowe.

### Konsekwencje
- Drastyczne zmniejszenie wymagań sprzętowych (brak potrzeby VRAM/GPU, uruchomienie na zwykłym CPU).
- Czas przetwarzania < 50ms, natychmiastowe ładowanie w aplikacji mobilnej.
- 100% zgodność historyczna — po pokolorowaniu użytkownik widzi rzeczywisty, nienaruszony oryginał polskiego dzieła sztuki.
