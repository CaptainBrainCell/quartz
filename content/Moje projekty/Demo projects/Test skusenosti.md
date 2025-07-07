***********************************
![[119 1.png]]

POPIS : Ide o automatizaciu, ktora sa nam spusti kazdy den v urcitom case (ja som to nastavil na 6 hod rano), ktora nam zoberie eventy, ktore nas nasledujuci den cakaju, a z informacii zadanych v kalendari nam **Zisti zakladne informacie o danej firme (trzby,komodity,historia....) a osobnost osoby, s ktorou budeme jednat** , co nam nasledne pomoze pri rokovani s danou osobou. 

-> Funguje to tak, ze kazdy den rano sa nam to spusti, zoberie eventy z kalendara, ktore si nasledne rozhodi na jednotlive meetingy. Potom si nasa automatizacia zoberie jeden z meetingov, v ktorom skontroluje, ci sa v informacii z kalendara, ktora patri ku meetingu nachadza mp3 dokument (toto potrebujeme pre spravne fungovanie automatizacie neskor). 
	Ak nie, tak nam to vyhodi error, ktory nam nasledne nas [[Error logger]] zachyti a posle nam informaciu o chybajucom mp3 subore.  
	Ak ano tak nasa automatizacia pokracuje dalej. 	
Nasledne to ide do EXTRACT DATA, kde si vyberieme z textu, ktory bol pridany ku meetingom  polozky, ktore budeme potrebovat (ico, nazov meetingu, mp3 url.....). Z Extract data to prejde do code nodu kde pomocou [[skriptu]] vycistime mp3 link od html poloziek, ktore sa nam tam automaticky vlozili pri stahovani dat z kalendara.  Z code nodu sa nam to posunie do DOWNLOAD MP3, kde nam to stiahne nasu mp3 nahravku pomocou url, ktoru sme mali v google calendar texte. Dalej to prejde do PREPIS MP3, kde sa nasa mp3 nahravka prepise do textu a posle sa dalej do PEROSNALITY ANALYSIS, kde sa na zaklade [[promptu]] zanalyzuju osobnostne prvky cloveka z nahravky. Tuto analyzu si nasledne zapiseme do SET CONTENT aby sme ju vedeli neskor pouzit. Dalej nam to prejde na WEB SCRAPE, ktory sme spojazdnili pomocou [[firecrawl]]. Tento webscrape prejde na stranku https://finstat.sk/, kde vyhlada spolocnost, ktorej ico sme mali v google calendar texte a nasledne nam webovu stranku scrapne. Tieto informacie, ktore sme ziskali scrapom webu potom vlozime do  COMPANY ANALYSIS, kde sa nase informacie spracuju na zaklade naseho [[systemoveho promptu]] a vyberu sa len tie najdolezitejsie a najreleventnejsie. Nasledne si vystup z COMPANY ANALYSIS zapiseme do SET INFO. Dalej to putuje do BRIEFING CREATION, kde sa spracuje alalyza osobnosti, ktoru sme mali ulozenu v SET CONTENT a analyza firmy a vytvori sa nam z toho jeden finalny vystup, ktory obsahuje vsetko, co sme chceli (formu tohoto vystupu si vieme nastavit podla seba a podla nasich potrieb (detailnost, dlzka, ..... )). [[Priklad vystupu]]
Nasledne sa nam  tento vystup zapise do google docs databazy ( na neskorsiu kontrolu ak by to bolo potrebne ) a potom sa nam to posle na [[slack]], kde sa to zapise do nami zvoleneho kanala (sem sa to posle aby k vystupu malo pristup viac ludi v pripade ze by musel dany meeting prebrat niekto iny) a taktiez sa to posle priamo obchodnikovy na mail (toto mozeme zmenit na cokolvek ine napr. Telegram, Whatsupp ...). V pripade, ze by sme v dany den mali viac meetingov, a nie len jeden tak som pridal aj funkciu LOOP THROUGH EVENTS, ktora nam umoznuje urobit tento process pre kazdy jeden meeting, ktory na nasledny den mame. *Cize ak mame 3 meetingy v nasledujuci den tak nemusime tento proces 3 krat spustat ale spusti sa sam v nami urcenom case a spravi tieto vystupy pre kazdy jeden meeting a kazdu jednu firmu, ktoru mame v kalendari*. Ked sa tento proces dokonci a urobia sa vystupy pre kazdy jeden meeting tak nam to posle na mail potvrdenie, ze vystupy boli dokoncene.  [[done message]]

**Informacie**
**************
-> Ak by sa toto realne malo niekedy vyuzit tak mp3 subory by sa museli nahadzovat napriklad na google disk z kadial by sa nasledne dali stahovat do n8n workflowu. 
-> Z presnymi informaciami, formou z google calendaru a mp3 nahravkou by sa dal tento model nastavit tak, aby bol uz realne funkcny na to co ma ( teraz ma nahrate len demo veci na testovanie)

**Dolezite Linky**
**************
->   [[write to database ]] 
->   [[slack message]]
->   [[gmail message obchodnikovy]]
->   [[done message]]
->   [[Priklad vystupu]]
->   [[Json]] , [[test_skusenosti_json_download]]
