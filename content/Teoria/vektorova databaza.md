![[vector databsae.png]]

rozmiestnenie jednotlivych informacii v 3d priestore (xyz osi) a roztriedime si to tak, aby na jednej strane bol len 1 druh info cize na lavej strane mame napriklad ovocie(banany,hrusky), na pravej mame auta(mustang voklswagen) a dole mame hry (tarkov, rust ) teraz, ked to mame takto pekne roztriedene tak ked pouzivatel zada, ze hlada napriklad melon tak automaticky ho to hodi do sekcie s ovocim, lebo tam je najvacsia prilezitost ze to najde .....

***SUPERBASE*** -> k nasemu AI agentovi si pripojime model, ktory chceme a ako memory tam dame **postgres** a ako toolku ku nemu dame **superbase vector store**

nastavenie Postgres
************
Vtvorime si ucet na superbase
Vytvorime novy projekt (zapiseme si heslo)
Potom prejdeme na Database, kde klikneme connect
![[52.png]]
Potom sa pozrieme na Transaction pooler, kde najdeme *hosta* -h , *usera* -u , port -p a este tam musime zadat heslo ktore sme si zapisali![[53.png]]

nastavenie Superbase (vektorova databaza)
************
Do nastaveni si pridame hosta, ktoreho najdeme v superbase -> Data API![[54.png]]
A potom musime doplnit aj service role secret, ktory najdeme v project settings -> API Keys![[55.png]]
Ked to mame tak potom musime v n8n rozkliknut superbase, kde v pravo hore vidime docs. Klikneme na to potom sa musime prekliknut na [quickstart for setting up your vector store](https://supabase.com/docs/guides/ai/langchain?database-method=sql) a tam najdeme toto: ![[56.png]]
to skopirujeme a pojdeme naspat na webovu stranku superbase -> SQL Editor a tam to pastneme a pustime![[57.png]]