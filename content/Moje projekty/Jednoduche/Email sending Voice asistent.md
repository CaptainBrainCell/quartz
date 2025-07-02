*****************************
![[69.png]]

popis -> ide o jedmoducheho AI agenta, ktory vyuziva webhook na komunikaciu s elevenlabs, kde je prepojeny s voice asistentom, ktory nam pomaha odosielat maily. Voice asistent nam poda -> komu to mame poslat a o com ta sprava ma byt. 

![[70.png]]
webhook sme si nastavili na testovaci rezim a http metodu sme dali ako post. Ako path sme si nastavili n8n, co potom neskor zadefinujeme aj v elevenlabs
Dalej sme isli na eleven labs kde sme si vytvorili noveho agenta ktoremu sme dali prompty : 

![[71.png]]
![[71.png]]
a potom sme ho prepojili s webhookom 
![[72.png]]
a na zaver sme mu nastavili, co ma posielat naspat 
![[72 1.png]]
to co nam potom prislo spracoval nas AI agent a poslal mail 




miesto, kde si mozeme skontrolovat, ci nam nas webhook funguje : https://www.postman.com/