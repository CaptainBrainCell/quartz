![[30.png]]

***POPIS***
	vytvoril som AI fakturanta. Tento fakturant, sa aktivuje, ked niekto hodi do priecinka fakturu. Ked ju tam hodi tak tu fakturu stiagne, vytiahne z nej dolezite data, dohodi veci do databazi a informuje tim o novej fakture, ktoru treba splatit.

***POSTUP***
	***PRVY KROK***
	Rozplanovat si vsetko na papier alebo niekde inde, co budem robit, ako budem robit, zapisovat si tam postup ze ako si predstavujem ze to mam spravit a ako chcem aby to fungovalo. Toto je dolezite si najskor napisat a nakreslit, nech to clovek vidi a nech mame aku taku predstavu, ze ako to vlastne bude fungovat a co budem robit. 
		![[22 1.png]]
															**Priklad rozpisania***
***Druhy krok***
	Vytvorit si workflow na zaklade naseho rozpisu a skontrolovat, ci vsetko funguje tak, ako ma. V tomto pripade sme si vsimli, ze nam to nejde pretoze ten invoice nam sice dosiel ale nemohli sme to extractnut, pretoze nemali sme binary data, ktore by sme vedeli extractovat. To znamena, ze ten invoice sme najskor museli stiahnut. Cize dosiel do naseho priecinku -> stiahli sme ho(aby sme ziskaly binary informacie(lahsie na extractovanie) ).
	
***Treti krok***
	Dalej sme si zapojili INFORMATION EXTRACTOR, ktory nam z napriklad PDF vie vyextractovat jednotlive veci, ktore potrebujeme(v nasom pripade - meno zakaznika, email zakaznika, telefonne cislo zakaznika ....) Tento email extractor urobi to, ze extraktne len to, co potrebujeme a nie vsetko. 
	
***Stvrty krok***
	Teraz, ked uz to mame vsetko spravene, pridame tam dalsi krok, co je, nech nam tu updatne nasu databazu(v tomto pripade google sheet) a zapise tam tie dolezite veci, ktore sme si vyextractovali. 
	
***Piaty krok***
	Dalej si vytvorime dalsi krok, co bude OPEN AI Message Model, ktoremu zadame, aky message ma vytvorit, ako ma vyzerat, co ma obsahovat... (on je email crafter).
	STATIC INFO -> SYSTEM MESSAGE ROLE (co ma robit, sablona, ako ma robit...)
	DYNAMIC INFO -> USER MESSAGE ROLE (cislo faktury, meno zakaznika, cislo zakaznika...)
		Musime si tam taktiez zaskrtnut to, ze to ma robit ako JSON, aby nam to rozdelilo na 2 casti (subject a email body), lebo teraz sme to mali len ako 1 velky celok(content).

***Siesty krok***
	Uz nam zostava len posledne a to ten vytvoreny e-mail poslat nasemu timu, ktory spracuvava faktury, na to, aby to zaplatili. Tym padom sme im usetrili a zautomatizovali celu jednu cast businessu, ktorou je Analyza emailu, Preskumanie, Spisanie, Zapisanie do databazy, Napisanie/prepisanie emailu			![[28.png]]


***NAVRHY NA ZLEPSENIE***
automaticky zaradovat faktury do google disku a davat ich do toho suboru 
zapisovat jednotlive faktury a ich splatnost do kalendara pre lepsi prehlad 

Zlepsenie:
dorobil som tam aj to zapisovanie do kalendara, cize ked pride faktura, tak automaticky ju zapise aj do kalendara, kde mame vacsi prehlad o fakturach ![[31.png]]