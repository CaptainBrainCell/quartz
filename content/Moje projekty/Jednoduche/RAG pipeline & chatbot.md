[[My_workflow_5.json]]
![[rag pipeline & chatbot 1.png]]

**POPIS**
*CAST PRVA* -> Vytvorili sme system, ktory cerpa z pinecon databazy, do ktorej si rucne pridame nejake dokumenty. Funguje tak, ze mi si do folderu na google disku pridame nieco co podrebujeme (FAQ, Ako funguju zaruky, rozne ponuky co ponukame a ich popisy). My ich sice vidime ale nevieme ich precitat. Tym ze to tam pridame, automticky sa zapne trigger. Ked sa zapne trigger, tak dalej sa nam to najskor musi stiahnut, aby sme to vedeli precitat (preto ten download). Dalej sme si napojivi vektorovu databazu ( pinecone ), do ktorej tieto informacie nasledne umiestnime. Vybrali sme si embedding model (model, ktory nam do tej databazy informacie pouklada a potom ich tam bude hladat). Dalej ako data loader sme si zvolili defaultny data loader a recursive charakter text splitter([[splitery]]), pomocou ktoreho sme si ten jeden velky text (cele FAQ) rozdelili na viacej malych casti. 
*CAST DRUHA* -> Vytvorili sme si chatbota, ktory cerpa z tej databazy, kde sme si vsetko zapisali a rozdelili. On funguje tak, ze dostane otazku, premysli to a nasledne vyberie z databazy to co sa mu najviac hodi. Najskor sme si tam dali chat trigger (aktivuje sa na spravu). Ako nahle dostane spravu, zacne premyslat. Ked si premysli tak pojde do pinecone databazy, co sme mu dali ako toolku a tam pomocou embedding systemu najde informacie, ktore sme po nom chceli. (Ak by nespolupracoval tak ako ma tak mu mozeme napisat blizsie informacie a naviest ho tak, ze si ho rozklikneme a pridame mu system message, kde mu podrobne popiseme, co ma robit a z kadial ma cerpat)





**PRVY KROK** -> Zapli sme skenovanie suboru na nasom google drive (vzdy ked sa nam tam nieco nahodi, tak sa to zapne a automaticky to stiahne)![[1.png]]



**DRUHY KROK** -> vytvorili sme si pinecone datavazu ( vektorovu ) kam sme si pridali AI model, ktory bude ako mozok, ktory nam tie informacie v tej databaze pouklada. Potom sme pridali dataloader, ktory to do tej databazy loadne a text splitter, ktory to rozdeli na viacej casti (velkost sme si zvolili)
![[1 1.png]]


**TRETI KROK** -> pridali sme si chat trigger, ku ktoremu sme pripojili AI asistenta. Ako model sme vybrali open router chat model (umoznuje nam vybrat si ktorekolvek llm, ktore chceme). Ako tool sme mu pridali [[vektorova databaza]] , v ktorej su naukladane informacie, z ktorych chceme aby chatbot cerpal.