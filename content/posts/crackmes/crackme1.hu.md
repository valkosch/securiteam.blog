+++
title= "Program visszafejtés - No. 1"
date= 2024-07-17T17:53:55+02:00
draft= false
toc= true
summary = "Bevezető a reverse engineering világába, ahol kezdő szintű program visszafejtős feladatokat oldok meg, bemutatom a technikákat és az alapokat. Csak annyit tudok mondani hogy: \"Ki itt belépsz, hagyj fel minden reménnyel.\""
description = "Mai világban egyre több dolgot kapunk meg kézhez anélkül, hogy tudnánk mi történik valójában a motorháztető alatt. Ez kényelmes és egyszerű, de ekkor teljesen meg kell bíznunk a készítő szaktudásában és szándékában, hiszen nincs módunkban ellenőrizni a helyességet. A reverse engineering tud ezen segíteni."
tags = ['reverse', 'crackme', 'programming']
+++

## Bevezető

Az informatikában sok helyen hasznos lehet ha az ember beláthat a színfalak mögé. Ilyen például amikor egy kártékony programmal megtámadnak egy céget, és a hatékony védekezés érdekében, a blue team oldalán, meg akarjuk tudni az ellenség módszereit, taktikáit és procedúráit. Ekkor az említett programot elemeire kell bontani, szét kell cincálni. Ugyanakkor előfordul, hogy a másik oldalon, a red team-ben vagyunk, amikor is teljesen nemes szándékkal vezérelve, és engedéllyel a birtokunkban, megpróbálunk hibát keresni egy programban, hogy aztán azt kihasználva, érvényesítsük céljainkat. 

Nem lehet jobban hangsúlyozni a program visszafejtéssel elsajátítható készségek fontosságát az informatikában. A kiberbiztonság kétségkívül egy olyan ág, ahol ez különösen igaz. Nem véletlen, hogy a 3 betűs amerikai ügynökségek bizzilión mennyiségű dollárt fektetnek be ebbe az ágazatba.

Ebben a kis cikk sorozatban vissza fogok fejteni egy pár "crackme"-t a lehető legegyszerűbb eszközöket használva. A programok a [crackmes.one](https://crackmes.one) oldalról vannak, ahova edukációs célokkal tudnak az emberek programokat feltölteni, amiket mások meg gyakorlásként fel tudnak törni.

![crackmes](/crackmes.png)

## Kis alapozás

Egyáltalán miért is olyan fene nehéz visszafejteni a programokat? A világon több gazdasági modell is létezik a program fejlesztésre, de általában azt szoktuk mondani, hogy egy program vagy zárt vagy nyílt forráskódú.

A zárt programokat általában cégek finanszírozzák és fejlesztik, majd a végeredményt értékesítik. Ekkor a forráskódot nem teszik közzé, hiszen az egyenlő lenne azzal, hogy ingyenessé teszik a programjukat.

Ezzel ellentétben a nyílt forráskódú programok, mint például a linux kernel, a közösség álltal vannak fentartva, ingyenesek, bárki hozzájárulhat és fejlesztheti őket.

Most nem megyek bele ebbe a vitába, mindkét oldalnak megvannak a saját érvei, de annyit elmondanék, hogy biztonsági szempontból nézve a nyílt forráskódú projektek szerintem jobb helyzetben vannak mint a part túloldala. Az ok erre egyszerű, a zárt programok kódját visszafejtés nélkül nem lehet ellenőrizni. Az egész rendszer a "trust me bro" elven működik, mivel a felhasználónak semmi hatalma nincsen arra, hogy megnézze valójában mit is csinál a progam. Ez abból a szempontból is kicsit gáz, hogy így nem csak abban kell bízni, hogy a fejlesztők szakmailag tökéletes munkát végeznek, hanem abban is, hogy a program tényleg semmi mást nem csinál az állítottakon kívül, hiszen ha úgyse tudja ellenőrizni ezt senki, és ráadásul még így lehet több profitot termelni, akkor miért is ne csinálnák? Láttuk már, hogy ekkor mi történik (khm Facebook per). 

Tehét most olyan C/C++ nyelvben írt programokkal fogunk foglalkozni, amiknek a forráskódja nem adott, csak is egy futtatható állomány az egész. Ez az executable gépi kódból áll, hiszen az emberi nyelven írt kódot a fordító lefordítja a számítógép nyelvére, és ha explicit módon nem adtuk meg a fordítónak paraméterként, hogy hagyja benne a szimbólumokat, akkor egyáltalán nem lesz kölcsönösen egyértelmű ez a leképezés, kód szempontjából. Egy szó mint száz, nem fogjuk tudni a forráskódot olvasni. Azt megemlítem viszont hogy vannnak olyan csúcsszuper "decompiler"-ek, mint a [Ghidra](https://github.com/NationalSecurityAgency/ghidra), amik így is nagyjából ki tudják találni, hogy mire gondolt a költő, amikor a programot írta, de ezekre most szerintem nem lesz szükségünk.

Most relatíve egyszerű és kezdőknek szánt feladatokat fogok megoldani, de hogy teljesen megértsd a megoldásokat, ahhoz C/C++ illetve Intel x86-64 assembly tudás az elengedhetetlen.

Az alábbiakat CLI programokat fogom használni:
* **objdump** - az assembly kódját tudjuk ezzel olvasni a programnak
* **ltrace** és **strace** - a program futása közben leköveti milyen dinamikus könyvtár hívások történnek (pl printf vagy strcmp) 
* **gdb** - egy svájci bicska ami minden programozónak a legjobb barátja, debugger szoftver de visszafejtésnél is szuperál
* **strings** - kis egyszerű eszköz, ami kiírja az összes kiírható karaktert amit megtalált az állományban

Fontos óvintézkedés mielőtt még bele lendülünk, hogy érdemes egy virtuális gépen csinalni ezeket a crackme-ket, mivel annak ellenére, hogy az exe-ket ellenőrzik feltöltés után, mégiscsak egy ismeretlen programot készülünk futtatni. A virtuális környezet pedig elszeparálja ezt a programot a valódi rendszerünktől, így biztonságosabbá téve ezt az egészet. Arra viszont figyeljünk, hogy még ez sem teljesen atombiztos, mivel ha van például megosztott mappa a virtuális gépünkben akkor máris van híd a kettő rendszer között. Illetve ismertek már olyan vírusok is, amik képesek kilépni a virtuális gépből, bizonyos körülmények között. Mindenesetre ezek közül valószínüleg egyse fenyeget minket, de ez az elővigyázatosság illetve ez az "adversary mindset" tehát, hogy a legrosszabbra készülünk, hasznos rutin amikor az ember kiberbiztonságban érdekelt.

## Visszafejtés

Virtuális gépnek a [VirtualBox](https://www.virtualbox.org/)-ot használom, amin van egy csupasz Arch Linux a felsorolt eszközökkel.

### Az első:
#### [D4RKFL0W's Easy_firstCrackme-by-D4RK_FL0W](https://crackmes.one/crackme/5c8e1a9533c5d4776a837ecf)

Letöltötjük és futtatjuk, csak hogy lássuk mivel van dolgunk.

![fut](/crackmes2.png)

Mint ahogy látszik egy jelszót kell valahogy kitalálnunk vagy éppenséggel kikerülnünk, és a jelszó az nem az almafa :((.

Próbáljuk rajta ki az eszközeinket:

Először nézzük az ltrace-t. Nagyon sokra nem mentünk vele, semmi érdekes nem látszik benne, csak a kiíratás folyamata. Ha például a beolvasott jelszót úgy ellenőrizte volna a program, hogy strcmp()-t használ, akkor azt itt láttuk volna.

![ltrace](/crackmes3.png)

Az objdump már annál hasznosabbnak bizonyult. Megkeressük a main blokkot, mivel valószínüleg ott történik a jelszó helyességének a vizsgálata. Itt már látszik egy kis minta, amit ki is emeltem a képen. A beolvasott jelszót karakterenkét ellenőrzi a program cmp operátorral. A "je" utasítás az a "jump if equal"-nak felel meg. Tehát ha a karakter egyenlő cmp-ben a hexadecimálissal, akkor átugorja a programazt a függvényhívást ami valami "failed" metódust hív meg. Az összehasonlításban használt hexadecimális értékeket ASCII karakterre fordítva és konkatenálva megkapjuk a kívánt jelszót: H1DD3N. Yey!

![objdump](/reverse_help.png)

Megjegyezném, hogy karakterenként soha sehol sem érdemes jelszót ellenőrizni, vagy ha muszáj akkor figyeljünk oda arra, hogy konstans idő legyen az ellenőrzés, máskülönben támadható lesz a program side-channel támadással. Például ha van egy 4 jegyű beléptető PIN terminál ami egyből visszatér "Rossz jelszó!"-val miután beírtuk az első számjegyet, akkor brute force-nál 10.000 próbálkozás helyett elég lesz csak 40 próbálkozás. 

### A második:
#### [D4RKFL0W's crackme2-be-D4RK_FL0W](https://crackmes.one/crackme/5c95646333c5d46ecd37c960)

Ez lesz sem sokkal nehezebb első látszatra, itt is jelszót kell megfejtenünk.

A strings, ltrace, strace itt sem hasznos sokra. Az objdump-ot nézve sem találtam nagyon semmi triviális megoldásra vezetőt, de azt láttam hogy van a programban egy olyan blokk hogy \_Z14check_passwordPc ami talán érdekes lehet a számunkra. Visszont passzív módon nem lehetet csak úgy az assembly kódból kiolvasni a jelszót mint előbb, valószínüleg a kolléga szándékosan úgy csinálta, hogy a program valamelyik eldugott részébe strcat()-tel futás időben állítja elő a jelszót karakterről karakterre.

Tehát futás közben kell végig lépkedünk azon, hogy mi is történik valójában. Erre fogom használni a **gdb**-t. A gdb önmagában kicsit csupasz ezért ajánlom neked is, hogy kicsit tuningold fel a [GEF](https://github.com/hugsy/gef)-fel, hogy egyszerűbb legyen a szomszéd néni wifijének a feltörése (viccelek).

Tehát amit csinláltam:
```sh 
gbd -q crackme2-be-D4RK_FL0W
```
Elindítjuk a gdb-t.

```sh
set disassembly-flavor Intel
break *_Z14check_passwordPc
run 
```
Már nem emlékszem, hogy mi a default disassembly-flavor de én jobban preferálom az Intel féle syntaxot ezért állítottam be arra. Majd breakpoint-ot raktam a már említett függvényhez, hogy a programfutása ott megálljon, és megtudjuk vizsgálni közelebről is. A run-nal pedig elindítjuk a programot.

![gdb1](/crackmes4.png)

Ez a felület fogad minket. Gyors hogy miket láthatunk: 
* felül ott van mind a 16 db 64 bites regiszter amikkel a számítógép dolgozik (igazából sokkal több regiszterrel dolgozik a számítógép, hogy minél több párhuzamosítást tudjon végrehajtani, csak az architektúra utasításkészlete 16 regiszterre lett kitalálva, és ettől eltérni nem lehet, mivel akkor nem lenne visszafelé kompatibilis és így a régi programok nem müködnének, ezért a processzor belül ezt a 16 regisztert képzi le az összes szabad szerintem több ezer regiszterére).
* alatta ott van a jól ismert stack
* utána az assembly utasítások, zölddel jelölve az épp következő utasítást

a többi annyira most nem érdekes számunkra

Itt már kiszúrja a szemünket valami, egész pontosan az r8 regiszterben ez a "isAAthisFunBBCCD". Potenciálisan megvan már a győztesünk, de azért kutakodjunk tovább. Amikor ilyennel foglalkozik az ember akkor általában teljesül az, hogy ha valami túl jó hogy igaz legyen akkor tényleg az. Ezt csak azért mondom mert ezekben a feladatokban többszőr előfordul, hogy szándékosan tesznek bele "honeypot"-ot, aminek kb a magyar megfelelője a mézesmadzag. Ezeket szándékosan könnyen megszerezhetővé teszik, hogy az emberek bedőljenek neki. Ennek van valós gyakorlati haszna is, rengeteg rendszerbe helyeznek el ilyen honeypot-okat, csalikat, hogy a támadó fél erőforrását, idejét vesztegesse rajta fölöslegesen. Olyakor még úgy is tud viselkedni, mint egy viharjelző, tehát ha valaki elkezdi támadni a honeypot-ot, akkor értesítést küld a védekezőknek, így több időt nyerve nekik a védelemben való felkészülésnél.

Vissza feladathoz, lépkedjünk a programban, ezt a **stepi** és **nexti** utasításokkal lehet a gdb-ben megcsinálni. A stepi és a nexti közötti különbség az az, hogy az egyik belelép a függvényhívásoknál a függvényekbe a másik pedig csak átugorja. Amikor a saját programodat debuggolod, akkor ugyanazek müködnek csak a végén az i nélkül. Gondolom az i az "instruction"-nek felel meg, mivel itt assembly utasításokról van szó.

Ekkor real-time látszik a program futása és a regisztereken a változás.

![gdb2](/crackmes5.png)

Itt látszik már, hogy tényleg a feltételezett megoldásunk lesz a jó, mivel a program....
1. az **rax** regiszterbe a 0x69("i") értéket rakta 
2. az **rdx** regiszterbe a 0x74("t") értéket rakta 
3. majd ezeknek az alsó 8 bitjét hasonlította össze **cmp dl, al** utasítással

Tehát itt is karakterenkét hasonlítja össze a program az általunk adott jelszót, ami most a "test" volt, a programban előállított "isAAthisFunBBCCD" -vel. Ez a megoldás.

Ennyi volt ez a kis bevezető, ha ijesztő az assembly számodra, nem vagy egyedül, de csak bíztatni tudlak, mivel megérteni talán még annyira nem nehéz és mellette kifizetődő, főleg ha érdekel a program visszafejtés.
