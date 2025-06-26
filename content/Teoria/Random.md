
BUILD & SELL AI AGENTS

**RAG**
retrival augmented generation -> Ked sa ma niekto spyta otazku a ja na nu nemam odpoved, tak si ju niekde vyhladam ( napriklad v mobile ) a potom mu poviem odpoved
**VECTOR DATABASE**
rozmiestnenie jednotlivych informacii v 3d priestore (xyz osi) a roztriedime si to tak, aby na jednej strane bol len 1 druh info cize na lavej strane mame napriklad ovocie(banany,hrusky), na pravej mame auta(mustang voklswagen) a dole mame hry (tarkov, rust ) teraz, ked to mame takto pekne roztriedene tak ked pouzivatel zada, ze hlada napriklad melon tak automaticky ho to hodi do sekcie s ovocim, lebo tam je najvacsia prilezitost ze to najde .....![[vector databsae.png]]

**SPOLOCNE FUNGOVANIE**
zoberieme dokument a rozdelime ho na casti. Embedding model ho premeni na cisla (tak aby sa v tom pocitac zorientoval) a hodi to do vektorovej databazy
![[vdb&rag.png]]

**AKO TO SPOLOCNE FUNGUJE? (RAG PIPELINE & CHATBOT)**
![[ako to  funguje.png]]

mi dame otazku na naseho bota (llm), On si na zaklade tej otazky najde miesto vo vektorovej databaze, kde sa to asi moze nachadzat, zoberie 5 najblizsich veci co tam najde a sformuje z toho odpoved
