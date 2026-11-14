# Statistik med Python – kurs i 15 steg

Kursen bygger på `statistik_grunder.ipynb` men är uppdelad i en notebook per område. Varje notebook börjar med
små exempel som går att räkna på i huvudet och bygger sedan vidare steg för steg. Du lär dig genom att **köra, ändra och
gissa**, inte bara genom att läsa.

## Så här är varje notebook upplagd

| Markering | Betyder |
|---|---|
| 🔮 **Gissa först** | Bestäm dig för ett svar *innan* du kör cellen. |
| 🧪 **Labba** | Ändra värdena som är markerade med `# ← ändra` och kör om. Leta efter mönster. |
| ✏️ **Din tur** | Skriv egen kod i den tomma cellen. |
| 💡 **Lösningsförslag** | Klicka för att fälla ut. Titta först när du har försökt själv. |

Varje notebook inleds med lärandemål och förkunskaper och avslutas med en sammanfattning och kontrollfrågor med svar.
Notebookarna är sparade **utan utdata**, så att du kan gissa innan du kör.

## Kursens gång

Gör notebookarna i ordning. Varje del bygger på de tidigare.

### Del A – Beskriva data
| # | Notebook | Du lär dig |
|---|---|---|
| 1 | [Verktygen](01_verktygen/01_verktygen.ipynb) | Jupyter, NumPy, slumptal, pandas och matplotlib – precis det kursen behöver |
| 2 | [Variabeltyper och skalnivåer](02_variabeltyper/02_variabeltyper.ipynb) | Kvalitativ/kvantitativ, nominal → kvot, och varför det avgör vad man får räkna ut |
| 3 | [Deskriptiv statistik](03_deskriptiv_statistik/03_deskriptiv_statistik.ipynb) | Lägesmått, spridningsmått, frekvenstabeller, histogram och lådagram |
| 4 | [Fördelningar och normalfördelningen](04_fordelningar/04_fordelningar.ipynb) | Täthetskurvor, 68–95–99,7-regeln, z-värden och sannolikheter med `stats.norm` |

### Del B – Från stickprov till population
| # | Notebook | Du lär dig |
|---|---|---|
| 5 | [Population och stickprov](05_population_och_stickprov/05_population_och_stickprov.ipynb) | Urvalsfel, medelfel, $1/\sqrt{n}$ och centrala gränsvärdessatsen |
| 6 | [Datakvalitet, felkällor och bias](06_datakvalitet_och_bias/06_datakvalitet_och_bias.ipynb) | Bortfall, täckningsfel, mätfel – och varför mer data inte botar bias |
| 7 | [Punktskattning och konfidensintervall](07_konfidensintervall/07_konfidensintervall.ipynb) | Konfidensintervall för medelvärde och andel, tolkning, t-fördelningen |

### Del C – Hypotesprövning
| # | Notebook | Du lär dig |
|---|---|---|
| 8 | [Hypotesprövning och p-värden](08_hypotesprovning/08_hypotesprovning.ipynb) | Nollhypotes, p-värde, t-test för ett och två medelvärden, ensidigt/dubbelsidigt |
| 9 | [Jämföra två andelar (A/B-test)](09_tva_andelar/09_tva_andelar.ipynb) | z-test för två andelar, signifikans kontra praktisk betydelse |
| 10 | [Typ I- och typ II-fel](10_typ1_typ2_fel/10_typ1_typ2_fel.ipynb) | Falska larm, missade effekter, styrka och stickprovsstorlek |

### Del D – Samband
| # | Notebook | Du lär dig |
|---|---|---|
| 11 | [Korstabeller och chi-två-test](11_korstabeller_chi2/11_korstabeller_chi2.ipynb) | Samband mellan två kvalitativa variabler |
| 12 | [Korrelation](12_korrelation/12_korrelation.ipynb) | Spridningsdiagram, Pearsons r, korrelation kontra kausalitet |
| 13 | [Enkel linjär regression](13_linjar_regression/13_linjar_regression.ipynb) | Minsta kvadrat-metoden, tolkning, R², residualer, extrapolering |

### Del E – Läsa och granska
| # | Notebook | Du lär dig |
|---|---|---|
| 14 | [Från Python till R](14_python_och_r/14_python_och_r.ipynb) | Läsa R-utskrifter och para ihop dem med det du gjort i Python |
| 15 | [Kritisk värdering av slutsatser](15_kritisk_vardering/15_kritisk_vardering.ipynb) | Checklista, många tester, p-hackning och en avslutande granskningsuppgift |

## Kom igång

Från projektets rot:

```
python -m venv .venv
.venv\Scripts\activate          (macOS/Linux: source .venv/bin/activate)
pip install -r requirements.txt
```

Öppna sedan `course/01_verktygen/01_verktygen.ipynb` i VS Code eller Jupyter och välj `.venv` som kernel.

## Tips

- **Kör cellerna uppifrån och ned.** Senare celler bygger på tidigare. Om något blir konstigt: *Restart & Run All*.
- **Varje notebook är fristående.** All data skapas i notebooken, så du kan börja om eller hoppa tillbaka när du vill.
- **Ändra hejvilt.** Du kan inte förstöra något. I värsta fall hämtar du tillbaka originalet med `git checkout -- <fil>`.
- **Slumpen och dina resultat.** Notebookarna använder ett fast frö, så en körning uppifrån och ned ger alltid samma tal.
  Kör du om en enskild cell flera gånger får du nya slumptal. Det är avsiktligt i labbarna.
- **Koden** förklaras bibliotek för bibliotek i [`statistik_kodguide.md`](../statistik_kodguide.md).
