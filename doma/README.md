# SEMANTIC GAP · BASELINE

**Projekt:** Chceš myš? Slovník mezer  
**Autor:** Michaela Nováková  
**Fáze:** BASELINE bez MAPY VÝZNAMU  
**Model:** `Qwen/Qwen3-VL-2B-Instruct`

## Smysl první fáze

První fáze vytváří výchozí srovnávací materiál bez autorské MAPY VÝZNAMU. Model dostane pouze obecné zadání k neutrálnímu rozboru dodaného materiálu. Nedostává definice typu „dívka představuje řeč“, „zvíře představuje poruchu“ ani pravidla mezery a kruhové topologie.

To není opomenutí. Je to kontrolní základ, ke kterému bude možné později porovnat běh se stejným modelem a stejnými vstupy, ale s MAPOU VÝZNAMU.

## Jeden model, tři samostatné režimy

1. **OBRAZ** – pouze ilustrace. Text nakreslený uvnitř ilustrace zůstává součástí obrazového vstupu.
2. **TEXT** – pouze původní česká pohádka. Zachovává se obsah souboru včetně zalomení řádků a prázdných řádků.
3. **OBRAZ + TEXT** – ilustrace a samostatná pohádka jsou předány v jednom vstupu.

Každý režim má vlastní notebook a vlastní výstup. Model je ve všech třech případech stejný.

## Notebooky

- [01 · Myš – obraz](01_mys_obraz_baseline.ipynb)
- [02 · Myš – text](02_mys_text_baseline.ipynb)
- [03 · Myš – obraz + text](03_mys_obraz_text_baseline.ipynb)

Otevři notebook přes Google Colab: na stránce notebooku zvol **Open in Colab** nebo stáhni soubor a nahraj jej do Colabu. V každém notebooku spusť buňky postupně shora dolů.

## Vstupní soubory

Notebook vytvoří na Google Drive složku:

`MyDrive/SEMANTIC_GAP_BASELINE/inputs/`

Pro první kartu očekává:
- `mys.jpg` – ilustrace,
- `mys.txt` – pohádka v UTF-8.

Pokud soubor ve složce chybí, notebook nabídne jeho nahrání přímo z telefonu nebo počítače. U textu se zachovává původní řádkování.

## Co se ukládá

Na Google Drive vznikne:
- `results/<zvire>/<rezim>/run_<otisk>/embedding.npy` – vektor v NumPy formátu,
- `embedding.json` – stejný vektor pro pozdější načtení ve webové vizualizaci,
- `analysis.json` a `analysis_raw.txt` – strukturovaný a původní textový výstup modelu,
- `metadata.json` – model, režim, otisk vstupu, rozměr vektoru a nastavení,
- `worklog.jsonl` – časová posloupnost dokončených kroků,
- ZIP archiv v `exports/`.

Po dokončení notebook nabídne ZIP ke stažení. Průběžné checkpointy zůstávají na Google Drive, takže při přerušení lze navázat od uloženého kroku.

## Co znamená zdejší embedding

Notebook získává skrytou reprezentaci poslední vrstvy modelu a vytváří z ní normalizovaný průměrný vektor (mean pooling přes platné pozice sekvence). Jde o **experimentální pooled hidden-state reprezentaci modelu**, nikoli o specializovaný embeddingový výstup nebo důkaz sémantického porozumění.

Vektory lze porovnávat kosinovou podobností jako explorativní měření. Srovnání je smysluplné pouze při stejném modelu, verzi, vrstvě, způsobu poolingu a kompatibilním nastavení. Podobnost sama o sobě nedokazuje shodu významu.

## Co zatím baseline nedělá

- Nepoužívá autorskou MAPU VÝZNAMU.
- Nepředepisuje 12 perspektiv ani kruhovou topologii.
- Nevyžaduje od modelu, aby dívku nebo zvíře interpretoval podle autorských definic.
- Neprovádí přenos pravidel mezi zvířaty.
- Nemění ani netrénuje váhy modelu.

Tyto funkce patří do další fáze. Záměrně je nepřidáváme do baseline, abychom měli srovnatelný výchozí bod.

## Důležité technické upozornění

Model má přibližně 2 miliardy parametrů. GPU je výrazně vhodnější. Notebook automaticky použije dostupné GPU; pokud GPU není přiděleno, zkusí CPU. Na CPU může být načítání i výpočet pomalý a může narazit na limit RAM bezplatného Colabu. Bezplatné GPU Colab negarantuje.

Model se stahuje z Hugging Face při prvním spuštění. Notebook nevyžaduje OpenAI API ani placený API klíč.

## Odkazy

- [Oficiální model na Hugging Face](https://huggingface.co/Qwen/Qwen3-VL-2B-Instruct)
- [Dokumentace Qwen3-VL v Transformers](https://huggingface.co/docs/transformers/model_doc/qwen3_vl)
- [Hlavní rozcestník](index.html)
