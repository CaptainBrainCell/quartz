## 🧩 Prehľad typov Text Splitterov v n8n:

### 1. **Character Text Splitter**

🔹 **Jednoduchý splitter podľa znaku.**  
Rozdelí text podľa určitého znaku (napr. nový riadok `\n`, medzera alebo bodka).

#### ⚙️ Parametre:

- **Separator** – znak(y), podľa ktorých sa text delí (napr. `\n\n`, `.` ).
    
- **Chunk Size** – maximálna dĺžka jednej časti (napr. 500 znakov).
    
- **Chunk Overlap** – koľko znakov má byť na konci jednej časti a zároveň na začiatku ďalšej (pomáha udržať kontext).
    

✅ **Vhodný pre:**

- Základné delenie textu (napr. články, e-maily, logy).
    

---

### 2. **Recursive Character Text Splitter**

🔹 **Inteligentnejší splitter.**  
Funguje podobne ako Character Text Splitter, ale **rekurzívne skúša viaceré separátory** — najprv delí podľa väčších jednotiek (`\n\n`), potom menších (`.` , `,` ), až po znaky, aby čo najlepšie zapadol do limitu.

#### ⚙️ Parametre:

- **Chunk Size** a **Chunk Overlap** – rovnaké ako vyššie.
    
- Funguje bez explicitného zadania separátora — používa zoznam priorít.
    

✅ **Vhodný pre:**

- Rozdelenie dlhých textov pri zachovaní prirodzenej štruktúry (napr. paragrafy, vety).
    
- Ideálne pre LLM, kde je dôležitý **kontext**.
    

---

### 3. **Token Text Splitter**

🔹 **Splitovanie podľa počtu tokenov (nie znakov).**  
Toto je **najpresnejšie** pre LLM modely (napr. OpenAI), pretože rešpektuje počet tokenov, nie počet znakov.

#### ⚙️ Parametre:

- **Model** – vyberie sa tokenizer podľa modelu (napr. GPT-3.5, GPT-4).
    
- **Chunk Size** – maximálny počet tokenov na chunk.
    
- **Chunk Overlap** – počet tokenov, ktorý sa bude opakovať v ďalšom chunku.
    

✅ **Vhodný pre:**

- Práca s LLM, kde je dôležité neprekročiť tokenový limit (napr. 4096 tokenov).
    
- Veľké dokumenty, ktoré chceš "embednúť" alebo spracovať po častiach.
    

---

## 🔚 Zhrnutie rozdielov:

|Splitter|Delenie podľa|Inteligencia|Vhodné pre|
|---|---|---|---|
|Character Text Splitter|znaky|nízka|jednoduché rozdelenie textu|
|Recursive Character Splitter|viacero znakov|stredná|prirodzené rozdelenie (napr. vety)|
|Token Text Splitter|tokeny (LLM)|vysoká|LLM, embedovanie, token limity|