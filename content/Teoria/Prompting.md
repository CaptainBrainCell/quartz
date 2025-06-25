Je to 80 % z celeho systemu. Ak su zle nastavene prompty tak to poriadne nefunguje. 
***Proaktivne vs Reaktivne prompty***
	Proaktivne -> su to prompty, ktore si vytvorime a potom ch cele hodime do naseho agenta. Tato metoda promptovania nie je uplne idealna, pretoze kazdy jeden agent to musi mat nastavene inak a nie je 1 prompt, ktory by sedel vsetkym. A este tym, ze tam hodime cely ten prompt naraz, tak nevidime, co nam moze sposobovat errory, co nam stazuje nasledny debugging.
	Reaktivne -> su to prompty, ktore si postupne do naseho agenta doplname tak, ako portrebujeme. Ak pridame nejaku toolku a nas agent to nevie poradne pouzivat, tak mu to napiseme do system promptu. Tu mame vyhodu ze ako nahle sa nieco pokazi tak presne vieme, kde a co sa pokazili, pretoze cely nas prompt si piseme postupne 
	
***Ako reaktivne promptovat?***
-> Najskor zacni bez promptu a na zaciatok mozes napisat napriklad ##Overview, kde popises, co ma ten agent vlastne robit a co je zac (napr. 
## Overview
You are a prompt-making assistant. Your task is to take the user's input and analyze the PDF. Based on the analysis, generate a useful prompt.)
-> potom, ked pripojis nejaku toolku, pozri, ci ju vie agent pouzit bez toho, aby si mu to nakazal v system prompte (napr. pripoj k agentovi email agenta a pozeraj, ci ho vie bez promptu pouzit spravne)
-> pridavaj prompty na zaklade errorov. Ked nieco nefunguje tak, ako by malo tak napis do system promptu pravidlo, ktore to opravi a pripadne aj nejaky priklad (napr.

- Input: send an email bob asking him what time he wants to leave
1) Action: Use ‘contactAgent’ to get bob's email. Send this email
address to the ‘emailAgent’ tool.
2) Action: Use ‘emailAgent’ to send the email.
- Output: The email has been sent to bob. Anything else I can help you
with?)

-> debuguj postupne, najskor jeden error, potom druh. Ak budes chciet riesit vsetky naraz tak sa z toho zblaznis a nic nevyriesis

***Ako by to malo fungovat?***
-> pridaj toolku
-> pridaj nejaku vetu do system message o tejto toolke
-> skus niekolko scenarov
-> ak vsetko funguje tak, ako ma tak pridaj dalsiu
-> ak to nefunguje tak, ako by malo tak to zahardocuj do agenta (## Rules)
-> testuj dalsie scenare
-> opakuj

***Najdolezitejsie komponenty promptu***
-> Background  : tu definujeme kto ten AI Agent vlastne je a jeho ciel
               nastavujeme tu jeho spravanie a pristup
               bez tohoto AI nema smer, je strasne suchy a prilis vseobecny
               
        pr. ## Role
			You are a [role] AI agent designed to [specific purpose]. Your goal is
			to [main objective].
			
-> Tools : tu definujeme, ake toolky ma AI Agent a vyber a kedy ich ma pouzit
		zabezpecime tym ze nas agent pouziva spravne toolky na spravne veci
		dobre urobena tools sekcia robi agenta viacej efektivnym
		
	pr. ## Tools Available
			1. **Google Search** - Use this when the user asks for real-time
			information.
			2. **Database Lookup** - Use this to retrieve past customer orders.
			3. **Email Sender** - Use this when the user wants to send a message.
			
-> Instuctions (rules) : sem zadame presne pravidla, podla ktorych sa ma nas agent spravat
				   zadavame tu poradie, v ktorom ma AI konat
				   prevencia nedorozumeni
	
	pr. ## Instructions
			4. Always greet the user politely.
			5. If the user provides incomplete information, ask follow-up
			questions.
			6. Use the available tools only when necessary.
			7. Structure your response in clear, concise sentences.
		
->Examples : tu ukazeme ai agentovi, co vlastne chceme na real life priklade
		    dame mu priamy navod ako sa ma v danych situaciach zachovat
		    presnejsie a konzistnetnejsie outputy
		    
	pr. ## Examples
			### Input:
			"Can you generate a trip plan for Paris for 5 days?"
			- Action: Call the **Trip planner tool* to get XYZ
			- Action: Call the **email** tool to send the itinerary
			### Expected Output:
			"Here is a 5-day Paris itinerary:
			- Day 1: Eiffel Tower, Seine River Cruise...
			- Day 2: Louvre Museum, Notre Dame..."
		
->Final notes & reminders : sem zadavame pestre ale dolezite pripomienky
				        moze tu byt napr : dnesny datum/ limity/ specificke poziadavky na formatovanie
				        
	pr. ## Final Notes
		- Always format responses as a Markdown list when possible.
		- Today’s date: {{CURRENT_DATE}}
		- If unsure about an answer, say: "I don't have that information."