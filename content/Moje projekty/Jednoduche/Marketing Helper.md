![[39 1.png]]

***POPIS*** -> Bot, ktory na zaklade formulara vytvori profesionalne video na marketingove ucely. Funguje tak, ze mi mu vyplnime formular so zakladnymi vecami ([[formular]])
On na zaklade tychto veci vytvori profesionalny obrazok cez image generator (gpt-image-1), ktory dalej posunie do image to video generatora (runaway), ktory z toho obrazku spravi profesionalne video, ktore je vhodne na marketingove ucely. 

***POSTUP***:	
***************************************************************
**Prva cast** -> vytvorime si formular, kde si zadame, co chceme, aby nam zakaznik poskytol(vec, obrazok, popis). Dalej si obrazok uploadneme na google disk.
	
**Druha cast** -> vytvorime AI Agenta, ktory ma za ulohu vytvarat prompty pre generovanie AI obrazku naseho produktu. Na zaklade naseho obrazka, ktory sme mu zadali vytvori cez AI taky isty obrazok ale v profesionalnom prevedeni. *Dalej sme pripojili download node pretoze nas GET PICTURE node vie pracovat len z binary datami a keby to nestiahneme tak to nie je v binary datach*.

***Tretia cast*** -> Pripojili sme http request na https://api.openai.com/v1/images/edits ku ktoremu sme sa dostali cez https://platform.openai.com/docs/api-reference/images/createEdit, kde si mozeme skopirovat cURL (v nasom pripade to neslo, pretoze bola pokazena) a tak som si tu musel nastavit sam. ![[41.png]]
Na API referencii mozeme vidiet ze potrebujeme *image*(nas obrazok), *prompt*(prompt, ktory pouzijeme na generovanie naseho obrazku) a *-H "Authorization"* (ide o header autorizaciu s Bearer pred nasim klucom). Tak tiez sme tam nasli aj nejake veci navyse, ak vy sme chceli podrobnejsie vyvorit nas obrazok ako napriklad *background (pozadie),output_format (v akom formate chceme, aby nam to vyplulo),n* (kolko obrazkov nam to ma vytvorit) a este sme si museli doplnit aj model, pretoze ak by sme si ho nedoplnili tak defaultne by nam to davalo *dall-e-2* model a my chceme *gpt-image-1* pre lepsiu kvalitu. 

![[42.png]]
										*(konecna query)*
***Stvrta cast*** -> Dalej sme si pripojili *Convert to file node*, ktory nam vie prehodit z base64 na obrazok. Po tomto si hodime nas obrazok na web cez imgbb (https://sk.imgbb.com/) aby sme s obrazokm vedeli dalej pracovat. Tu sme si vsimli, ze na API je povinne key a image a taktiez tam je url, na ktoru to mame posielat. 
![[42 2.png]]

***Piata cast*** -> Ked sme si nas obrazo hodili na web, je na case prehodit ho na image to video generator pomocou http requestu CREATE VIDEO (v nasom pripade Runaway), ktory nam vytvori profi video z obrazku. Isli sme na https://docs.dev.runwayml.com/api/#tag/Start-generating, kde vidime vsetko potrebne na nastavenie API. Vidime tam cURL , ktory skopirujeme a hodime si ho do naseho http requestu.  ![[43.png]]

Taktiez tam mozeme vidiet aj veci, ktore musime mat v nasom http requeste aby nam to vsetko poriadne fungovalo a aj nejake veci navyse ak by sme to chceli vytvorit este podrobnejsie![[44.png]]
V nasom pripade sme position nemuseli pouzit. Museli sme vsak este pouzit aj ratio (aky velky obrazok chceme), ktoreho rozmery boli v API Referencii. 

***Siesta cast*** -> Pridali sme si dalsi http request node, ktory nam pozera *Status* naseho videa (ci sa este tvori alebo uz je hotove). Tento request sme ziskali z https://docs.dev.runwayml.com/api/#tag/Task-management/paths/~1v1~1tasks~1%7Bid%7D/get, kde si mozeme skopirovat cURL a hodit si to do naseho requestu. ![[44 1.png]]

Tu sme len museli zmenit ID, ktore sme dostali z predchadzajuceho http requestu a mohli sme skontrolovat *STATUS* naseho videa. ![[45.png]]

***Siedma cast*** -> pridali sme tam loop, ktory kontroluje, ci uz je video vytvorene alebo este nie (tvorba videa moze byt az minutu a viac, preto to kontrolujeme, nech nam to neposiela prazdne maily). ![[46 1.png]]
funguje to tak, ze *prvy wait* je nastaveny ma 30s, nech nam ten dalsi loop neopakuje 30 krat... Dalej po STATUS CHECK sme doplnili If, ktore nam kontroluje, ci uz je video hotove alebo nie.![[47.png]]
Ak je status == RUNNING -> true, tak nam to hodi na wait, ktory je nastaveny na 5 sekund a potom to znova opakuje, az kym status != RUNNING, kedy to prejde na dalsi node, ktory nam posiela mailom uz hotove video a obrazok. 