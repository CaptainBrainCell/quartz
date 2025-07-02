***************************************
## Ako Rag funguje?
************
![[110.png]]

Zoberieme nejaky text alebo subor co mame a rozdelime ho na niekolko cast. Tieto casti potom pomocou embedding modelu naukladame na specificke miesta do databazy. Cize nase "vedomosti" zostanu ocislovane. Potom, ked mu zadame nieco, co chceme vediet, tak on si zoberie klucove slovo, da ho do databazy a potom z nej naspat vytiahne niekolko najblizsich dokumentov. 

![[111 1.png]]

Cize mi mu dame napriklad subor zaklad psieho chovu. On si ten subor rozdeli a na zaklade toho, co v nom je tak si to ulozi do databazy. A potom ked pride user a opyta sa napriklad V kolkych mesiacoch ma zacat psa trenovat. On zoberie klucove slovi -> trenovat a vlozi ho do databazy. Potom si z databazy nevitiahne len to slovo trenovat ale aj nejake tie rozdelene casti s tym. Potom tie jednotlive casti zanalyzuje a vytvori z nich odpoved. 


