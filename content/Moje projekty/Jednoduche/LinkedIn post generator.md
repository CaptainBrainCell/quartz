![[18.png]]

**POPIS**
	-> Ide o bota, krory nam vytvara posty napriklad na linkedin alebo facebook, to uz je na nas. Aktualne je na manualny trigger ale mohli by sme pridat Schedule trigger, ktory by zabazpecil to, ze kazdy den sa nam napriklad o 12 spravi novy text, ktory mozeme nasledne postnut. Funguje tak, ze mame nejaky google sheet ("*get rows*"), v ktorom mame policka : 
	![[19.png]]
	Rozdelime si to tak, aby sme tam mali hlavnu temu, moznost, ci uz bola dana sluzba vykonana alebo nie a content. V nastaveniach si nastavime filter, nech je to na status -> 1. moznost (este nebolo vykonane) a pridame si tam nech nam to vrati len prvy matching row. Potom posleme https request, ktorym ziskame informacie o danej teme. Tie informacie nasledne chatbot spracuje (v nasom pripade 3 clanky) a spravi z nich napriklad jeden (zalezi od toho, co mu zadame). Ja som mu zadal toto([[zadanie]]) a podla toho mi napisal clanok. Dalej sa to poslalo do toho isteho dokumentu ale uz to zmenilo aj status a pridalo content (nastavili sme v poslednom google sheet "*update row*")
	![[20.png]]
	Tu mozeme vidiet, ako nam to hodi.


**PRVY KROK** -> nastavime si trigger(moze byt manual alebo scheduled). Porom si pridame google sheet (*get rows*) pretoze potrebujeme precitat, co sa v tom sheete nachadza a aby sme si to vedeli pekne rozdelit na temu, status ... . Pridame si tam filter, nech to berie len polozky so statusom možnost 1. (v nasom pripade to je ako neurobene) a pridame si tam aj dalsiu moznost a to tu, nech nam to da len prvy vysledok, ktory sa zhoduje s nasimi poziadavkami, nie vsetky. 

**DRUHY KROK** -> Hodime si tam HTTPS request (pomocou neho vieme brazdit net a bud posielat informacie ->  POST alebo len ziskavat ->  GET), ktory si spojazdnime vdaka [[Tavily]] 

**TRETI KROK** -> Spojazdnime si AI Agenta, ktoremu zadame to, co checeme aby robil. Ja som mu ako source for prompt dal vlastny prompt([[prompt]]) a toto zadanie(system message) ([[zadanie]]). Tu som vlastne spojil tie 3 texty a zadal som AI agentovy, nech tieto texty spoji a urobi z nich jeden clanok. To som spojil s LLM, ktory to vsetko premyslel a urobil (vybral som si OpenRouter chat model, kde si mozem vybrat ktorekolvek LLM). 

![[24 1.png]]

**STVRTY KROK** -> vybral som dalsi google sheet (*update rows*), kde som dal to, nech sa to matchuje na Topicu (lebo je jedinecny) a nech to prehodi Status na 2. moznost (urobene) a content nech da output z AI agenta.



***NAVRHY NA ZLEPSENIE***
-> mozeme tam dat namiesto MANUAL TRIGGER -> ON A SCHEDULE TRIGGER co nam umozni to, ze sa nam to bude robit same v nami zvolenom case, cize poveim si ze rano o 7 to tam chcem mat, aby som to mohol postovat a ono mi to rano o 7 spravi nech to mam
-> mozem to spravit tak, nech sa to potom postuje automaticky, ze sa to spravi same o 7 a o pol 8 sa to same postne
-> mozem spravit to, nech mi to same vytvara TOPICS, nech to nemusim vkuse dopisovat ja

---> z tohoto by som mal plne automaticky system, ktory by mi automaticky pridal nove TOPICS, spracoval ich, napisal na ne clanky a POSTOL bez toho, aby ja som nieco spravil 