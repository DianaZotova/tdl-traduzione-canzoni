# tdl-traduzione-canzoni
# Tradurre con i modelli: analisi comparativa tra NMT e LLM sui testi musicali italiani (IT → RU)

**Corso:** Tecnologie dei dati e del linguaggio (Prof. Alfio Ferrara)  
**Traccia:** 14. Tradurre con i modelli  
**Autore:** [  Diana e Zotova – 73681A]

---

## 1. Tema e Domanda di ricerca

Il progetto esplora la traduzione automatica **dall'italiano al russo (IT → RU)** applicata ai testi della musica contemporanea italiana.

**Domanda di ricerca:**  
*Quali errori commettono i modelli automatici nella traduzione di testi musicali moderni appartenenti a generi diversi, in quali condizioni modificano il tono o il registro dell'originale, e quale ruolo conserva l'intervento umano?*

## 2. Ipotesi di ricerca

La traduzione automatica dei testi musicali presenta criticità specifiche legate alla presenza di tratti marcati (slang, espressioni idiomatiche, metafore), riferimenti culturali e vincoli ritmici.  
L'ipotesi di partenza è che:
1. Un modello neurale dedicato (**NMT**) e una decodifica deterministica (**LLM con `temperature = 0`**, *greedy decoding*) tendano a produrre calchi sintattici e lessicali, appiattendo il tono e perdendo le sfumature culturali.
2. Un **Large Language Model (LLM)** guidato da un prompt contestuale e da un campionamento controllato (**`temperature = 0.7`**) riesca a preservare meglio il registro espressivo, pur rischiando instabilità morfosintattiche in russo.
3. Il **post-editing umano** (distinguendo tra correzione superficiale e revisione profonda) resti indispensabile per negoziare stile, cultura e naturalezza.

## 3. Obiettivi

- Confrontare la resa di un sistema neurale dedicato (**Meta `NLLB-200`**) e di un LLM (**`Qwen2.5-1.5B-Instruct`** in due configurazioni: `T = 0.0` e `T = 0.7`) sulla coppia **IT → RU**.
- Valutare l'impatto del **genere musicale** e dei tratti marcati sulla qualità della traduzione.
- Classificare gli errori sistematici secondo una griglia tipologica (calchi sintattici, sviste lessicali, perdita di registro, riferimenti culturali, omissioni e ipertraduzione).
- Osservare la differenza tra *light post-editing* (correttezza grammaticale) e *full post-editing* (adattamento stilistico e culturale).

## 4. Il Corpus Parallelo e Fonti dei Dati

Il corpus parallelo (`songs_dataset.csv`, **186 versi** complessivi) è stato costruito selezionando quattro brani italiani contemporanei appartenenti a generi distinti, al fine di osservare l'ampiezza delle variazioni stilistiche:

1. **Pop Dance:** *Direzione: la vita* – **Annalisa** (63 versi)  
   - Testo originale (IT): [Genius](https://genius.com/Annalisa-direzione-la-vita-lyrics)  
   - Traduzione umana di riferimento (RU): [LyricsTranslate](https://lyricstranslate.com/en/direzione-la-vita-napravlenie-zhizn.html)
2. **R&B Pop:** *Barrio* – **Mahmood** (40 versi)  
   - Testo originale (IT): [Genius](https://genius.com/Mahmood-barrio-lyrics)  
   - Traduzione umana di riferimento (RU): [Amalgama-Lab](https://www.amalgama-lab.com/songs/m/mahmood/barrio.html)
3. **Pop Rock:** *L’altra dimensione* – **Måneskin** (35 versi)  
   - Testo originale (IT): [Genius](https://genius.com/15658456)  
   - Traduzione umana di riferimento (RU): [Amalgama-Lab](https://www.amalgama-lab.com/songs/m/maneskin/l_altra_dimensione.html)
4. **Rap Pop:** *Superclassico* – **Ernia** (48 versi)  
   - Testo originale (IT): [Genius](https://genius.com/Ernia-superclassico-lyrics)  
   - Traduzione umana di riferimento (RU): [Lyrsense](https://lyrsense.com/ernia/superclassico)

## 5. Metodologia e Criteri di Verifica

- **Generazione automatica in Python:** traduzione dei 186 versi tramite `Meta NLLB-200-distilled-600M` e `Qwen2.5-1.5B-Instruct` (`T = 0.0` vs `T = 0.7` con prompt contestuale).
- **Metriche quantitative:** valutazione rispetto al riferimento umano mediante **BLEU**, **chrF** (scelto per la ricca morfologia flessiva del russo) e **BERTScore (F1)**, affiancate dall'analisi della lunghezza media e della ricchezza lessicale (**Type-Token Ratio – TTR**).
- **Analisi qualitativa e Post-Editing:** annotazione manuale degli errori ricorrenti e confronto tra *light* e *full post-editing* su casi studio rappresentativi.

## 6. Struttura del Repository

- `*.ipynb`: Notebook Jupyter/Colab con codice Python riproducibile, visualizzazioni grafiche e analisi critica dei risultati.
- `songs_dataset.csv`: Dataset parallelo di partenza (186 segmenti).
- `risultati_traduzioni_it_ru.csv`: Output completi generati dai tre sistemi confrontati.
