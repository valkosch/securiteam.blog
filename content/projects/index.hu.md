+++
title = '::Projektjeim::'
date = 2024-07-13T20:48:48+02:00
draft = false
+++

Itt találod az összes megemlítésre érdemes munkámat, pet projektemet, amiket az évek során csináltam. Ezeket vagy autodidakta módon, vagy a tanulmányaim keretei között csináltam szabadidőmben.

## Fraktálfa és egyéb kis programozás feladatok 

### cirka 2022

Szárny próbálgatás jelleggel programoztam, főképp hogy ismerkedjek a nyelvekkel és hogy kiéljem kreatív vágyaimat. Ennek az időszaknak egyik szüleménye ez a fraktálfa generátor, ami **nagyon** nem volt hatékony, de legalább korán megismerkedtem a forgatási mátrix hasznával :)).

![Fraktálfa1](/fraktalfa1.png)
![Fraktálfa2](/fraktalfa2.png)

## Arduino öntözőrendszer 

### 2022 november

Szerettem volna valami gyakorlatias és kézzel foghatót készíteni, ezért csináltam egy öntözőrendszert egy talajnedvesség szenzor, egy Arduino Uno, egy relé és egy kis 5V-os vízpumpa felhasználásával. A logika mögötte nagyon egyszerű, pollinggal lekérdezi bizonyos időnként a szenzor értékét az Arduino, amit leképzek 0-tól 100-ig terjedő skálára, és ha 50 alá esik az érték akkor beindul a locsolási folyamat. Tervben volt, hogy locsolás közben zenét is játszon a növénynek egy kis hangszoró és egy SD kártya segítségével, mivel bizonyos (klasszikus és jazz) zenék frekvenciája akkora, hogy a növények pólusait kinyitják ezáltal több oxigént kapnak. Meg funkyn nézett volna ki.

![Locsolo](/locsolo1.jpg)

## Napelemes esti fény 

### 2023 május

A koncepció alapvetően annyi, hogy a napelem nappal tölt egy litium-ion akkumulátort, este pedig erről az akksiról müködik egy LED. Szerintem szép megoldás lett, mivel semmilyen fény szenzort nem használtam, csupán a napelemből származó visszaáramot és egy tranzisztort.

![napelem1](/napelem1.jpg)


### 
Müködés közben

![napelem2](/napelem2.jpg)

### 
Kapcsolási rajzom hozzá

![napelem3](/napelem3.png)

## Útvonaltervező

### 2023 szeptember-december

[Github repo](https://github.com/valkosch/A_Star.git)

![utvonaltervezo](/utvonal1.png)

Ez a Programozás 1 tantárgy nagy házi feladatának készült az első félévben.

A célja ennek a tantárgynak az volt, hogy megismerkedjünk a C nyelvvel, és úgy érzem, hogy ezt sikerült is elérnem és be is mutatnom ezzel a projekttel. Amikor ki kellett találnom, hogy mit készítsek, az lebegett a szemem előtt, hogy valami gyakorlatiasabb, valóságban is hasznosat csináljak, ne pedig egy cuki telefonkönyvet vagy miegymást.

Ez a progam egy útvonaltervező Budapest egy kis téglalapján, amin az A* algotimus segítségével, meg tudja találni a felhasználó 2 pont között a legrövidebb, vagy a leggyorsabb utat. Persze az autókat, dugókat és a forgalmat nem veszi számításba. A kódbázis egész részét én készíttem, egyedül az SDL grafikus könyvtárat importáltam.

A térkép az az OpenStreetMap-ről lett lekérve, egy kis *makegraph.py* python kód használatával, ami az OSM-től elkéri a térképet, majd gráfot készít belőle, aminek az adatai két külön .txt fájlba lettek szétszedve (élek és csúcsok halmaza).


### How to 

Ha kiszeretnéd próbálni 2 lehetőséged van:
1. [Itt](https://entercomgroup.hu/23-24/Astar/index.html) ki tudod próbálni minden erőfeszítés nélkül, mert a kedves kollégák feltöltötték nekem webassemblyvel.
2. Ha esetleg a fenti opció nem müködik, vagy éppenséggel kiváncsi vagy a forráskódra (sajnos itt még nem jöttem rá az inline dokumentáció szépségeire, ezért aligha van dokumentálva a kód, de ha kérdésed van válaszolok szívesen), akkor az alábbi lépésekkel tudod ezt megtenni:

#### Letöltés

Először klónold a git repo-t.
```sh
git clone https://github.com/valkosch/A_Star.git 
```
Utána vagy tudod futtatni az általam biztosított binaryt miután beléptél a klónozott könyvtárba:
```sh
./astar
```
Vagy akár a saját futtatható állományodat is lefordíthatod. Ehhez először töltsd le a függőségeket. Az Arch Linuxomon ez az alábbi paranccsal tehető meg, de biztos vagyok, hogy a saját csomagkezelődnél is találhatsz hasonló csomagokat. Ha Windows-t használsz, ne használj Windowst :)
```sh
sudo pacman -S sdl2 sdl2_gfx sdl2_image sdl2_ttf
```
Ezek után a projekt könyvtárban lefordítod a forráskódot:
```sh
gcc *.c -o myexec `sdl2-config --cflags --libs` -lSDL2_gfx -lSDL2_ttf -lSDL2_image -lm
```

#### Használat 

A program indítása után az alábbiakat kell tenned:

1. Kiválasztani 2 pontot a térképen. Ezt megtehed 2 különálló
bal kattintással. Sikeres kiválasztásnál a program a
kattintás helyén elhelyezett piros pöttyel jelez vissza.

2. A jobb oldali kezelőfelületen kiválasztani melyik súlyozás
szerint fusson az útkereső algoritmus. A leggyorsabb utat,
ahogyan a gomb színkódolása is mutatja, piros színnel, a
legrövidebb utat pedig kékkel rajzolja ki majd a program. Nem
kizáró vagy a 2 gomb közötti kapcsolat, midkettőt ki lehet
választani egyszerre is igény szerint.

3. Ha megfelel a pontok választása és a súlyozás is, a „GO”
gombbal tudod indítani az útvonaltervezést. Ezután a
térképen kirajzolódik a kívánt út / utak.

A program futása alatt bármelyik időpillanatban törölheted a
kiválasztott pontokat és a kirajzolt utakat az egér jobb
kattintásával, és alaphelyzetbe állíthatod ezzel a térképet.
Ezután újra adhatsz már új kezdő és végpontot. Ugyanakkor a
térkép jobb alsó sarkában elhelyezett ikonra kattintva
szintúgy bármikor tudsz váltogatni a forgalmi és a műholdas
térkép nézet között.

## REVIS

### 2023 december

[Github repo](https://github.com/valkosch/REVIS.git)

Láttam [ezt](https://www.youtube.com/watch?v=4bM3Gut1hIk&list=PLUyyOw61zxiJXMihb4PjYbGHEgdGxMuY3&index=2) a videót, aminek a lényege az volt, hogy a bináris fájlokat megpróbáljuk vizualizálni, mivel sokkal könyebben találhatunk bennük ezáltal visszafejtés során mintákat, mintha hexadecimális számok sorozatát néznénk. Egy másik fontos mondanivalója az volt, hogy mindegyik visszafejtő program csak egy adott fájl típusra működik, tehát nincsen univerzális megoldás. Ebből inspirálódva megpróbáltam én is megcsinálni a videó ötlete alapján a saját ilyen programomat C-ben.

A koncepció mi sem egyszerűbb: a fájljaink bájtokból állnak, amik 2 számjegyű hexadecimális számoknak felelnek meg. Egy bájt értéke legalább 0 és legfejlebb 255. Alkotunk egy koordinátarendszert ahol az x és y is eleme a [0;255] intervallumnak. Ez után páronként értelmezzük a bájtokat egy adott fájlban, ahol az első tag mindig az X koordináta, a második pedig mindig az Y. 

Ez alapján a programom is csak annyit tesz, hogy beolvassa bájtonként a fájlt és egy 255x255 mátrixban eltárolja, hányszor fordult elő már az adott pont, azaz bájt pár. A maximum előfordulás lesz a teljesen fehér pixel, a 0 pedig a fekete, a többi pedig ezeknek az árnyalatai. Így kapunk a végén egy 255x255 méretű .png kiterjesztésű képet, ami reprezentálja visszafejtett fájlt. Sehol nem használtuk ki, hogy a fájl milyen típusú, ezáltal univerzálisan jó lesz minden, olyan fájlra ami 0-sokból és 1-sekből áll, de szerencsénkre más fajta fájlt nem is ismerűnk a sima számítógépek világában.

Példák:

### Szöveg

![ascii](/ascii_revis.png)

Csak a képet látva, már el lehet dönteni, hogy ez egy szöveg fájl, és azt is, hogy nem magyar nyelvű, mivel csak ASCII karaktereket tartalmaz. Honnan tudom? Az ASCII karakterek decimélis értéke 0-tól 127-ig tart, és ugyanez látszik a képen is, hiszen csak a felső negyedben vannak fehér pixelek azaz pontosan 0-127-ig voltak benne bájtok X és Y tengelyen is.

![nonascii](/nonASCII_revis.png)

Itt egy másik példa, ennek ugyanaz a formája de most már nem csak ASCII karakterek vannak benne, mint látszik. Ez a Shrek magyar szövege volt...

### Hang 

![audio](/audio_revis.png)

Ez egy nyers hangfájl volt. (fontos hogy ha kipróbálod, nyers fájlon csináld, mert tömörítés után már csak a tömörítés struktúrája fog visszajönni képben). Nem lövöm le miért így néz ki, érdemes elgondolkodni azon, hogy a hang miként fordul elő a világban.

### Exec

![exec1](/astar_revis.png)
#### 
![exec2](/exec_revis.png)

A futtatható állományok érdekesen néznek ki. A kép alapján elég háttértudással még azt is meg lehet mondani milyen architektúrának a majdnem gépi kódját látjuk. Ez a kép például a nemrég említett nagy házimnak a futtatható állománya, ami az Intel x86-os architektúrára készült. Oké van benne szöveg, de mik azok a függőleges és vízszintes vonalak?

```asm
	mov	rdi, rbx	#, tmp333
	call	mapItNodes@PLT	#
	lea	rdi, 128[rsp]	# tmp336,
	call	GetMapI@PLT	#
	mov	QWORD PTR 96[rsp], 0	# openSet.first,
	mov	QWORD PTR 104[rsp], 0	# openSet.last,
	mov	QWORD PTR 112[rsp], 0	# closedSet.first,
	mov	QWORD PTR 120[rsp], 0	# closedSet.last,
	mov	QWORD PTR 32[rsp], 0	# StartNode,
	mov	QWORD PTR 40[rsp], 0	# EndNode,
```

Ebben rejlik a megoldás. Ez a nagy házim kódjának egy részlete, eggyel alacsonyabb szinten, ez már Intel x86-os assembly kód. Észre lehet venni, hogy a MOV operátor elég sokszor előfordul, és általában más-más regisztereket használva. A MOV és a regiszterek címe is megfeleltethető egy-egy bájtnak, ekkor viszont pont azokat az egyenes vonalakat kapjuk, mint amit a képen is látunk, hiszen egymást követik az utasításokban a bájtok.


### Képek

![img1](/img_revis.png)
#### 
![img2](/img_raw_revis.png)

Talán ezek a leglátványosabbak. Itt nyers képeket használtam. A képeknek az a tulajdonsága, hogy az egymás mellett lévő pixelek nem sokban térnek el egymástól. Gondolj például egy napnyugtára ahol nincsenek éles váltások sokkal inkább progresszíven megy át egy szín egy másikba. Itt is ezt a tulajdonságot lehet felismerni.

![img3](/img_stego_revis.png)

Ezt érdemes még szemügyre venni, mivel ez is egy képből jött, bár látszik hogy nem nyers hanem tömörített. Egy .png fájlból keletkezett ez, de valami nem stimmel, valami gyanús. Látszik benne a szövegeknek a mintája. Ez azért van mert ez egy olyan .png kép volt amibe elrejtettem egy kis szöveget. Egy ilyen rejtett dolgot, ilyen könnyen le lehetett leplezni ezzel a programmal.


## PNG inject

### 2024 január

[Github repo](https://github.com/valkosch/png_inject.git)

Ez sem valami hatalmas projekt volt, de ha már megemlítettem az előbb, hogy szöveget rejtettem el a képben akkor már bemutatom ezt is. Ekkor tájt kezdett el érdekelni a stegonográfia, ami magyarul annyit tesz, hogy valamit nem csak titkosítunk, hanem magában azt a tényt is elrejtjük, hogy titkosítva van valami. Ez a kis program is csupán arra képes, hogy a png szabványát kihasználva, egy olyan chunk-ot szúr be a png-be, amit az olvasó programok ignorálni fognak, tehát a kép ugyanúgy megjelenik, de közben ugyanebbe a chunk-ba, tetszőleges payload-ot helyez el.

## Cipher

### 2024 február-május

[Github repo](https://github.com/valkosch/cipher.git)

Programozás 2 tantárgyból is kellett projekt munkát készíteni, a C++ újdonságait és az objektumorientált programozás paradigmáit használva. Ez egy framework, amiben implementálva lett pár STL tároló, mint például a String, List vagy a Vector, illetve pár klasszikus szimmetrikus titkosítási metódus mint az XOR, Bifid, Vigenere, mivel ez volt a választott feladatom alap koncepiója. Ezen túl kihívás gyanánt én megcsináltam az SHA256 hash függvényt, amely segítségével 256 bites hash-eket lehet előállítani tetszőleges szövegből. Ugyanazt a hash digest-et adja vissza, mint bármely más hivatalos SHA256 eszköz. Ezt azért csináltam mert így például ezt felhasználva, lehet egy fiókrendszert csinálni, ahol a fiókokhoz tartozó jelszavak és felhasználónevek egy ilyen hash-ben vannak eltárolva, hiszen **plaintext-ben jelszót soha nem tárolunk**. Ennél a feladatnál már sokkal komolyabban vettem a dokumentálás fontosságát, ami a kommentek gyakoriságában is látszik, illetve Doxygent is használtam.

Ha esetleg érdekel a dokumentáció, azt [itt](https://github.com/valkosch/cipher/blob/main/dokumentacio_nhf.pdf) megtalálhatod.

## liftech.hu 

### 2024 március-július

[Github repo](https://github.com/valkosch/liftech.git)

![liftech](/liftech.png)

Második félév közben elvállaltam egy weboldal készítését, amelyhez próbáltam a legújabb technológiákat használni, hogy minél minőségibb végeredmény szülessen. Ezért használtam a Next.js framework-ot az app router-rel. Az oldal célja az a reklámozás, kapcsolattartás és bemutatás volt. Mivel én csináltam mindent, ezért nem csak frontend, hanem backend szinten is tudtam fejlődni a munka során. Az oldalnak kellett egy Node.js futásikörnyezet a szerveren, ezentúl tűzfalat beállítani, Nginx proxyt konfigurálni, illetve a weboldal űrlapos részének egy API-t írni. Összeségében szerintem sokat tudtam tanulni a készítése során, miközben a megrendelőm elvárásainak is megfeleltem és még egyetemről sem buktam ki :).

A kliensem visszajelzése referencia gyanánt:
> "Cégünk, a Liftech Group Kft weboldal készítéséhez keresett segítséget, és így találtunk rá Kurcz Valentinra, akinek a személyében egy fantasztikus fiatalembert ismertünk meg több hónapos együttműködésünk során. Első pillanattól teljes odaadással állt neki a munkának. Áttanulmányozta a cég felépítését, működését, utána maximális szakmai tudással illetve alapossággal, kreativitással készítette el az elvállalt munkát. Mindig elérhető volt, minden felmerülő kérésünket nagyon gyorsan teljesítette. A végeredménnyel maximálisan meg vagyunk elégedve, és reményeink szerint a jövőben is szeretnénk fenntartani a szakmai együttműködést Valentinnal." - Lábas Attila

Az oldal: [liftech.hu](https://www.liftech.hu)



