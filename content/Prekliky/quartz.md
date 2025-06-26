**Quartz** je moderný, rýchly a minimalistický statický generátor webových stránok (SSG), zameraný najmä na tvorbu **digitálnych záhrad** (digital gardens), poznámok, blogov a osobných wiki. Využíva Markdown súbory ako zdroj obsahu a z nich generuje rýchle a prehľadné weby.

---

#### Kľúčové vlastnosti Quartz:

- **Digitálne záhrady a poznámky:** Ideálne pre ľudí, ktorí chcú organizovať svoje poznámky, myšlienky alebo blogovať vo formáte, ktorý umožňuje prepojenia medzi obsahom a postupné rozširovanie.
    
- **Rýchlosť:** Používa moderné nástroje a build pipeline (napr. esbuild) na rýchle generovanie stránok.
    
- **Markdown-first:** Obsah sa píše jednoducho v Markdown formáte, vrátane podpory matematických vzorcov, citácií, a rôznych rozšírení (každý súbor predstavuje stránku alebo článok).
    
- **Prehľadné rozhranie:** Výstupný web je elegantný a prispôsobiteľný, často s podporou tematických skinov a pluginov.
    
- **Podpora Git:** Využíva Git na sledovanie zmien obsahu, čo umožňuje napríklad zobrazovanie dátumu poslednej úpravy.
    
- **Lokálny server:** Quartz obsahuje aj jednoduchý development server na lokálne náhľady obsahu pred publikovaním.
    
- **Jednoduchá publikácia:** Výstup (statické HTML, CSS, JS) sa dá publikovať na hostingové platformy ako Netlify, Vercel, GitHub Pages a podobne.
    

---

#### Typický workflow s Quartz:

1. Napíšeš si obsah vo formáte Markdown v priečinku `content`.
    
2. Spustíš Quartz build (napr. `npm run quartz -- build`).
    
3. Quartz vygeneruje statické súbory do priečinka `public`.
    
4. Môžeš lokálne prehliadať web cez `npm run quartz -- build --serve`.
    
5. Výstup nahraješ na hosting (Netlify, GitHub Pages, atď.).