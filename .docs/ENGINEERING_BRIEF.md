# ENGINEERING_BRIEF.md — Specyfikacja Techniczna Silnika Kolorowanki Pikselowej

## 1. Architektura i Przepływ Danych

```
[ Otwarte Bazy Dzieł Kultury ] (Polona / Cyfrowe MNW / Wikimedia Commons)
               │ (oryginalne obrazy HD)
               ▼
   [ Pixel Coloring Engine (src/core/pixel_engine.py) ]
   1. Resize do siatki (np. 32x32 lub 48x48)
   2. K-Means clustering (K = 5 dominant colors w CIELAB/RGB)
   3. Kwantyzacja pikseli -> macierz ID: 1..5
   4. Wygenerowanie palety HEX [#RRGGBB, ...]
               │
               ▼
[ Baza Dzieł Sztuki + Ciekawostki Kulturowe ] (src/data/artworks.json)
               │
               ▼
       [ FastAPI REST API ]
   ├── GET /api/artworks (zwraca dzieła z siatką 1-5, paletą i ciekawostkami)
   ├── GET /api/artworks/{id} (szczegóły konkretnego dzieła)
   └── POST /api/convert (konwersja dowolnego obrazu do kolorowanki)
               │
               ▼
[ Aplikacja Mobilna TikTok-style ]
   1. Wyświetla siatkę pikseli z numerami 1-5 i paletą na dole
   2. Użytkownik koloruje pola
   3. Po ukończeniu: płynna animacja "odpikselowania" do oryginału HD
   4. Prezentacja ciekawostki historycznej i kontekstu narodowego
```

---

## 2. Format Danych Zwracany do Aplikacji Mobilnej (`GET /api/artworks`)

```json
[
  {
    "id": "stanczyk-matejko",
    "title": "Stańczyk",
    "artist": "Jan Matejko",
    "year": "1862",
    "original_image_url": "https://.../stanczyk_hd.jpg",
    "thumbnail_pixelated_url": "/static/previews/stanczyk_pixel.png",
    "grid": {
      "width": 32,
      "height": 32,
      "data": [
        [1, 1, 2, 2, ...],
        [1, 3, 4, 5, ...]
      ]
    },
    "palette": [
      { "id": 1, "hex": "#7A141A", "name": "Szkarłat królewski" },
      { "id": 2, "hex": "#1F1A24", "name": "Cień nocy" },
      { "id": 3, "hex": "#C69C55", "name": "Złoto wawelskie" },
      { "id": 4, "hex": "#4B2E1E", "name": "Ciemny brąz" },
      { "id": 5, "hex": "#E3D5C5", "name": "Kość słoniowa" }
    ],
    "cultural_data": {
      "trivia": "Stańczyk na balu u królowej Bony jest jedyną osobą przeżywającą utratę Smoleńska, podczas gdy polski dwór beztrosko bawi się w sąsiedniej sali.",
      "historical_context": "Dzieło symbolizuje patriotyczną troskę i odpowiedzialność za losy Rzeczypospolitej. Matejko nadał błaznowi własne rysy twarzy.",
      "location": "Muzeum Narodowe w Warszawie"
    }
  }
]
```

---

## 3. Algorytm Przetwarzania Obrazu na Kolorowankę
1. **Wczytanie i Skalowanie**:
   - Obraz źródłowy jest skalowany z zachowaniem proporcji do wymiaru docelowego (np. $32 \times 32$ piksele) za pomocą filtru `PIL.Image.Resampling.BOX` lub `LANCZOS`, aby uśrednić lokalne barwy.
2. **Ekstrakcja Palety 5 Barw**:
   - Piksele reprezentowane są jako wektory RGB (lub CIELAB dla lepszej percepcji ludzkiego oka).
   - Uruchamiany jest algorytm K-Means ($K=5$, `n_init=10`).
   - Centroidy stają się 5 kolorami palety, posortowanymi wg jasności lub częstotliwości występowania.
3. **Mapowanie do Siatki**:
   - Każdy piksel w siatce otrzymuje etykietę klastra $1, 2, 3, 4, 5$.
   - Rezultatem jest prosta tablica 2D o rozmiarze $H \times W$ zawierająca liczby od 1 do 5.
