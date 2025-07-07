****************
![[26 1.png]]

***POPIS*** -> Ide o ERROR LOGGING workflow, co znamena, ze si ho pripojime k nasim main workflowom, kde caka, az kym ho main workflow neaktivuje (aktivuje ho tym ze dostane error)
Ked sa aktivuje, tak automaticky nam zapise najhlavnejsie informacie do google tabulky, kde bude vsetko pekne spisane aby sme presne vedeli, co kde kedy a ako sa stalo a taktiez nam posle spravu do naseho slack servera, kde si taktiez budeme vediet pozriet najdolezitejsie veci 

***POSTUP***
**************
***Prva cast*** -> hodime si do naseho workflowu *Error Trigger* (aktivuje sa vzdy, ked najeky z nasich workflowov, kde mame tento error logger aktivovany dostane nejkaky druh erroru) a pripojime si k nemu nasu databazu (v mojom pripade google sheets - prehladnost) a slack (tu si vytvorime kanal napr. errorloging, kde nam budu chodit vsetky errory) kde bude nas error trigger ukladat informacie o erroroch tak, aby boli co najprehladnejsie a najjednoduchsie na vyriesenie
*toto sa da este viacej vylepsit, napriklad ked pride nejaky error tak nam to vie posla spravu alebo zavolat*

***Druha cast*** -> prejdeme na workflow, s ktorym si chceme prepojit nas ERROR LOGGER (mozeme pripojit viac workflowov na jeden ERROR LOGGER) prejdeme do nastaveni a tam ako ERROR WORKFLOW nastavime nas ERROR LOGGER workflow. Tymto sa tieto workflowy prepoja a ako nahle dostane nas main workflow error tak sa aktivuje ERROR LOGGER, ktory nam ten error podropne zapise pre neskorsiu analyzu

*Error workflow funguje len v pripade, kedy je nas main workflow aktivny*

![[28 1.png]]

![[29.png]]