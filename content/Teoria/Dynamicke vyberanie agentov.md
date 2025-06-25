Princip -> Pouzijeme jedneho AI agenta, ktory ma zapojeny free LLM (najviac cost effective), ktory bude rozhodovat o LLM, ktory bude pouzivat dalsi AI agnet.

![[68.png]]
v tomto pripade mozeme vidiet dvoch AI agentov. Jeden je len cisto na vyberanie LLm-ka pre dalsi model. Mozno sa pytate preco? Takymto sposobom je vyber LLM-ka efektivnejsi, uspornejsi a mame vacsiu kontrolu nad tym, ktory model nas MANAGER vyberie na task. 

CIZE -> Model Mage vyvberie na zaklade tohoto promptu [[model_mage_prompt]] niektory z llm-iek, ktore sme mu zadali ze ma na vyber a vyberie je tak, ze pozrie sa na to, co od neho ziadame a podla narocnosti situacie a poziadaviek vyberie llm

->MANAGER je len taky testovaci panak, ci to vsetko islo tak, ako by malo 

***Stranky na porovnavanie a vyberanie LLM***
**************
https://www.vellum.ai/llm-leaderboard# -> pozeranie, ktory agent je na aku cinnost najlepsi, kolko stoji a porovnavanie s ostatnymi

https://lmarena.ai/ -> porovnavanie llm-iek in real time, vidime jednotlive vystupy