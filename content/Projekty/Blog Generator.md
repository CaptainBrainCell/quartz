***************************************************
![[25.png]]

***POPIS*** -> Ide o bota, ktory tvori blogy. Funguje tak, ze caka, kym mu napiseme temu na telegram. Ked mu temu napiseme tak vyhlada na webe najaktualnejsie informacie a LLM z nich vytvori blog. Vlastnosti blogu sme si nastavili v system prompte, kde sme specifikovali ako ma ten blog vyzerat, aky dlhy ma byt .... [[blog_generator_system_prompt]]. Dalej sme si blog setli ako post s value {{ $json.output }} pretoze ak ho neskor budeme menit tak nech to nemusime menit od zaciatku ale nech sa nam to postupne preraba. Dale si to posleme na telegram, kde mozeme odpovedat ci sa nam to paci alebo nie. Ak je to dobre tak to posleme dalej a postne sa to na twitter. Ak sa nam to nepaci tak napiseme co je tam zle, a BLOGREVISION nam to prerobi tak, aby opravil to co sme mu napisali ze je zle. 

***POSTUP***
*********************
***Prvy krok*** -> pridali sme si telegram get message trigger, ktory sa automaticky spusti, ked mu na telegram posleme spravu

***Druhy krok*** -> Pridali sme si AI Agenta (BLOG CREATOR), ktory ma za ulohu zobrat spravu z telegramu, na zaklade tej spravy vyhladat informacie na webe cez [[tavily]] , ktore nasledne spracuje a vytvori z nich blog. Vlastnosti blogu sme si nastavili v system prompte [[blog_generator_system_prompt]]

***Treti krok*** -> Pridali sme Set Node, ten nam zabezpeci to, ze ked sa nam nas blog nebude pacit a budeme tam chciet nieco zmenit tak nebudeme musiet vzdy upravovat povodny blog originalne z BLOG CREAT agenta ale nastavime si ho ako post do set nodu a vzdy ked sa nam v tom blogu nieco zmeni tak automaticky sa nastavi zmeny blog na post . 

*funkcionalita*
(original -> set) -> (zmena -> set -> zmeneny)   -> **ok**
								    -> (zmena -> set -> zmeneny) ........
								    
***Stvrty krok*** -> pridali sme si send and wait telegram message, ktory zabezpecil to, ze nam dosiel na telegram nas blog, ktory sme chceli ale ta wait funkcionalita nam umoznuje nieco dalej robit s tym blogom (validation/zmena)![[26.png]]
V nasom pripade sme vybrali free text, vdaka ktoremu sme mohli nas blog dalej upravovat, ak sa nam nieco nepacilo ( prilis dlhy / prilis vseobecny...)

***Stvrty krok*** -> Pripojili sme TEXT CLASSIFIER, ktory nam rozhoduje, ci je nas feedback kladny alebo zaporny. V tomto node sa rozhoduje, ci nas blog pojde dalej a postne sa alebo pojde dalej do BLOGREVISION, kde sa upravi podla nasich poziadaviek. V TEXT CLASSIFIERI sme si vytvorili 2 kategorie - approved a dissaproved
*Approved* -> rozhoduje o tom, ci sa text schvali a pojde delej. Toto rozhodovanie prebieha na zaklade promptu [[approved_prompt]] 
*Disapproved* -> rozhoduje o tom, ci sa texy neschvali a pojde dalej do BLOGREVISION, kde sa upravi podla nasich poziadaviek a tento cyklus za zopakuje. Funguje na zaklade tohoto promptu[[disapproved_prompt]]+

***Piaty krok*** -> Ak sa nas TEXT CLASSIFIER rozhodol, ze to NEapprovdne tak sa nam nas blog spolu s feedbackom da do BLOGREVISION agenta, ktory nam nas blog opravi podla nasich poziadaviek.
Potom ten opraveny blog posle do Set Node, kde sa tento blog nastavi ako post a cyklus sa opakuje az kym nebudeme s blogom spokojni. 

*Tu som sa zasekol a nevedel som prist na to, preco mi sem nejde dat output (blog). lebo ked som pozeral Set node tak mi tam pisalo ze ziadne predchadzajuce nody neboli aktivovane, cize nema ziadne data a az po chvili trapenia som prisiel na to, ze ja som nemal brat ten blog z Set nodu ale z BLOGCREATOR outputu.* :D

*taktiez vsetky ##Overviev prompty. co sa nachadzaju v tychto modeloch boli kompletne vygenerovane mojim [[Prompt Creator]], ktory som vytvaral davnejsie*

