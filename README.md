# Proiect Econometrie Avansată — Predicția Bolii Cardiace

Proiect de econometrie aplicată care folosește **regresia logistică** (binară și multinomială) împreună cu modele de **machine learning** (Random Forest, Gradient Boosting) pentru a prezice prezența și severitatea bolii cardiace, pornind de la caracteristici clinice ale pacienților.

**Sursa datelor:** [Heart Disease UCI Dataset — Kaggle](https://www.kaggle.com/datasets/redwankarimsony/heart-disease-data)

---

## Obiectivul proiectului

Construirea unui sistem de predicție care răspunde la două întrebări clinice distincte:

| Parte | Întrebare | Target | Tip problemă |
|---|---|---|---|
| **Partea I** | Are pacientul boală cardiacă? | 0 / 1 | Clasificare binară |
| **Partea II** | Cât de severă este boala? | 0, 1, 2, 3, 4 | Clasificare multinomială |

În ambele părți, regresia logistică (modelul principal econometric) este comparată cu **Random Forest** și **Gradient Boosting** pentru a evidenția trade-off-ul dintre **interpretabilitate** și **performanță predictivă**.

---

## Structura proiectului
Proiect Econometrie Avansata.ipynb   # Notebook principal cu toată analiza
heart_disease_uci.csv             # Dataset (920 pacienți, 16 variabile)
requirements.txt                   # Dependențe Python (versiuni exacte)
README.md                          # Acest fișier
.gitignore                         # Fișiere/foldere excluse din git
---

## Conținutul notebook-ului

Notebook-ul este structurat conform cerințelor proiectului și acoperă următoarele etape:

1. **Importuri și configurare globală**
2. **Încărcarea datelor** — citire CSV, redenumire coloane
3. **Analiza Exploratorie a Datelor (EDA)** — distribuții, corelații, vizualizări
4. **Tratarea valorilor lipsă** — strategie diferențiată (mediana / modul / categorie nouă)
5. **Feature Engineering** — variabile derivate (colesterol_ridicat, scor_risc)
6. **Label Encoding & Standardizare**
7. **Partea I — Clasificare binară**
   - Regresie logistică cu `statsmodels` (summary complet)
   - Verificare multicoliniaritate cu VIF
   - Evaluare pe test set (Accuracy, Precision, Recall, F1, ROC AUC)
   - Comparație cu Random Forest și Gradient Boosting
   - Analiza erorilor
   - Interpretarea coeficienților (log-odds, odds ratios)
8. **Partea II — Clasificare multinomială**
   - Regresie logistică multinomială
   - Comparație cu modele ML
9. **Concluzii finale**

---

## Cum se rulează proiectul

### Cerințe preliminare

- **Python 3.10 sau mai recent** ([descarcă de aici](https://www.python.org/downloads/))
- **Git** (pentru clonare) ([descarcă de aici](https://git-scm.com/downloads))
- **PyCharm**, **VS Code** sau **Jupyter Notebook** (pentru deschiderea fișierului `.ipynb`)

### Pas 1 — Clonează repository-ul

```bash
git clone https://github.com/ruximarian/Proiect-Econometrie-Avansata.git
cd Proiect-Econometrie-Avansata
```

### Pas 2 — Creează un virtual environment

**Pe Windows (PowerShell):**
```powershell
python -m venv .venv
.\.venv\Scripts\activate
```

**Pe Mac/Linux:**
```bash
python -m venv .venv
source .venv/bin/activate
```

După activare, prompt-ul terminalului va începe cu `(.venv)`.

### Pas 3 — Instalează dependențele

```bash
pip install -r requirements.txt
```

Durează aproximativ 2-3 minute. Sunt instalate toate librăriile necesare cu versiunile exacte folosite în dezvoltare.

### Pas 4 — Deschide notebook-ul

**În PyCharm:**
1. `File → Open` → selectează folderul proiectului
2. Configurează interpretatorul: `File → Settings → Project → Python Interpreter` → alege `.venv\Scripts\python.exe`
3. Deschide `Proiect Econometrie Avansata.ipynb`

**În VS Code:**
1. Deschide folderul proiectului
2. Deschide notebook-ul
3. În colțul dreapta-sus, selectează kernel-ul `.venv`

**În Jupyter Notebook:**
```bash
jupyter notebook
```
Apoi navighează la `ECONOMETRIE_FINAL_GATAAAA.ipynb` și deschide-l.

### Pas 5 — Rulează notebook-ul

Click pe **Run All** (sau `Kernel → Restart & Run All`). Toate celulele se vor executa secvențial.

---

## Tehnologii folosite

| Bibliotecă | Versiune | Utilizare |
|---|---|---|
| `pandas` | 3.0.3 | Manipulare date tabelare |
| `numpy` | 2.4.6 | Operații numerice |
| `matplotlib` | 3.10.9 | Vizualizări de bază |
| `seaborn` | 0.13.2 | Vizualizări statistice |
| `scikit-learn` | 1.8.0 | Modele ML (Random Forest, Gradient Boosting) |
| `statsmodels` | 0.14.6 | Regresie logistică cu summary statistic |
| `jupyter` | 1.1.1 | Interfață notebook |

---

## Rezultate principale

### Partea I — Clasificare binară

| Model | Accuracy | F1 | ROC AUC |
|---|---|---|---|
| Random Forest | 0.848 | **0.864** | **0.931** |
| Gradient Boosting | 0.832 | 0.852 | 0.912 |
| Regresie Logistică | 0.815 | 0.830 | 0.905 |

**Cei mai puternici predictori** (după magnitudinea coeficienților, p < 0.01):
1. `major_vessels` — numărul vaselor afectate la fluoroscopie (OR = 2.26)
2. `st_depression` — depresia segmentului ST la efort (OR = 1.96)
3. `thalassemia` — rezultatul testului de talasemie (OR = 1.86)
4. `colesterol_ridicat` — colesterol peste 240 mg/dl (OR = 1.82)
5. `sex` — bărbații au risc mai mare (OR = 1.73)

### Partea II — Clasificare multinomială

Performanța este mai slabă pe multiclasă (Accuracy ≈ 48%, F1 macro ≈ 0.34), confirmând că diferențierea între gradele de severitate 1-4 este intrinsec dificilă din cauza claselor dezechilibrate (clasa 4 are doar 28 de pacienți).

---

## Concluzii

- **Regresia logistică binară** este modelul cel mai potrivit pentru screening clinic: oferă interpretabilitate (odds ratios cu semnificație clinică) și performanță foarte bună (AUC > 0.90).
- **Modelele ML** câștigă marginal la performanță predictivă, dar pierd din transparență — compromis acceptabil doar dacă interpretabilitatea nu e prioritară.
- **Indicatorii de efort** (`st_depression`, `max_heart_rate`) sunt mai relevanți clinic decât valorile statice (`cholesterol`, `resting_bp`).
- **Pacienții asimptomatici** au paradoxal cel mai mare risc — confirmă fenomenul „bolii tăcute" descris în literatura medicală.

## Notă

Acest proiect este realizat în scop educațional pentru cursul de Econometrie Avansată. Modelul nu este destinat utilizării clinice reale fără validare suplimentară pe date contemporane și aprobare medicală.
