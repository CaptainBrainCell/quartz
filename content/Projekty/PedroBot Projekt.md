************************************
***PLAN*** -> Projekt, kde idem spojit viacero veci naraz. Pojde o AI Chatbota, ktory bude mat pristup k tymto poznamkam a bude vediet zodpovedat na otazky tykajuce sa tychto poznamok. Urobim mu vlastny interface, ktory sa bude podobat na chatgdp interface, kde sa bude uzivatel pytat otazky ( dam mu tam aj tieto poznamky nech vie na co sa ma zhruba pytat). Vysledkom by mal byt interface na webe, ktory je prepojeny s n8n a odpoveda na nase otazky ohladom nasich poznamok, ci uz ide o to, akych agentov sme robili, ako sme postupovali, co robia dany agenti a na koniec, ak este budem mat cas tak spravim aj dashboardu, ktora bude zobrazovat pocet poziadaviek, porovnavat to v jednotlivych dnoch ukazovat na kolko sprav uz nas chatbot odpovedal a taketo srandy. 

# POME NA TO
*************
***Postup***: 
***Prva cast*** (*neskor som menil*) -> vybral som si, ze zacnem s n8n funkcionalitou. V prvej casti som urobil agentov, ktory budu pracovat s databazou a neskor aj s lovable.  Zacal som ai agentom, ktory analyzuje to, o co nas user ziada, a na zaklade toho vyberie adekvatne informacie z nasej databazy, prepise ich, nech su napisane trosku formalnejsie. V databaze zatial nic nemame. Ale mame hotoveho agenta, ktory nam vyberie info a poskytne ho uzivatelovi.  Ako memory som mu tam zadal Postgres, pretoze je stabilnejsia a lepsia ako obycajna windows buffer memory a ako databazu som vybral superbase pretoze native vektorova databaza v n8n nie je dobra a preco som nevybral pinecone? Pinecone som nevybral lebo na taketo mensie projekty, kde je viac datovych formatov je lepsia Superbase. A taktiez ak budem chciet tak superbase databazu si viem hostnut aj ja na rozdiel od Pinecone. A ako system message dostal toto [[pedrobot_system_message]]

![[74.png]]
**********************************
***Druha cast*** (*neskor som menil*) -> dalej som vytvoril workflow, ktory nam bude vkladat informacie do nasej Superbase databazy. Funguje to tak, ze ked do pricinku na google disku vlozime nejaky novy subor tak sa to aktivuje, nastavi nam to metadata, ak by sme s tym v buducnosti chceli narabat, rozdeli jednotlive subory a hodi nam ich do nasej databazy.

![[75.png]]
*******************************************
**Tretia cast** -> uz ked mam vymyslene, ako budem davat veci do databazy tak teraz tam tie veci aj mozem zacat davat aaaaaale tu nastava zadrhel. Ja mam vsetky poznamky v obsidianovej forme co je .md a su tam aj obrazky, prekliky a ostatne blbosti. A vyriesil som to tak, ze som si spravil skriptik, ktory zoberie jednotlive priecinky a subory z naseho obsidianu a odstrani z tade markdown, vsetky obrazky a ostatne veci 

```python
import re
from pathlib import Path

#  Vault cesta
VAULT_PATH = Path(r"C:\Users\peter\OneDrive\Desktop\Vysualist\Visualyst Starter\Brigadka")
OUTPUT_PATH = VAULT_PATH / "cleaned_texts"

#  Vytvor výstupný priečinok
OUTPUT_PATH.mkdir(parents=True, exist_ok=True)

def clean_markdown(content):
    content = re.sub(r'!\[\[.*?\]\]', '', content)  # odstráni obrázky ![[img.png]]
    content = re.sub(r'\[\[.*?\]\]', '', content)   # odstráni interné odkazy [[note]]
    content = re.sub(r'\[([^\]]+)\]\([^)]+\)', r'\1', content)  # odkazy [text](url)
    content = re.sub(r'[*_~`>#-]', '', content)     # markdown znaky
    content = re.sub(r'\n{2,}', '\n\n', content)    # zredukuje viac prázdnych riadkov
    return content.strip()

def process_all_notes():
    md_files = list(VAULT_PATH.rglob("*.md"))
    if not md_files:
        print("❌ Nenašli sa žiadne .md súbory.")
        return

    for md_file in md_files:
        rel_path = md_file.relative_to(VAULT_PATH)
        output_file = OUTPUT_PATH / rel_path.with_suffix(".txt")
        output_file.parent.mkdir(parents=True, exist_ok=True)

        with open(md_file, encoding="utf-8") as f:
            raw = f.read()
        cleaned = clean_markdown(raw)

        with open(output_file, "w", encoding="utf-8") as f:
            f.write(cleaned)
        print(f" Vyčistené: {rel_path}")

if __name__ == "__main__":
    process_all_notes()
    print("\n HOTOVO! Vyčistené poznámky sú v priečinku 'cleaned_texts'")
```

Ked som ten skript spustil tak nam zostalo nieco taketo: 

![[79.png]]

***Stvrta cast*** -> Tym ze sme si vytvorili agenta, ktory nam automaticky pridava veci do databazy sme si teraz celkom ulahcili robotu pretoze nemusime to teraz vsetko rucne nahadzovat do databazy ale staci nam, ked si nas workflow aktivujeme a vsetky subory co chceme nahadzeme do naseho priecinku na google disku. Ked to spravime tak uvidime, ze postupne nam to bude nas workflow nahadzovat do databazy. Ked bude hotovy mali by sme vidiet nieco taketo : 

![[80.png]]

tu uz vidime ze jendotlive subory co sme si hodili do priecinka na google disku sa dostali do databazy, to znamena ze nas workflow na pridavanie suborov do databazy funguje (jupiiiiiii) a tymto sme si usetrili celkom dost casu. 

*************************************************************
# Zistil som dalsie veci

***Nasa databaza je velmi jednoducha na taketo veci***
Workflowy, ktore som spravil vyssie boli prilis jednoduche a nemali dostatocnu funkcionalitu na narocnost tejto ulohy. Taaaaaakze som musel hladat, ako to vylepsit. Nasiel som sposb. Nasiel som temlpate, ktory som si stiahol na databazu a podla videa, co bola ku nej (https://www.youtube.com/watch?v=mQt1hOjBH9o) som tento template upravil tak, aby co najepsie splnal nase poziadavky.

***Riesenie***
Template co som si stiahol som si rozlozil na 2 casti:
*****************************
## VEDLAJSIA  (nahadzovanie veci do DB)
![[95.png]]
Tato sekcia sluzi na nahadzovanie veci do DB a pripadne ich upgradnutie (ak pridame nove lebo stare su uz neaktualne). Taktiez nam to umoznuje pridavat xsxl prvky a cvs prvky co su vlastne tabulky, co by bolo pred tym mozne ale teraz ich vie nas agent aj poriadne spracovat. Funguje to tak, ze ako trigger mame nastaveny google docs dalej sa nam to loopuje, az kym neprejdeme cez vsetky polozky (sluzi na to, ak chceme nahodit do DB viac veci naraz ) potom nam to nastavi id -> na metadata, ktore nasledne skontroluje a ak sa uz ten jeden file v databaze nachadzal tak ho to vymazalo (aby sa tam mohol nahrat novy a aby sa tam nemiesali stare a nove informacie). Dalej nam to do naseho file hodi metadata ( na neskorsiu manipulaciu ), stiahne nam to subor a prejde do swichu, kde sa rozhodne, ci ide o pdf/txt/xsls/csv. Ked sa to rozhodne tak swich posle tieto subory na trasu, kam patria. Nasledne sa dane informacie spracuju a postnu sa do DB. V DB mame potom v table editore tieto moznosti :
				![[96.png]]
*****************
				
V ***document_metadata*** mame id ku kazdemu dokumentu, pre lepsiu manipulaciu, ci uz ide o mazanie, updatovanie alebo o samotne vyhladavanie v DB
![[97 1.png]]
**************

V ***document_rows*** mame zapisane vsetky rows z tabuliek ci uz ide o xsls alebo csv, ktore sme si zapisali do samostatnej tabulky pre lepsiu manipulaciu
	![[98 1.png]]ng]]
*************

V ***ducuments*** mame vsetky dokumenty, ktore mame dostupne a ktore sme si hodili do nasej DB
![[99.png]]
***********

A ***n8n chat history*** asi nemusim vysvetlovat :D

***Ale tu som prisiel na dalsi problem*** -> ked som si to prehadzoval na .txt cez [[script]] tak mi to kompletne zdemolovalo strukuru mojich suborov. Ako som to vyriesil? No predsa dalsim scriptom :DD [[script2]], ktory my do mohich poznamok navratil aku taku strukturu a potom som vytvoril dalsi script, ktory mi tieto .txt subory premeni na .docx [[script3]] aby som ich nasledne mohol prehodit do PDF ka. To som spravil cez https://www.ilovepdf.com/. ***A preco som to nenechal v .txt formate?*** Lebo som prisiel na to, ze ked to vkladam ako pdf a nie ako .txt tak to je potom prehladnejsie a nas agent to vie lahsie vyhladat a pracovat s tym. Ked uz som toto vsetko spravil -> sposob ako nahodit veci do databazy, sposob ako pretransformovat jednotlive subory tak, aby boli co navyhodnejsie a realne to do tej databazy aj dal, tak potom som mohol ist na dalsiu cast.


## HLAVNA
![[94.png]]
V hlavnej casti sa nachadza nas AI agent, ktory odpoveda na webhook, hlada informacie v databaze, vytvara odpovede a odpoveda.
Tu ma nas AI agent pristup k viacerym toolkam, co mu umoznuje lepsie pracovat s informaciami, hladat informacie a presnejsie odpovedat. Nastavil som to tak, aby to bolo dobre pre nase potreby a uz teraz by mal fungovat tak, ako by mal. Pripojili sme webhook, ktory je napojeny na lovable, dalej sme nastavili ID -> postgres a zapisovanie chat history, pripijili sme mu toolky a napojili sme ho na databazu, kde uz ma nahadzane informacie z predosleho kroku. 


## USER INTERFACE
******************
Cez lovable som si vytvoril taku jednoduchu user interface, ktora je minimalistica a pekna. Tu sa bude user pytat na otazky a cela konverzacia bude prebiehat tu. Upravil som si to tak, aby sa mi to pacilo a pridal som tam nejake prvky ako napriklad ***MOZES SA MA OPYTAT NA TOTO***, na ktore ked user klikne tak ho to prehodi na podstranku, kde si bude moct pozerat vsetky tieto poznznamky alebo ***AKO SOM BOL STVORENY*** na ktore ked klikne tak uvidi tento dokument, kde zapisujem co som robil, ako som robil .... ![[100.png]]