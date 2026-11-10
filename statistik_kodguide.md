# Kodguide till `statistik_grunder.ipynb`

Den här guiden förklarar **all kod** som används i notebooken `statistik_grunder.ipynb`, bibliotek för bibliotek.
Läs ett avsnitt, kör exemplen i en egen cell och gör uppgiften under **Prova**.

```python
import numpy as np                 # räkna med tal och listor av tal, slumptal
import pandas as pd                # tabeller (som Excel i Python)
import matplotlib.pyplot as plt    # diagram
from scipy import stats            # statistiska fördelningar och tester
```

`np`, `pd` och `plt` är kortnamn (alias) som alla använder. Därför skriver man `np.sqrt(...)` i stället för `numpy.sqrt(...)`.

---

## 0. Python som dyker upp i koden

### f-strängar och formatering
En f-sträng (`f"..."`) låter dig stoppa in värden inom `{ }`. Efter `:` anger du hur talet ska visas.

```python
x = 3.14159
print(f"{x:.2f}")     # 3.14      – 2 decimaler
print(f"{0.355:.1%}") # 35.5%     – som procent med 1 decimal
print(f"{0.000012:.2g}")  # 1.2e-05 – 2 värdesiffror (bra för små p-värden)
print(f"{5:>4}")      # '   5'    – högerjustera på 4 tecken
```

### Övrigt
| Kod | Betyder |
|---|---|
| `10_000` | Samma som `10000`. Understrecket är bara för läsbarheten. |
| `[expression for _ in range(500)]` | **List comprehension:** gör en lista med 500 värden. `_` betyder "variabeln används inte". |
| `for ax, n in zip(axes, [10, 100])` | `zip` går igenom två listor parvis. |
| `for name, y in dictionary.items()` | Gå igenom nycklar och värden i en dictionary. |
| `a, b = function()` | **Uppackning:** funktionen returnerar flera värden som läggs i var sin variabel. |
| `x**2` | Upphöjt till 2. |
| `misses += not hit` | `True` räknas som 1 och `False` som 0, så det här räknar antalet missar. |
| `display(table)` | Jupyter-funktion som visar en tabell snyggt mitt i en cell. Utan `display` visar Jupyter bara värdet av cellens **sista uttryck**. |
| `len(x)` | Antal element i en lista, array eller tabell (antal rader). |
| `range(140, 211, 10)` | Talen 140, 150 … 210: start, stopp (ingår inte) och steg. |
| `axes[1:]` | **Slicing:** alla element från och med index 1, alltså alla utom det första. |
| `m - h <= mu <= m + h` | **Kedjad jämförelse:** sant om `mu` ligger mellan `m - h` och `m + h`. |
| `"tab:blue" if hit else "red"` | **Villkorsuttryck:** ger det första värdet om `hit` är sant, annars det andra. |

### if och else
```python
alpha = 0.05
if p_value < alpha:
    print("Förkasta H0")          # körs om villkoret är sant
else:
    print("H0 kan inte förkastas")   # körs annars
```
Indraget (fyra mellanslag) avgör vilka rader som hör till `if` och `else`.

### Egna funktioner med def
```python
def share_rejected(true_difference, n_per_group, num_trials=2000, alpha=0.05):
    ...
    return (p_values < alpha).mean()

share_rejected(0.5, 30)              # num_trials = 2000 och alpha = 0.05 används
share_rejected(0.5, 30, alpha=0.01)   # ändra bara alpha
```
`def` skapar en funktion och `return` anger vad den ger tillbaka. Argument med `=` har ett **standardvärde**
och behöver bara anges när man vill ha något annat.

**Prova:** skriv ut `2/3` som procent med 0 decimaler.

---

## 1. NumPy

NumPy arbetar med **arrayer**: listor av tal där man kan räkna på alla värden på en gång.

```python
a = np.array([1, 2, 3])
a * 10          # array([10, 20, 30]) – ingen loop behövs
a.mean()        # 2.0
```

### Slumptal med en generator
```python
rng = np.random.default_rng(42)
```
`rng` är en slumptalsgenerator. Talet `42` är ett **frö** (seed): samma frö ger samma "slump" varje gång, så resultaten går att upprepa.

| Funktion | Ger | Exempel i notebooken |
|---|---|---|
| `rng.normal(mean, sd, size)` | Normalfördelade tal | längd: `rng.normal(172, 9, N)` |
| `rng.lognormal(mu, sigma, size)` | Högersneda positiva tal | inkomst |
| `rng.poisson(mean, size)` | Heltal 0, 1, 2 … (antal händelser) | antal barn |
| `rng.uniform(min, max, size)` | Jämnt fördelade tal i ett intervall | studietimmar |
| `rng.random(size)` | Tal mellan 0 och 1 | används för att slumpa vem som svarar |
| `rng.choice(values, size, p=[...])` | Slumpvis val ur en lista, `p` = sannolikheter | utbildning, kanal |
| `rng.choice(array, n, replace=False)` | Dra *n* värden **utan återläggning** | ett stickprov |

`size` kan också vara en **form**: `rng.normal(0, 1, (2000, 30))` ger en tabell med 2000 rader och 30 kolumner, alltså 2000 stickprov på en gång.

### Matematik
| Funktion | Gör |
|---|---|
| `np.sqrt(x)` | Kvadratrot |
| `np.log(x)` | Naturlig logaritm |
| `np.abs(x)` | Absolutbelopp (tar bort minustecken) |
| `np.round(x, 3)` | Avrunda till 3 decimaler |
| `np.sort(x)` | Sortera |
| `np.clip(x, min, max)` | Begränsa värden till ett intervall, t.ex. provpoäng 0–100 |
| `np.linspace(start, stop, num)` | Jämnt fördelade punkter. Används som x-axel när man ritar kurvor. |
| `np.std(x)` | Standardavvikelse |

### Två viktiga detaljer
**Villkor ger True/False, och medelvärdet av dem blir en andel:**
```python
p_values = np.array([0.01, 0.3, 0.04, 0.8])
p_values < 0.05            # array([ True, False,  True, False])
(p_values < 0.05).mean()   # 0.5  – andelen som är under 0,05
(p_values < 0.05).sum()    # 2    – antalet
```

**`ddof` – dela med n eller n − 1:**
`np.std(x)` delar med *n*. För stickprovets standardavvikelse *s* ska man dela med *n − 1*, och då skriver man `x.std(ddof=1)`.
(pandas `.std()` använder *n − 1* automatiskt.)

ddof betyder "delta degrees of freedom" – hur många frihetsgrader man ska dra av när man beräknar standardavvikelsen.

**Prova:** dra 1000 tal med `rng.normal(0, 1, 1000)` och räkna ut hur stor andel som är större än 2.

---

## 2. pandas

pandas har två huvudtyper:
- **Series:** en kolumn med värden.
- **DataFrame:** en tabell med flera kolumner.

### Skapa och titta
```python
df = pd.DataFrame({"name": ["Ada", "Bo"], "height": [165, 180], "income": [30000, 20000]})
df.head()      # de första 5 raderna
df.dtypes      # datatyp för varje kolumn
```
`df` är det vanliga namnet på en DataFrame. **Obs:** i scipy heter argumentet för *frihetsgrader* också `df`
(degrees of freedom), t.ex. `stats.t.ppf(0.975, df=99)`. Det är två helt olika saker. I notebooken heter
tabellerna därför `population`, `sample` och `survey`, och egna variabler för frihetsgrader heter `dof`.

### Välja data
| Kod | Ger |
|---|---|
| `df["height"]` | En kolumn (Series) |
| `df[["height", "income"]]` | Flera kolumner (DataFrame). Lägg märke till de dubbla hakparenteserna. |
| `df[df["height"] > 170]` | **Filtrera:** bara raderna där villkoret är sant |
| `df[responds]` | Filtrera med en färdig True/False-array |

### Beräkningar
| Metod | Gör |
|---|---|
| `.mean()`, `.median()` | Medelvärde, median |
| `.std()` | Standardavvikelse (delar med *n − 1*) |
| `.min()`, `.max()`, `.sum()` | Minsta, största, summa |
| `.abs()` | Absolutbelopp |
| `.round(2)` | Avrunda |
| `.describe()` | Sammanfattning: antal, medel, std, min, kvartiler, max |
| `.quantile([0.25, 0.75])` | Kvartilerna Q1 och Q3 (kan packas upp: `q1, q3 = ...`) |
| `.mode()[0]` | Typvärde. `.mode()` ger en Series eftersom flera värden kan vara lika vanliga, och `[0]` tar det första. |
| `.value_counts()` | **Frekvenstabell:** hur många gånger varje värde förekommer, vanligast först |
| `.sort_index()` | Sortera efter index (radetiketterna), t.ex. klasserna i ordning |
| `.reindex(new_order)` | Ordna raderna i en egen ordning, t.ex. `["Grundskola", "Gymnasium", "Högskola"]` |
| `.to_numpy()` | Gör om till en NumPy-array |

Andel med ett villkor fungerar som i NumPy:
```python
(population["education"] == "Högskola").mean()   # andel med högskoleutbildning
```

### Stickprov
```python
df.sample(n=100, random_state=1)   # 100 slumpvis valda rader (OSU), random_state = frö
```

### Klassindelning
```python
pd.cut(df["height"], bins=range(140, 211, 10))
```
Delar in värdena i intervall: (140, 150], (150, 160] … Hakparentesen `]` betyder att gränsen ingår, och den runda `(` att den inte gör det.
Med `right=False` blir klasserna i stället [140, 150), [150, 160) …
Kombinera med `.value_counts().sort_index()` för att få en frekvenstabell.

### Korstabell
```python
pd.crosstab(df["age_group"], df["channel"])                       # antal per kombination
pd.crosstab(..., margins=True, margins_name="Totalt")              # med rad- och kolumnsummor
pd.crosstab(..., normalize="index")                                # radprocent (varje rad summerar till 1)
```

### Snabbdiagram direkt från pandas
```python
frequency.plot(kind="bar", rot=0, title="Utbildning")   # stapeldiagram, rot=0 ger raka etiketter
```

**Prova:** gör en frekvenstabell över `num_children` i `sample`, och räkna ut andelen som har fler än 2 barn.

---

## 3. matplotlib.pyplot

### Två sätt att rita
**1. Direkt med `plt`:** enklast när du har ett diagram.
```python
plt.scatter(x, y)
plt.xlabel("x"); plt.ylabel("y"); plt.title("Titel")
plt.show()
```

**2. Med `fig` och `ax`:** när du vill ha flera diagram bredvid varandra.
```python
fig, axes = plt.subplots(1, 3, figsize=(12, 3), sharex=True)   # 1 rad, 3 diagram
axes[0].hist(data)              # rita i första diagrammet
axes[0].set_title("Titel")      # med ax heter det set_title, set_xlabel …
fig.suptitle("Gemensam rubrik")
plt.tight_layout()              # justera så att inget överlappar
plt.show()
```
`figsize=(width, height)` anges i tum. `sharex=True` ger samma x-axel i alla diagram.

Sätt alltid axelrubriker: `plt.xlabel(...)` och `plt.ylabel(...)`, eller `ax.set_xlabel(...)` och `ax.set_ylabel(...)`.
`ax.set_xticks([])` tar bort skalstrecken på x-axeln.

### Diagramtyper
| Funktion | Diagram |
|---|---|
| `hist(data, bins=20)` | Histogram. `bins` = antal klasser. |
| `plot(x, y)` | Linje (t.ex. en täthetskurva eller regressionslinje) |
| `scatter(x, y)` | Spridningsdiagram |
| `boxplot(a)` / `boxplot([a, b])` | Lådagram för en eller flera grupper |
| `plot([i, i], [low, high])` | Ett lodrätt streck från `low` till `high` vid x = i (används för konfidensintervallen) |
| `axvline(x)` / `axhline(y)` | Lodrät / vågrät referenslinje |
| `fill_between(x, y, where=condition)` | Fyll ytan under kurvan där villkoret är sant (används för p-värdet) |

### Vanliga inställningar
| Argument | Gör |
|---|---|
| `color="red"` | Färg |
| `linestyle="--"` | Streckad linje |
| `"k--"` | Kortform: svart (`k`) streckad (`--`) linje |
| `label="μ"` + `legend()` | Namn i en teckenförklaring |
| `alpha=0.4` | Genomskinlighet (0 = osynlig, 1 = helt täckande) |
| `density=True` | Histogrammet visar täthet i stället för antal, så att en fördelningskurva passar ovanpå |
| `edgecolor="white"` | Kantfärg på staplarna |
| `s=8` | Punktstorlek i `scatter` |
| `plt.xticks([1, 2], ["A", "B"])` | Egna etiketter på x-axeln |

**Prova:** rita ett histogram över `population["income"]` med 30 klasser och en röd lodrät linje vid medianen.

---

## 4. scipy.stats

### Fördelningar: `stats.norm` och `stats.t`
`stats.norm` är normalfördelningen och `stats.t` är t-fördelningen (som också behöver `df`, frihetsgrader).
Båda har samma metoder:

| Metod | Svarar på | Exempel |
|---|---|---|
| `.pdf(x)` | Hur hög är kurvan vid x? (används för att **rita** fördelningen) | `stats.norm.pdf(x, mu, sigma)` |
| `.cdf(x)` | Sannolikheten att få **högst** x (arean till vänster) | `stats.norm.cdf(1.96)` → 0,975 |
| `.sf(x)` | Sannolikheten att få **minst** x (arean till höger) = 1 − cdf | `stats.norm.sf(1.96)` → 0,025 |
| `.ppf(q)` | **Omvänt:** vilket x har arean q till vänster? (används för **kritiska värden**) | `stats.norm.ppf(0.975)` → 1,96 |
| `.interval(0.95, ...)` | Intervallet som innehåller de mittersta 95 % | konfidensintervall |

Två mönster som återkommer:
```python
t_crit = stats.t.ppf(0.975, df=n - 1)            # kritiskt värde för 95 % KI (2,5 % i varje svans)
p_value = 2 * stats.t.sf(abs(t_obs), df=n - 1)   # dubbelsidigt p-värde: arean i båda svansarna
```

`stats.norm.pdf(x, mu, sigma)`: utan `mu` och `sigma` används standardnormalfördelningen (medel 0, sd 1).

### Statistiska tester
| Funktion | Test | Returnerar |
|---|---|---|
| `stats.ttest_1samp(data, mu_0)` | t-test: ett medelvärde mot ett givet värde | `.statistic` (t), `.pvalue` |
| `stats.ttest_ind(a, b, equal_var=False)` | t-test: två oberoende grupper (Welch) | `.statistic`, `.pvalue` |
| `stats.chi2_contingency(table)` | Chi-två-test för samband | `chi2, p, dof, expected` (eller `.statistic`, `.pvalue`) |
| `stats.pearsonr(x, y)` | Pearsons korrelation | `r, p` (kan packas upp) |
| `stats.linregress(x, y)` | Enkel linjär regression | `.intercept`, `.slope`, `.rvalue`, `.pvalue` |

Användbara argument:
- `alternative="two-sided"` (standard), `"greater"` eller `"less"`: dubbelsidigt eller ensidigt test.
- `equal_var=False`: anta inte att grupperna har samma varians (Welchs t-test, säkrare).
- `axis=1`: kör ett test **per rad** i en 2D-array. Så körs 2000 tester på en gång i simuleringarna.
- `correction=False` i `chi2_contingency`: stäng av kontinuitetskorrektionen, som annars används automatiskt för tabeller med 2 × 2 celler.

Resultatet från ett t-test kan också ge ett konfidensintervall:
```python
res = stats.ttest_ind(a, b, equal_var=False)
ci = res.confidence_interval(0.95)   # KI för skillnaden mellan medelvärdena
ci.low, ci.high                      # nedre och övre gräns
```

Resultat kan läsas på två sätt:
```python
res = stats.ttest_1samp(height, 170)
res.pvalue                 # med namn

r, p = stats.pearsonr(x, y)          # med uppackning
r = stats.pearsonr(x, y)[0]          # bara första värdet

reg = stats.linregress(x, y)
reg.rvalue**2              # R² (förklaringsgrad)
reg.stderr                 # lutningens medelfel
```

**Prova:** räkna ut `stats.t.ppf(0.975, df=dof)` för 2, 20 och 200 frihetsgrader. Vad händer med t\* när antalet frihetsgrader växer, och varför?

---

## 5. Mönster som återkommer i notebooken

| Mönster | Exempel | Idé |
|---|---|---|
| **Andel = medelvärde av True/False** | `(p_values < 0.05).mean()` | True = 1, False = 0 |
| **Simulera många stickprov i en loop** | `[rng.choice(heights, n, replace=False).mean() for _ in range(500)]` | Se hur en skattning varierar |
| **Simulera utan loop** | `rng.normal(0, 1, (2000, 30))` + `axis=1` | Varje rad är ett stickprov, vilket går snabbare |
| **Räkna själv, kontrollera med scipy** | Manuellt t-test, sedan `stats.ttest_1samp` | Förstå formeln *och* se att den stämmer |
| **Skriv slutsatsen i ord** | `if p_value < alpha: print("förkasta H0 …")` | Ett test är inte klart förrän slutsatsen är formulerad |
| **Rita kurva över histogram** | `hist(..., density=True)` + `plot(x, stats.norm.pdf(x, ...))` | Jämför data med en teoretisk fördelning |
