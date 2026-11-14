# jupyterstatistics

Kursmaterial i statistik med Python.

| Fil | Innehåll |
|---|---|
| `statistik_grunder.ipynb` | Kursens statistiska begrepp med körbara exempel och **Prova själv**-frågor |
| `statistik_kodguide.md` | Förklaring av koden i notebooken, bibliotek för bibliotek |
| [`course/`](course/README.md) | Kursen uppdelad i 15 notebooks, en per område, med labbar och övningar i en genomtänkt progression |

## Kom igång

OBS! Om du har Windows, se till så att du inte använder PowerShell! I VS Code, används Command Prompt istället (Går att välja om du trycker på pilen bredvid plusset bredvid där det står PowerShell i terminalfönstret i VS Code.). Annars kommer vissa kommandon i terminalen inte fungera korrekt.

Skapa ett virtuellt Python-miljö och aktivera det samt installera alla paket som behövs så här:

```
python -m venv .venv
.venv\Scripts\activate          (macOS/Linux: source .venv/bin/activate)
pip install -r requirements.txt
```

Öppna sedan `statistik_grunder.ipynb` i VS Code eller Jupyter, välj `.venv` som kernel och kör **Restart & Run All**.
