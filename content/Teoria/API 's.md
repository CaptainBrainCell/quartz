	-> pomocou nich prepajame vsetko, co chceme pouzivat a pomocou nich prepajame jednotlive sluzby ako OpenAI a DeepSeek alebo PineCone tak, aby s nimi vedel n8n pracovat.

***AKO FUNGUJU***![[33.png]]
Funguje to tak, ze nas ai agent si pozrie, ktore requesty potrebuje, potom posle cez http request na server, kde je dana sluzba, ktora mu vyberie data, ktore si zelal a tie mu potom posle naspat cez server a http request ![[34.png]]

***FILTRE V API***
-> Method : GET -  Ked sa chceme dostat na nejaky endpoint (server) a nechceme tam nic postnut,      ale len si z tade zobrat (nic tam neposielame, ziadne data ale len z tade data berieme (ked si           chceme pozriet pocasie v Thajsku ))
		   POST - Ked chceme danej sluzbe poslat nejake informacie, na zaklade ktorych chceme nejaky pozadovany vysledok (using this information, send me back what i am asking for)
-> Endpoint : dana webstranka alebo server, na ktory sa chceme dostat
-> Query parameters : filtre, podla ktorych vyberame, co chceme
-> Header parameters : Autorizacia, pomocou ktorej preukazeme ze sme na to autorizovany : Bearer xxxxxxx - api kluc
api_key xxxxxxx
-> Body parameters : uz jednotlive veci, ktore chceme {{"name" : "John"}}

Priklad ***cURL***
![[35.png]]

***Url*** - Z kade chceme dane informacie ziskat (*z ktorej restiky si chceme objedntat*)
***Header authorization*** - sem zadame token, pomocou ktoreho sa autorizujeme (*zadame nasu kreditku*)
***Data*** - uz jednotlive veci, ktore chceme ziskat (*chceme pizzu/burger ak pizzu tak stiplavu/nestiplavu, s hranolkami/bez...*)

ak chceme aby za nas vyhladaval ***AI AGENT*** to co mu zadame tak musime JSON trosku upravit:
![[35 1.png]]
		*zakladny*
Tuto mozeme vidiet, ze tie informacie su tam hadrcodnute, to znamena ze vzdy, ked nieco vyhladame, tak nam to vyhlada to iste a ak to chceme zmenit tak, aby to vyhladavalo veci, ktore zadame do naseho AI Agenta, tak v prvom rade si to musime pripojit k AI Agentovi ako toolku
1. AI Agent s HTTP Request toolkou
2. Dat tam cURL, ktory nam vyplni najdolezitejsie veci
3. Upravit JSON tak ako potrebujeme, v nasom pripade, ak chceme aby AI vyhladavalo na webe veci, co mu zadame tak musime dat do JSON'a {{fromAI(*nieco co to vystihuje napr. SearchTerm - vec, co chceme vyhladat*)}}

![[36.png]]

Vzdy to doplname do **USER** role, pretoze do **SYSTEM** role zadavame len nejake presne instrukcie, co a ako ma dane veci vyhladavat 

![[37.png]]

	Funguje to vlastne tak, ze to AI si nahradi ten searchTerm za to, co mu zadame a pomocou toho hlada na webe, to je jeho klucove slovo/slova
	
***ERRORY CO MOZEM DOSTAT***
![[38.png]]

200 - preslo
400 - zla query
401 - zly api kluc
403 - nemam pristup k danym veciam
404 - zla url, zle zadana stranka
500 - server m aproblem, nie je moja chyba 