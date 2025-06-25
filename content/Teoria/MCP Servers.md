***Model Content Protocol***
(robime AI inteligentnejsie)

***PRIKLAD***
*************************
***user -> AI Agent*** (urob mi stretko s matom)

***AI Agent -> MCP server*** (ake toolky a resources mam k dispozicii)

***MCP server -> AI Agentovi*** (Toto su dostupne **resources** (napr. kalendar, lokacie, kontakty) a **tools**  (napr. create_event, book_table)

AI Agent -> LLM (Tu je poziadavka od pouzivatela a tu su dostupne resources/tools. Ktore mam pouzit?)

LLM -> AI Agentovi (Pouzi resource: 'dostupne casy v kalendari' a tool: 'create_event) 

AI Agent -> LLM (Tu su detaily (dostupne casy, kontakty atd.). co konkretne mam spravit?)

LLM -> AI Agentovi (Vytvor event: 'Kava s Matom', cas: utorok o 10:00, miesto: Espresso Bar, pozvat: Mato@example.com)

AI Agent -> Vytvori udalost v kalendari s danymi parametrami

Poskytuje nam rozne toolky a resources, ktore s ktorymi vie nas AI Agent pracovat. Je to efektivnejsie ako normalne toolky napriklad v n8n pretoze v MCP serveri je vsetko na jednej kope, nas AI Agent ma vsetky dostupne toolky a resources, s ktorymi moze pracovat a tak nie je obmedzeny na jendotlive toolky, ktore sme mu poskytli ale ma siroku skalu tooliek a resources, s ktorymi dokaze pracovat

https://www.youtube.com/watch?v=FLpS7OfD5-s



***Z CHATA***
### 🔄 **Ako funguje AI agent + MCP (nie NCP):**

1. **AI agent v N8N (alebo inom systéme)** je veľmi „ľahky“ – má iba základnú schopnosť komunikovať so serverom (MCP).
    
2. MCP **centrálne spravuje všetky nástroje, API konektory, prompty, recepty, datasety, kontexty**.
    
3. Agent pošle do MCP požiadavku:
    
    > "Tu je moja úloha. Pošli mi, čo všetko viem použiť."
    
4. MCP mu vráti:
    
    - ✅ Zoznam dostupných nástrojov (napr. „send_email“, „get_user_data“, „generate_invoice“)
        
    - ✅ Kontextové informácie, napríklad dostupné premenné, zdroje, alebo dátové štruktúry
        
    - ✅ Prípadne predefinované prompty alebo "recipes" na vykonanie činností
        
5. **LLM (napr. GPT)** vyhodnotí, ktoré tooly a aké kroky použiť a pošle späť plán.
    
6. Agent vykoná jednotlivé kroky podľa inštrukcií, pričom sa môže opakovane radiť s LLM.
    

---

### 🧠 Rozdiel medzi N8N bez MCP a s MCP:

|Funkcia|N8N bez MCP|N8N + MCP|
|---|---|---|
|Tooly|Statické (manuálne)|Dynamické (získané z MCP)|
|Kontext|Obmedzený|Centrálne spravovaný (široký rozsah)|
|Prompty a šablóny|Žiadne / ručne|Uložené a zdieľané v MCP|
|Flexibilita agenta|Obmedzená|Veľmi vysoká|
|Údržba|Manuálna|Centralizovaná (ľahšia)|