# ENGINEERING_BRIEF.md — Specyfikacja Techniczna i Kontrakty API

## 1. Przepływ Danych (Data Flow Architecture)

```
[ Aplikacja Mobilna (TikTok-style) ]
               │
               ▼  1. Pobiera listę dzieł & palet: GET /api/artworks
[ Katalog Dzieł Sztuki (Metadane + 5-Color Palette) ]
               │
               ▼  2. Użytkownik koloruje piksele w interfejsie
[ 5-Kolorowy Obraz Użytkownika ]
               │
               ▼  3. POST /api/depixelate { image_base64, artwork_id, user_palette }
[ Serwis API image_generation (FastAPI) ]
         ┌─────┴──────────────────────┐
         ▼                            ▼
[ Tryb Mock / Fallback ]      [ Model Depixelation / Super-Res ]
(Natychmiastowy blend/HD)    (Rekonstrukcja PyTorch / ONNX)
         │                            │
         └─────────────┬──────────────┘
                       ▼  4. Zwraca obraz HD + kontekst historyczny
[ Odpowiedź API: { restored_image_url / base64, historical_trivia, context_pl } ]
```

---

## 2. Kontrakt API (Specyfikacja OpenAPI / JSON)

### Endpoint: `GET /api/artworks`
Zwraca listę dzieł dostępnych do kolorowania w feedzie.

**Odpowiedź `200 OK`:**
```json
[
  {
    "id": "stanczyk-matejko",
    "title": "Stańczyk",
    "artist": "Jan Matejko",
    "year": "1862",
    "preview_pixelated_url": "/static/pixelated/stanczyk.png",
    "target_palette": ["#7A141A", "#1F1A24", "#C69C55", "#4B2E1E", "#E3D5C5"],
    "grid_size": [48, 64],
    "historical_trivia": "Błazen na balu u królowej Bony przeżywa utratę Smoleńska, podczas gdy dwór beztrosko bawi się za drzwiami.",
    "cultural_context": "Jeden z najważniejszych symboli polskiej refleksji historycznej i odpowiedzialności za losy ojczyzny."
  }
]
```

### Endpoint: `POST /api/depixelate`
Przyjmuje pokolorowany obraz z aplikacji mobilnej i wykonuje rekonstrukcję.

**Request body (`application/json`):**
```json
{
  "artwork_id": "stanczyk-matejko",
  "image_base64": "data:image/png;base64,iVBORw0KGgo...",
  "user_palette": ["#7A141A", "#1F1A24", "#C69C55", "#4B2E1E", "#E3D5C5"],
  "fidelity_mode": "guided" 
}
```

**Odpowiedź `200 OK`:**
```json
{
  "status": "success",
  "restored_image_base64": "data:image/png;base64,iVBORw0KGgo...",
  "artwork_info": {
    "title": "Stańczyk",
    "artist": "Jan Matejko",
    "year": "1862"
  },
  "historical_trivia": "Błazen na balu u królowej Bony przeżywa utratę Smoleńska...",
  "cultural_context": "Jeden z najważniejszych symboli polskiej refleksji historycznej..."
}
```

---

## 3. Algorytm Degradacji Obrazu (Pipeline Danych)
1. **Downscaling**: Obraz źródłowy HD ($H \times W$) zmniejszany bez interpolacji antyaliasingowej (Nearest Neighbor lub Box Filter) do siatki o niskiej rozdzielczości (np. $48 \times 64$).
2. **Kwantyzacja Palety**: K-Means clustering ($K=5$) w przestrzeni barw CIELAB lub RGB, przyporządkowujący każdy piksel do jednego z 5 klastrów.
3. **Symulacja Użytkownika**: Wstrzykiwanie szumu (np. 5-10% niedokładnych pikseli, drobne pomyłki kolorów), aby model uczył się tolerować niedokładne kolorowanie użytkownika.
