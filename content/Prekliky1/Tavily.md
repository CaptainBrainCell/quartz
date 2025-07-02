https://app.tavily.com/home

Nastroj, vdaka ktoremu mozeme brazdit web. Vytvorime si tu API kluc a dalej potrebujeme ist do *Documentation* -> *API Reference* -> *Tavily search*.
![[221.png]]
Tu najdeme celu dokumentaciu, ako co funguje a co mame robit ale nas zaujima najma cURL
![[21.png]]
Ten si hodime to https requestu konkretne do polozky JSON, kde si ho potom upravime tak, aby robil to, co potrebujeme (tu si nastavime co chceme, aby hladal, kolko chceme aby hladal ...).
![[22 1.png]]
potom si uz len nastavime metodu, url (v nasom pripade: https://api.tavily.com/search ) a headers, kde si nastavime nas API kluc. 
![[23.png]]
tam pred byva: Bearer <token> ale mi namiesto toho <token> dame nas token, ktory najdeme na tavily webstranke. 