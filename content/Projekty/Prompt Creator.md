***************************
![[62.png]]

***POPIS*** - Ide o Agenta, ktoreho ulohou je vytvarat ## Overview prompty pre inych ai agentov na zaklade zakaznokovych poziadaviek. Funguje tak, ze vzdy, ked zakaznik vyplni formular, tak on si automaticky stiahne pdf s informaciamy o promtoch a promptacii z tade si vytiahne najdolezitejsie informacie a hodi iich spolu s poziadavkami do PROMPT CREATORA, ktory na zaklade poziadaviek zakaznika a na zaklade informacii z PDF ka vytvori co najlepsi prompt ktory nasledne zapise do nasej google docs databazy a potom prompt posle naspat na zakaznikov email, ktory zadal vo formulari.

***POSTUP***
***********
***Prva cast*** -> vytvorili sme formular, kde sme si zadali, aby nam zakaznik zadal email a popisal, co ma jeho agent robit. 

***Druha cast*** -> Dalej som vytoril pdf, so zakladnymi informaciami, ako funguju prompty a ako ich pouzivat, ktrore si nas agent sitahne, vytiahne z tade najdolezitejsie informacie pomocou EXTRACT FROM PDF a posle ich do naseho PROMPT CREATORA. Tu som s tym mal trosku problemy tak som to musel vyriesit cez  *GENERATE FROM JSNO EXAMPLE* a uz to potom islo

![[63.png]]

***Tretia cast*** -> Nas PROMPT CREATOR zoberie informacie z PDF EXTRACTORA, zoberie si aj customer input a na zaklade toho vytvori velmi specificky a jedinecny prompt pre ai agenta, ktory nasledne zapise do databazy a posle zakaznikovi mailom informacie o prompte (jeho input, prompt) dal som mu tento prompt, ktory som musel tak na 5 krat prepisovat pretoze nesiel poriadne [[systemprompt]] a taktiez som tam pridal *PARCER*  ![[64.png]]
kde som si vygeneroval output v specifickom formate tak, ako budem neskor potrebovat. Teraz mam namiesto 1 outputu, kde je vsetko 2 outputy, kde mam **agenta** - na zaklade user imputu si PROMPT GENERATOR vydedukoval o akeho agenta by mohlo ist a **output** - uz samotny prompt, ktory PROMPT GENERATOR vygeneroval na zaklade poziadaviek a informacii z pdfka

***Stvrta cast*** -> PROMPT GENERATOR tieto informacie posle dalej do nasej databazy (google sheet), kde si zapisujeme vsetko, co vytvoril ![[65.png]]
a nasledne ked to zapise do nasej databazy tak z tade to este zoberie a posle na zakanikov mail kde to je vo formate : 

based on this text :
	text uzivatela.............................................

we make a prompt :
	prompt ktory sme vytvorili...........

hope it helps :D![[66.png]]