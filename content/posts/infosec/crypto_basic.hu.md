+++
title= "Kriptográfia alapok - Nyílvános kulcsú titkosítás"
date= 2024-07-30T14:42:35+02:00
draft= false
math=true
toc=true
summary = "A cikk végére példákon keresztül világossá válik számodra, hogy miként tudunk magabiztosan titkos információkat megosztani egymással, főképp az interneten, anélkül, hogy megosztottunk volna egymással, bármiféle titkos és privát kódot, kulcsot vagy jelszót. Ezen túl még sokkal több..."
description = "Meg kell tudnunk valósítani azt, hogy egy hatalmas teremben elkiálthassuk magunkat úgy, hogy csak az az egyetlen ember érthesse, akinek szól az üzenetünk, és ezt mind úgy, hogy ezzel az emberrel nem beszéltük meg előre a titkos kódunkat, nyelvünket egy már titkos médiumon keresztül. Ha ezt meg tudjuk oldani akkor az interneten is tudunk titkosan üzenetet megosztani hiszen istenigazából az internet is csak egy hatalmas terem."
tags = ['Kriptográfia', 'RSA']
+++

## Szimmetrikus kriptográfia

A klasszikus titkosítási módszerek, amelyeket használtak egészen mostanáig arra alapoznak, hogy a titkosításhoz (encryption) és a feloldáshoz (decryption) is ugyanaz a kulcs használatos. Ez azt igényli, hogy a kulcsot valami titkos közegben kellene átadnunk a második félnek anélkül, hogy egy harmadik fél ne szerezzen róla tudomást, hiszen akkor az összes titkosított üzenetünket olvasni tudná. Ez kicsit ilyen "tyúk vagy a tojás" probléma, hiszen pont arra akarnánk használni a kulcsot, hogy megteremtsük ezt a titkosított közeget. Illetve ekkor, teljesen meg kell bíznunk a második félben, hiszen miután megosztottuk vele a kulcsunkat, az teljesen az ő döntése lesz, hogy azt nem e adja tovább másnak.

Tegyük fel, hogy a bankba besétálunk és a fiókunkhoz elkéri a recepciós a jelszót. Ha egy kicsit is korumpálható a recepciós, akkor a fiókunk jelszava könnyen nem kívánatos emberek kezébe kerülhet. Ehhez a problémához még visszatérek, addig is gondolkozz egy megoldáson.

> Az Enigma is szimmetrikus kriptográfia szerint működött és mint titkosítás a korában egyedülálló és példátlan volt. Komoly szürkeállománynak kellett összeülnie a Szövetségesek oldalán, hogy feltörjék de végül Alan Turing és csapatának illetve automatájának (kezdetleges számítógép) köszönhetően sikerült. Szó sincs arról, hogy a titkosításban találtak hibát. Mint mindig az informatikában, a gyenge láncszem és a legnagyobb sérülékenység itt is az ember volt. A katonák nem minden nap cseréltek kódot, ahogy azt kellett volna, illetve az üzenetek végén ismétlődő kifejezések mint például a "Heil Hitler" egyszerűsítették a kódfejtő dolgát. Érdekesség, hogy 1982-ben is használták még az Enigmát még pedig az argentínok, Fakland-szigeteki háborúban. Nagyot néztek mikor az angolok minden lépésüket tudták előre. Nem lehet hibáztatni az argentínokat, hiszen az Egyesült Királyság sose hozta nyílvánosságra az eredményeiket az Enigma feltörésében, így még mindig az volt a köztudat, hogy az Enigma titkos.

Csak azért hoztam föl az Enigmát, mert egy jó példa arra, hogy a szimmetrikus titkosítás önmagában miért nem elég. Gondolkoztál már azon, hogy az Enigmát miért csak tengeralatjárókon használták? Azért mert a tengeralatjáró ha megsemmisül, akkor az Enigmát is viszi magával a tengerfenékre, így nem lehet a gépet, azaz a titkosítást visszafejteni illetve a kódot ellopni. Ezért nem használták szárazföldi csapatoknál. Tehát nem volt skálázható, hiszen több ember esetén egyre nagyobb lett volna az esélye a kód kitudodásának. Az internet térnyerésével, és a felhasználók exponenciális növekedésével pedig a szükség egy skálázható titkosítási szabványra nőttön-nőtt.

## Asszimetrikus kriptográfia

Az asszimetrikus kriptográfiában, avagy nyílvános kulcsú titkosításnál kulcs párokról beszélünk, ahol van egy **privát kulcs** és egy **publikus kulcs**. Restrospektíven, úgy a székben hátradőlve nézve a koncepció egyszerűsítve az alábbi: páronként a titkosításhoz szükséges kulcs publikus, a feloldáshoz a kulcs pedig titkos, és a publikus kulcsból visszafejteni a titkos kulcsot algoritmusokkal annyi időbe telne, hogy nem csak a kávénk hűlne ki, de még a Naprendszer is. Az asszimetrikus jelző innen jön, hogy a titkosítás és dekódolás nem ugyanúgy megy.

Egy információ csere asszimetrikus titkosítást használva Bob és Alice között az alábbiak szerint zajlana le az ábrát követve:
1. Bob letitkosítja az üzenetét Alice publikus kulcsával, majd elküldi Alice-nak az eredményt.
2. Alice megkapja a titkosított üzenetet, majd a titkos kulcsával feloldja azt.

![pkc](/pkc1.png)

Az egész rendszer müködése azon alapszik, hogy a titkos kulcsok titkosak maradnak és senkivel nem osztjuk meg azt, mivel annak beláthatatlan következményei lehetnek ránk nézve.

Most pedig konkrét megvalósítások.

### RSA (Rivest–Shamir–Adleman)

Az RSA az egy kriptográfiában nevezetes algortimus amely nyílvános kulcsú titkosítást valósít meg, melyet 1977-ben vázolt a nevében is szereplő 3 dzsentlmen. A cél az volt, hogy hatékony algortimus, azaz polinomiális, szülessen asszimetrikus titkosításra.

#### Definíciók

\[n,e,p,q,d \in N, (e,\phi(n))=1: n=p * q\] 

Az alap ötlet az RSA mögött, hogy adott \(n, e\) és \(d\) egész számnál, az \(x \in N: 0 \leq x < n\) szám esetén az \((x^e)^d\) és \(x\) azonos maradékot adnak \(n\)-nel osztva, azaz kongruensek moduló n:

\[(x^e)^d \equiv x \pmod n\]

Ugyanakkor ha csak az \(n\) és \(e\) szám adott, a \(d\) szám kiszámítása borzasztóan nehéz. Olyannyira nehéz elég nagy n szám esetén, hogyha megpróbálkoznánk vele egy nem kvantumszámítógéppel, akkor nem csak a kávén hűlne ki hanem az univerzum is. Hogy miért lehetséges ez azt rövidesen tisztázom.

Az \(n\) és \(e\) természetes számok alkotják a publikus kulcsot, melyben az \(n\) a moduló amely \(p\) és \(q\) prímszám szorzatából áll, és az \(e\) pedig a titkosításhoz használt ("e" for encryption) hatványkitevő mely relatív prím \(\phi(n)\) számmal.
> a \(\phi(n)\) függvény az az Euler-féle phí függvény mely megadja, hogy \(x \in N: 1 \leq x \leq n\) számok között mennyi n-hez relatív prím van.

A \(p\) és \(q\) alkotják a titkos kulcsot, melyekbők kiszámítható a \(d\) ("d" for decryption) hatványkitevő, ami a dekódoláshoz fog kelleni. Igazából a \(p\) és \(q\) eldobható miután \(d\) ki lett számítva.

A titkosítás folyamata: 

#### Kulcs generálás

1. Generáljunk 2 nagy \(p\) és \(q\) prímszámot. A lényeg az lesz, hogy az \(n\) számból minél nehezebb legyen prím faktorizációval megkapni a \(p\) és \(q\) számot. Ehhez a \(p\) és \(q\) is legyen lehetőleg teljesen véletlenszerű, kellően nagy és kettőjük különbsége is legyen nagy. Hatékony prím generálásra a prím tesztelése véletlenszámokra. Fontos hogy a véletlenszám generátorunk legyen minél "véletlenebb", azaz entrópiája minél nagyobb, hiszen számítógépen csak pszeudo véletlent ismerünk hiszen a számítógép önmagában determisztikus. Ezzel egy külön ágazat foglalkozik matematikában, csak érdekességnek hoztam föl. 

2. Számoljuk ki \(n=p * q\). Biztonságosnak ítélt kulcs azaz \(n\) hossz az 2048 bit, ami kb 617 számjegynek felel meg a tízes számrendszerben azaz értéke saccperkb \(10^616\). Most álljunk meg és gondolkozzunk el ezen nagyságon egy picit. Ugyebár úgy tudnánk támadóként feltörni a titkosítást, ha \(n\)-ből meg tudnánk kapni \(p\)-t és \(q\)-t. Ez kicsit túl lő az üzenet feltörésén, hiszen ezután az összes üzenetet fel tudnánk törni. Ez a prím faktorizáció problémája melyre mai napig nem ismert polinomiális komplexitású algoritmus, ugyanakkor nincsen az se bizonyítva, hogy nem létezik ilyen, bár ez a sejtés. Ha esetleg találnál ilyet, akkor könnyűszerrel világuralomra törhetsz, hiszen a banki tranzakciók, kriptovaluták, államtitkok és nem utolsósoron a digitális aláírások titkosítása is ezt használja. Például a szofterfrissítések is digitális aláírással ellenőrzik, hogy a hivatalos szerverről jött a frissités, de ha feltörnéd ezt, bárkinek kiadhatnád magad, ezáltal például az összes Windows gép felett átvehetnéd az irányítást egyetlen Windows frissítéssel. Érdekes, hogy az egész világrendünk, a civilizációnk egy matematikai sejtésre van bízva. Csak hogy elképzelhesd mennyi időbe is telne ha naívan elkezdenénk próbálgatni brute-force módszerrel a prímeket 2-től kezdve \(\sqrt n\)-ig: kb. \(10^{80}\) proton van a megfigyelhető világegyetemen, tegyük fel hogy ez mind egy olyan számításiegység ami 4 GHz-en müködik azaz az egyszerűség kedvéért másodpercenként \(4 * 10^9\) prím tesztelésére képes, tehát másodpercenként \(4 * 10^{89}\) szám tesztelésére vagyunk képesek összesen. Ezek szerint \(\frac{\sqrt{10^{616}}}{4 * 10^{89}} \approx 10^{218} \) másodpercre lenne szükségünk. Az univerzum 13,7 milliárd éves azaz kb \(4,3 * 10^17\) másodperc éves. Remélem ez a kis gondolatkísérlet kontextusba helyezte a nagyságokat.

3. Válasszunk ki egy természetes számot az \(e\) számára úgy, hogy \((e,\phi(n))=1\), azaz relatív prímek. Itt a hossz nem annyira lényeges mint az \(n\) esetén, tipikus érték az \(e\)-nek a \(65537\).

4. Számoljuk ki a \(d\) értékét a \(p\) és \(q\) számok segítségével. Hogy hogyan azt a helyesség bizonyításánál méltatom bemutatni.

Tehát ezek után van egy publikus kódoló függvényünk 

\[C: x \mapsto x^e \pmod n\]

ahol \(x:=\) a plaintext üzenetünk azaz a titkosítandó adat

és van egy titkos dekódoló függvényünk 

\[D: y \mapsto y^d \pmod n\]

ahol teljesül az hogy \(D(C(x))=x\)

Aki eddig eljutott és ismeri a hatványazonosságokat az biztos elgondolkozott rajta, hogy a \(d\) és \(e\) felcserélhető-e. Bizony felcserélhetőek hiszen \((x^e)^d = (x^d)^e\). Tehát a \(d\) is használható titkosításra és az \(e\) pedig feloldásra, már ha a művelet inverze is a kulcs pár másik felével történt. Ezt ki is használjuk a gyakorlatban a digitális aláírásnál és azonosításnál, de erről többet kicsit lejebb :).

#### Kulcs megosztása

Bob és Alice az RSA-t akarják használni tikosításra beszélgetés közben, mert biztonságos. Ekkor ha Bob üzenetet szeretne küldeni Alice-nek, akkor elkéri Alice publikus kulcsát \((n,e)\), majd ezt egy csatornán (nem muszáj biztonságosnak lennie) meg is kapja. Alice a titkos kulcsát \((d)\) soha nem adja ki, azt a dekódolásra használja saját magánál.

#### Titkosítás

Bob miután megkapta Alice publikus kulcsát, azt felhasználva titkosítja az \(x\) üzenetet és megkapja titkosított szöveget \(c\)-t.

\[c \equiv x^e \pmod n\]

Minden józan, már tapasztaltabb emberben, fel kell merülnie annak az ördögi kérdésnek, hogy ez a lépés vajon polinomiális-e mert ha nem akkor semmi értelme nincsen az egésznek. Szerencsére a moduláris hatványozás megoldható polinomiálisan az ismételt négyzetre emelés módszerével, annak ellenére, hogy a hatványozás önmagában nem lenne polinomiálisan kivitelezhető.

#### Feloldás

Alice ezután megkapja a titkosított üzenetet és a titkos kulcs segítségével feloldja azt.

\[c^d \equiv (x^e)^d \equiv x \pmod n\]

#### Helyesség bizonyítása

Eddig valjuk be őszintén a levegőbe beszéltem, semmi se lett bizonytva, már pedig ez így nem járja.

Cél hogy bebizonyítsuk, hogy \[\forall n \in N, n=p * q, \forall e \in N, (e,\phi(n))=1\] esetén \(\exists d\) ahol igaz az, hogy \((x^e)^d \equiv x \pmod n\) ahol \(x\) tetszőleges üzenet.

Ehhez felhasználjuk az Euler-Fermat tételt miszerint 

ha \(n \leq 1\) és \((x,n)=1\) \(\Rightarrow x^{\phi(n)} \equiv 1 \pmod n\)

Emeljük a konkruenciát \(k \in N\)-ra és szorozzuk mindkét oldalát \(x\)-szel. Ezek ekvivalens lépések, ekkor

\[x^{k\phi(n) + 1} \equiv x \pmod n\]

tehát ha a hatványkitevő \(ed = k\phi(n) + 1\) akkor igaz a konkruencia.

Ebből a megállapításból az alábbi konkruencia következik

\[ed \equiv 1 \pmod {\phi(n)}\]

hiszen ez az előbbi állítással ekvivalens.

Ez pedig egy lineáris konkruencia a \(d\) ismeretlenre, hiszen \(e\) értéke adott és \(\phi(n) = (p-1)(q-1)\) itt nem tárgyalt azonosságok alapján. Ennek a lin. kon. biztosan \(\exists\) megoldása, mivel \((e,\phi(n))=1\). Ezt a megoldást hatékonyan meg is tudjuk találni az Euklideszi algortimussal.

Ez nem a teljes bizonyítás hiszen nem fedtük le azt az esetet amikor is \((x,n) \neq 1\). Ennek a gyakorlati valószínüsége ámbár nagyon kevés hiszen ez azt jelenti, hogy \(p | x\) vagy \(q | x\), de mégis kell vele foglalkozni, hiszen matematikusok vagyunk :D. 

Ha \(p | x\) és \(q | x\) is akkor \(pq | x\) azaz \(n | x\) ekkor \(x^{k\phi(n) + 1} \equiv x \pmod n\) természetes hogy létezik megoldás hiszen \(x \equiv 0 \pmod n\) és \(x^{k\phi(n) + 1} \equiv 0 \pmod n\) hiszen ha \(n | x\) akkor \(x\) többszörösei is oszthatóak \(n\)-nel.

Ha \(p | x\) és \( q \nmid x\) akkor külön-külön bemutatjuk 

1. \[x^{k\phi(n) + 1} \equiv x \pmod q\]

2. \[x^{k\phi(n) + 1} \equiv x \pmod p\]

kongruenciát is, hiszen ekvivalens ez \(x^{k\phi(n) + 1} \equiv x \pmod n\)-nel

Az 1. állítás bizonyítása 1:1 megegyezik azzal az esettel mikor \((n,x)=1\) hiszen most \((q,x)=1\).
A 2. állítás pedig szinte ugyanaz mint amikor \(x \equiv 0 \pmod n\). 

Most már teljes a bizonyítás!

#### Megjegyzések a gyakorlati alkalmazáshoz

Sajnos (vagy nem sajnos) a matekot nem lehet elkerülni ha az ember komolyabban akar foglalkozni az informatikával, és ez a gyakorlati implementációnál is igaz.


kulcs megosztás aztan szimmetrikus mert az gyorsabb
NSA gyüjti a titkosított adatot hátha fel tudja később törni
ssh - challange and response
tls

