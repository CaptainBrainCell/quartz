


Na základe výstupov z predchádzajúcich AI agentov (firma + hovor) priprav obchodný briefing pre obchodníka.

Predtým ako začneš generovať odpoveď, **použi Think Tool** na hlboké zamyslenie sa nad vstupmi. Zamysli sa nad:
- Aký typ klienta je táto firma?
- Na čo by sa mal obchodník pripraviť v rozhovore?
- Aké námietky by mohli nastať?
- Čo by som ako obchodník určite chcel vedieť pred stretnutím?

Think Tool slúži na zaznamenanie tvojho uvažovania – použi ho na vytvorenie súvislých poznámok, ktoré ti pomôžu pri následnom generovaní odpovede.

---

**Vstupy:**

- Výstup o firme: {{ $json.info }}
- Výstup z hovoru: {{ $('SET CONTENT').item.json.content }}

---

**Tvoja úloha:**

Na základe týchto dvoch vstupov vytvor komplexný výstup rozdelený do častí:

a) Analýza komodity podnikania firmy  
b) Analýza firmy (história, tržby, počet zam.)  
c) Analýza osobnosti konateľa z hovoru  
d) Analýza potrieb a predpokladaných námietok  

+ **Sekcia pre obchodníka**: Odporúčania na čo sa zamerať, čo ponúknuť, aký môže byť ďalší postup. Použi jazyk priamo pre obchodníka, praktický a vecný.

## príklad outputu:
🏢 Firma: IBV Dvorníky s. r. o.
📌 Komodita: Kúpa a predaj vlastných nehnuteľností, výstavba obytných a neobytných budov

🛠️ SK NACE: 68100 – Kúpa a predaj vlastných nehnuteľností
41209 – Výstavba obytných a neobytných budov i. n.

💼 Analýza firmy:

    História a vývoj: Firma založená v roku 2010, stabilne pôsobí na trhu realít v Bratislave a okolí.

    Tržby a zamestnanci: Ročné tržby cca 5 miliónov EUR, zamestnáva približne 35 ľudí.

    Kľúčové fakty: Firma sa zameriava na B2C segment a využíva lokálnych dodávateľov. V posledných rokoch stabilný rast napriek legislatívnym výzvam.

🧑‍💼 Osobnostná analýza konateľa:

    Konateľ prejavuje pragmatický a analytický prístup, je otvorený novým riešeniam, no kladie dôraz na overené postupy.

    Rýchlo reaguje na otázky, preferuje jasné fakty a konkrétne riešenia.

🎯 Potreby a námietky klienta:

    Potrebuje spoľahlivé informácie o trhu a digitálne nástroje na efektívnejšie riadenie projektov.

    Môže mať obavy z legislatívnych zmien a možných právnych komplikácií.

💡 Odporúčania pre obchodníka:

    Zamerať sa na diskusiu o aktuálnych trendoch v realitách a stavebníctve.

    Ponúknuť riešenia zlepšujúce digitalizáciu a správu projektov.

    Pripraviť sa na otázky ohľadom legislatívy a právnych rizík.

!!Ako output napis len to co mas, nic ine okolo!!