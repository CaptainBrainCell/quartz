***Prompt Chaining***
*************
![[58.png]]

-> ide o spojenie viacerych LLM na vytvorenie 1 vysledku, v nasom pripade blogu. Tento blog bude podrobnejsi a lepsi ako blog co vytvori len 1 LLM pretoze kazdu cast tohoto blogu ma na starost ine LLM, to znamena ze konecny vysledok bude presnejsi, lepsi a podrobnejsi. Taktiez sa tento sposob da lahsie scalovat a lepise upravovat zhladom na jeho jednoduchost
-> na napisanie 1 blogu pouzivame 3 LLM. Na Outline write pouzivame gemini 2.0 Flash pretoze je dostatocne silny a je lacny. Na outline evaluation pouzivame gpt 4o mini pretoze je silnejsi a lepsi ale uz ma prvotnu strukturu spravenu takze to bude vediet lepsie spravit a ako posledy na napisanie blogu pouzivame Claude 3.5 pretoze je najlepsi na pisanie blogov a na content a tym ze uz sme dali 2 predosle veci napisat inym LLM tak Claude to bude vediet lepsie a podrobnejsie napisat tak, ako potrebujeme

***Routing***
*************
![[59.png]]
-> funguje to tak, ze mame viacej kategorii v nasom pripade high priority,customer support, finance a promotions. Na zaciatku nam pride mail, potom LLM rozhodne, o aky typ mailu ide, ci o customer support, high priority ...... a na zaklade toho to presmeruje na dany smer. 
-> Kazdy jeden Agent v jednotlivych kategoriach moze mat svoju vlastnu personality a moze byt inak nastaveny, tak aby co najlepsie splnal podmienky, ktore su urcene pre jednotlive kategorie. (napr pri customer support emailoch moze byt mily a velmi napomocny a pri high priority moze byt trosku tvrdsi a subjektivnejsi)

***Paralelization***
**************
![[60.png]]
-> toto funguje tak, ze na nejaky ukon spustime naraz viac LLM, ktore zanalyzuju danu situaciu a budu ju vediet vyhodnotit rychlejsie a efektivnejsie ako keby to robilo len 1 LLM
-> v nasom pripade analyzujeme spravu, jej emociu, zamer a bias. Na kazdu 1 kategoriu mame 1 Agenta, ktory sa specializuje len na danu oblast, cize jeho odpoved bude presna a vystizna. Ked tieto odpovede dostaneme, tak ich spojime a posleme do finalneho Agenta, ktory spise spravu a nasledne nam ju zapise do google dokumentov

***Evalution Oprimizer***
***********
![[61.png]]
-> funguje to tak, ze mame 1 agenta v nasom pripade na pisanie blogov. Napise blog, ktory nasledne prejde k evaluator agentovi (ten overi, ci to moze byt alebo tam este nieco chyba) ak tam nieco chyba tak to posle dalej k optimizer agentovi, ktory to optimalizuje a vylepsi. Ten to nasledne posle naspat evaluator agentovi, ktory to zase skontroluje a toto sa opakuje, kym ten evaluator agent nebude spokojny s vysledkom. Ked uz to bude dobre, tak to publisne do dokumentov 