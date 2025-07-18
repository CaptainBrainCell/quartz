### Zadanie:

Automatizovaný AI asistent pre investorov do nehnuteľností.

**Požiadavky:**

- **Vstup**: Kritériá investície, rozpočet, preferovaná lokalita
- **AI Úlohy**:
    - Scraping realitných portálov (Sreality, Bezrealitky)
    - Analýza makro/mikro lokácie (doprava, školy, kriminalita)
    - Predpoveď rastu cien na základe historických údajov
    - Finančné modelovanie (ROI, cash flow, dane)
    - Analýza rizík (legislatíva, demografické trendy)
- **Automatizácia**: Denné sledovanie nových ponúk, cenové upozornenia, konkurenčná analýza
- **Výstup**: Personalizované investičné odporúčania, PDF správy, Telegram notifikácie
----------------------------------
Toto riesenie ma 2 casti -> **MAIN AGENT** :
![[125.png]]
**********
a **SEARCH AGENT**
![[133.png]]
******************************

Riesenie: Zacal som tym, ze som si pridal on form submission trigger, v ktorom si zadame podmienky nehnutelnosti, ktore hladame.![[124.png]]

Nasledne sa tieto hodnoty zapisu a posli search agentovi, ktory si tieto hodnoty zapise a podla url templatov na nehnutelnosti, topreality, zoznamrealit a bazos vytvori nase custom url s poziadavkamy, ktore sme si zadali my. [[ulr_script]]. Nasledne sa nam tieto urlka zapisu a pokracuju dalej do jednotlivych casti, ktore sluzia na spracovanie danych stranok.
![[127.png]]

Kazda tato cast ma dalsie 2 casti. 
1. cast sluzi na scraping webu, naslende spracovanie md formatu a zapisanie do dokumentu.

![[128.png]]

Najskor tam mame loop node, ktory nam zabezpeci to, ze skusime kazdu jednu url, ktoru nam vygenerovalo v create page url, co bolo celkom tazke pretoze kazda jedna stranka si url tvori inym sposobom a musel som prist na to, ako to jednotlive stranky robili.  A to funguje tak, ze zoberie to jednu url, hodi nam ju do firecrawl api, ktore nam stranku scrapne, potom to prejde do code nodu, ktory obsah z web scrapu vyfiltruje len na tie najdolezitejsie casti [[filter]] a skontroluje, ci url, ktoru sme zadali existuje alebo nie. Ak hej tak obsah nam zapise do google dokumentu a ak nie tak nam to posle spravu na telegram ze dana stranka bola spracovana. 

A 2. cast

![[129.png]]

Ktora nam robi to, ze obsah dokumentu, do ktoreho sa zapisuje vysledok prvej casti sa najskor vymaze, nech sa to neskor neprepisuje a nemixuje lebo to by nam pokazilo nas vystup. To funguje tak, ze najskor si najdeme najvacsi index v dokumente (get content + find endindex), od toho odpocitame 1 pretoze ked to deletujeme tak vzdy musi byt najvacsi index mensi ako pocet indexov a nasledne to pomoco delete content nodu vymazeme. 

**Cize v skratke kazda jedna cast tohoto search agenta najskor zoberie dokument, kde sa to vsetko zapisuje, odstrani obsah aby sa to neskor neprepisovalo, scrapne hlavny web a podstranky, ak su tie ponuky na viacerych stranach a vyfiltrovane vysledky zapise do google documentu. ** 

	*tieto casti su avsak vseky inak nastavene pre potreby jednotlivych webov pretoze         kazdy jeden ma inac strukturovanu stranu, inac strukturovanu url a inac vyzera            scrapnuty text zo stranky cize aj filtre musia byt custom pre kazdu jednu stranku*
	
Dalej ked uz mame vsetko zapisane v google dokumentoch, spusti sa nas main agent. 

![[130.png]]

Main agent zoberie vysledky zo search agenta, ktore su zapisane v google dokumentoch a analyzuje ich. Prejde jeden dokument, z ktoreho si zapise najdolezitejsie veci a potom prejde na dalsi a toto spravi so vsetkymi dokumentami. Na konci budeme mat zanalyzovane najlepsie ponuky a poznamky pre answer agenta, z ktorych bude vediet vyhodnotit najlepsie investicie. V answer agentovi

 ![[131.png]] 
si vytvorime finalnu spravu najlepsich ponuk a investicii, ktoru vytvori pomocou poznamok, ktore ma z main agenta a outputu z main agenta. Dalej sme si na google disku vytvorili dokument, do ktoreho si budeme zapisovat spravu, ktoru nam da answer agent. 

![[132.png]]

Tuto spravu si zapiseme, nasledne vymazeme predosli obsah z dokumentu aby sa nam to neprekrivalo s novym a zapiseme to do dokumentu a prehodime do pdf. Cize na konci vsetkeho nam vznikne sprava s najlepsimi ponukami vyhodnotene na cene, mieste, typu nehnutelnosti a jej vlastnosti a zaroven aj investicna sprava, na aky typ investicie by sa dana nehnutelnost hodila a nejake financne kriteria. 

*****************************
