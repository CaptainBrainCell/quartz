![[11.png]]

**POPIS**
	-> Ide o bota, ktory zareaguje na prijaty mail. Ten prijaty mail potom overi, ci to je nieco ohladne customer supportu alebo to je nejaky random mail. Ak je to random tak nerobi nic. Ak to je nieco, co ma docinenie s customer supportom, tak potom pokracuje dalej. Pripojili sme si ku tomu AI agenta, ktoremu sme dali instrukciem co ma robit ([[instrukcie]]). Pripojili sme ku nemu pinecone databazu, v ktorej uz mame nahodene veci z predosleho projektu ([[RAG pipeline & chatbot]]). Mame tam jednoduchce FAQ, na ktore sa budeme pytat. Tento Agent si zanalyzuje spravu, pochopi jej a na zaklade nasich poziadavok, ktore sme mu zadali vykona to, co ma. Ked vytvori odpoved, automaticky dame response na mail a vybavenko. 
	


**PRVY KROK** (spracovanie mailu a rozdelenie) -> pripojime si gmail trigger konkretne na message recieved. Dalej su ku tomu zapojime text klasifier (ten nam umozni to, ze vieme rozlisit, ci ide o mail tykajuci sa customer supportu alebo len o nejaky random mail). V text clasifiery si zvolime kategorie ([[kategorie]]) a ten potom LLM, ktoreho si zvolime rozhodne, do ktorej z tych kategorii nas mail patri. 

![[13.png]]

**DRUHY KROK** (odpoved na mail) -> pripojime si k tomu AI agenta, ktory bude vymyslat spravu, ktorou odpise. Nasemu agentovi dame insrukcie, podla ktorych ma konat ([[instrukcie]]) a nedame ho na chat trigger ale na message prompt, ktory mu zadame ([[instrukcie]]). Zvolime, na ktoru cast ma nas agent odpovedat (obsah emailu). Po tom zvolime LLM, ktory bude premyslat, v nasom pripade openrouter chat model (mozeme si vybrat ktorykolvek) a ako toolku dame nasemu AI agentovi nasu pinecone databazu([[vektorova databaza]]), v ktorej su potrebne informacie na odpoved. Ked si rozklikneme pinecone vector store (databazu) tak ***NASTAVIME PORIADNE MENO, PRETOZE MOZEME TO POTOM UPRESNIT AI AGENTOVI V SYSTEM PROMPTE ZE S TOUTO TOOLKOU MA PRACOVAT, CIZE KED TO NAZVEME KNOWLEDGE A NAPISEME AGETOVI ZE MA PRACOVAT S TOOLKOU KNOWLEDGE TAK HNED BUDE VEDIET*** taktiez ***PINECONE NAMESPACE SA MUSI ZHODOVAT Z NAMESPACE, KTORE SME SI ZADALI NA WEBE ABY TO SPRAVNE FUNGOVALO***!. Potom si uz len zvolime embeding model, pomocou ktoreho sme to ukladali.
![[15 2.png]]


**TRETI KROK (odpoved na mail)** -> zvolime si gmail -> reply a pridame si to. Tam uz len zvolime message ID (najdeme id mailu, ktory nam dal trigger) a odpoved z naseho AI asistenta. **NEZABUDNEME PREPNUT EMAIL TYPE Z HTML NA TEXT**

![[16.png]]


**NAVRHY NA ZLEPSENIE** 
-> Ak by sme to chceli spravit podrobnejsie, tak by sme mohli spravit tohoto pomocnika na kazdu jednu oblast, ktoru potebujeme (financie, reklamacia, dodacie casy.......)

-> Taktiez ak by mozeme pridat na ten mail aj label, ze ide o customer support, nech to je trosku prehladnejsie 
![[17.png]]
