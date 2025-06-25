***ROZDIEL***
![[50.png]]

***RELIABILITY & CONSISTENCY***
	AI Workflows -> my zadavame, co sa v akom poradi ma robit a nie je sanca na to, aby sa nieco poprehadzovalo, pokazilo, pretoze je zadane co to ma robit, ako to ma robit a v akom poradi
	AI Agents -> Ai agent si vybera poradie na zaklade system promptu, ktory mu zadame. Toto znamena ze ak je system prompt zle sformulovany, tak moze sa stat ze sa tam nieco prehadze a uz to nebude fubgovat tak, ako by malo 
***COST***
	AI Workflows -> su zadarmo, pretoze nemusime mat ziadne LLM, ktore rozmysla o dalsich postupoch a veciach, ktore musi robit dalej
	Ai agents -> vzdy, ked AI agent nad niecim musi porozmyslat tak nas to stoji peniaze tak isto ako ked musi pouzit nejaku toolku 
***Debbuging***
	Pri AI Workflowoch vieme presne urcit, co sa kde pokazilo a vidime co je zle a naopak pri AI Agentoch to poriadne nevidime, pretoze problem moze byt ci uz v system prompte, user prompte alebo v nejakej toolke


***PRIKLAD***![[49.png]]

V tomto pripade mozeme vidiet ze mame 2 sposoby, ako mozeme spravit Customer support. V prvom to je cez AI Agenta a v druhom to je cez AI Workflow. Ktory je lepsi?

***AI Agent*** -> zadali sme mu trigger (gmail sprava), spustil sa a musel zanalyzovat poziadavku, porozmyslat nad poziadavkou, pohladat relevantne informacie v databaze, porozmyslat nad tym co odpovie a odpovedat. Jeden AI agent ma dost vela uloh co v tomto pripade az tak velmi nevadi ale keby to bolo nieco zlozitejsie tak uz by sa mohol zacat mylit. Taktiez v nasom pripade pouzil toolku gemini 3x co znamena ze 3 krat nam bol za danu sluzbu uctovany poplatok. 

***AI Workflow*** -> Tak isto zacal na gmail trigger, potom to prejde na pinecone databazu, kde mame ulozene jednotlive veci ako faq, refoundy... z tade to vyberie najviac relevantne veci, ktore potom vyfiltrujeme a spojime ich dokopy. Text co dostaneme potom hodime do open ai modelu, ktory nam na zaklade pociatocneho mailu a naseho textu ktory mu dame sformuje odpoved v emailovej podobe a dalej to uz len posleme


