+++
title= "Kriptográfia alapok - Nyílvános kulcsú titkosítás"
date= 2024-07-30T14:42:35+02:00
draft= false
[params]
    math = true
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
1. Bob letitkosítja az üzenetét Alice publikus kulcsával, majd elküldi Alicenak az eredményt.
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

Bob és Alice az RSA-t akarják használni tikosításra beszélgetés közben, mert biztonságos. Ekkor ha Bob üzenetet szeretne küldeni Alicenak, akkor elkéri Alice publikus kulcsát \((n,e)\), majd ezt egy csatornán (nem muszáj biztonságosnak lennie) meg is kapja. Alice a titkos kulcsát \((d)\) soha nem adja ki, azt a dekódolásra használja saját magánál.

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

A dekódolás folyamatában, az 1 ismételt négyzetre emelést érdemes lecserélni 2 ismételt négyzetre emelésre, és ez ekvivalens is lesz a kínai maradéktétel miatt, ugyanakkor kisebb hatványkitevővel dolgozik ez a 2 ismételt négyzetre emelés ami a gyakorlatban gyorsabb mint az 1 nagy hatványkitevővel. A részletekbe most nem megyek bele.

Most pedig térjünk rá a biztonságra, ami egy titkosításnál sarkallatos pont úgy hiszem. Már eset róla szó hogy az egész RSA biztonsága az a faktorizációs problémára épül. Ahogy már említettem kulcs hossznál már a 2048 bit ajánlott, bár még nem ismert olyan eset, hogy 1024 bites kulcsot praktikus időn belül feltörtek volna. 512 bites kulcsokat még nem de már ennél rövidebb kulcsokat talán már 20 évvel ezelőtt is feltörtek gépparkokkal, ma már egy asztali számítógép is megtudja csinálni reális időn belül. Az aktuális viszont a kérdés, hogy a kvantumszámítógépek megjelenése mennyire zavarja meg ezt a magabiztosságot. Rosszul fogalmaztam mert ez már 1994 óta nem is kérdés, mivel Peter Shor bemutatta, hogy ha valaha keletkezik kvantumszámítógép, akkor azon polinomiális időn belül törhető lesz az RSA. Emiatt nemzet szintű szervezetek és játékosok, illetve most már applikációk is (pl. Signal, Messenger) kezdenek átállni kvantum rezisztens titkosításokra. 

Nem feltétlenül kell viszont magaslatokban gondolkodni ha RSA sérülékenységről van szó. Elég csak egy rossz konfiguráció. Például ha \(e=3\) azaz túl kicsi, akkor könnyen meglehet hogy \(x^e < n\) és ilyenkor csak elég \(e\)-dik gyökét venni a titkosított üzenetnek, és mivel nem jászott a modulus, ezért megkapjuk gond nélkül az üzenetet. Ez amatőr dolog, gyakorlatban nem megszokott de például CTF feladatban előfordulhat. Kulcs generálásnál is már említettem, hogy rendkívül fontos az eléggé random véletlenszám generátor, ami nem kis feladat. Hallottam, hogy például valamilyen nagy cég lávalámpa mátrixot használt, ahol a lávalámpákban mozgó kis buborékok mozgását figyelték és ezek alapján generáltak véletlenszámot.

A csupasz RSA determisztikus, tehát ugyanaz a kulcs ugyanazt az üzenetet, ugyanazt a titkosított szöveget köpi ki. Ebből az következik, hogy a támadóknak lehetősgük van idő-tárhely "trade-off"-ra. Ez hasonlít a hash függvények elleni támadásra, ahol úgynevezett "rainbow table"-lel előre lehashelnek rengeteg előfordulható szöveget (főképp jelszavakat) majd eltárolják az eredményeket. Itt is ez a helyzet, a nyílvános publikus kulcssal letitkosítunk temérdek variációt és eltároljuk őket, majd ha valamit vissza akarunk fejteni, csak megkeressük a hatalmas adatbázisunkban, hogy van-e egyezés. Félelemre semmi ok, pontosan ugyanúgy lehet védekezni ez ellen is mint a rainbow table-ök ellen is. Ott a salting azaz sózásnak hívták, itt padding-nek nevezik. Padding-nél az üzenetet bizonyos protokoll szerint kiegészítjük egy véletlenszerű toldással, majd ezt feloldásnál visszafele eljátszuk.

Ami engem izgat és biztosan meg fogom csinálni a gyakorlatban, azok a side-channel támadások. Ezek arra alapoznak, hogy a támadónak hozzáférése van plusz információkhoz a titkosítás során, például hogy milyen hardveren történik, mennyi idő alatt, mekkora teljesítményt igényel, mennyi hőt vagy elektromágneses hullámokat bocsájt ki a processzor a titkosítás alatt. Olyan titkosítás nem létezik amely side-channel támadással ne lehetne feltörni, mivel az implementációt nem lehet tökéletesre megcsinálni. Viszont ehhez fizikai hozzáférésre lenne szükségünk a számítógéphez amely szerencsére nagyban csökkenti a támadók esélyeit.

### Diffie-Hellman

Az RSA képes hatalmas üzenetek blokkonkénti titkosítására a tárgyalt asszimetrikus algoritmussal, de az igazság az, hogy bármennyire is próbálkozunk optimalizálni, egy szimmetrikus titkosítás mindig gyorsabb lesz nála. A szimmetikus titkosításnak viszont szüksége van egy közös titkos kulcsra amely titkosan lett megosztva a felek közöt, ezt a problémát már a cikk elegén túltárgyaltam. Tehát az asszimetrikus és szimmetrikus titkosításnak is megvannak előnyei és hátrányai. Mit csináltak a zseniális mérnökök? Mindkettőt használják egyszerre megtartva a jó tulajdonságokat a rosszaktól pedig megszabadulva. Ezért az internet vadvilágában az a szabvány, hogy A és B eszköz asszimetrikus titkosítással közös titkos kulcson megegyeznek, majd ezután egy megegyezett szimmetrikus titkosításra átváltanak. Lenyűgöző, nem?

Itt jön képbe a Diffie-Hellman-kulcscsere, ami időben meg is előzte az RSA-t, az 1976-os megjelenésével. Az elismerés érte Ralph Merkle-t, Whitfield Diffie-t és Martin Hellmant-t érdemli. Az algoritmus célja, hogy nyílvános kulcsú rejtjelezéssel, felek között megteremtsen egy közös titkot, amelyhez harmdik fél hozzáférni nem tudhatott. Ezt az RSA-hoz hasonlóan egy olyan függvénnyel teszi ami egyirányú.

![diffie](/diffie.png)

Wikipédián találtam ezt a remek kis egyszerű ábrát amit követve könnyen meg lehet érteni a fő gondolatot.

A folyamat az alábbi:

Alice és Bob megegyeznek publikusan \(p,g \in N\) számokról (ábrával ellentétben, nagyságrendben nagyobb számokat használva).

Alice és Bob is külön-külön felhasználják titkos kulcsaikat.
Alicenál, az \(a, \in N\) titkos kulcssal:

\[A=g^a \pmod p\]

Bobnál, a \(b, \in N\) titkos kulcssal:

\[B=g^b \pmod p\]

Majd \(A\)-t Alice elküldi publikusan Bobnak, Bob pedig \(B\)-t Alicenak.

Ezután mindkettőjük a kapott értéken megint felhasználják titkos kulcsukat. A keletkező \(s\) szám lesz a közös titok.

Az algoritmus fő ötlete, hogy kihasználja az alábbi összefüggést

\[g^{ab} \equiv g^{ba} \pmod p\]

Ez teszi lehetővé, hogy Alice és Bob is ugyanazt az értéket kapja meg. Mi állítja meg a támadókat a privát kulcsok visszafejtésére vagy akár a közös titok kiderítésétől?

Itt is egy olyan matematikai függvényen alapszik a biztonság, amelyről azt sejtjük, hogy nem lehet polinomiális időn belül (a kvantumszámítógépeket nem beleértve ebbe megint) elvégezni. Ez a függvény a diszkrét logaritmus. Egyszerűen arról van szó, hogy \(g^a,g^b,p,g\) ismeretével nem tudjuk visszafejteni \(a,b,g^{ab}\) értékeket még mielőtt a Nap kihűlne.

### Gyakorlat 

A fentiekben is már bőven szó esett arról, hogy mire is lehet használni az asszimetrikus kriptográfiát, de talán most jobban kifejtem.

#### TLS 

A TLS (Transport Layer Security), egy olyan keretrendszer, szabvány, amely azon protokollok biztonságáért felelős mint például a HTTPS, amelyek A címtől juttatnak el B címre adatokat. Tehát lényegében azt testesíti meg, ami eddig a cél volt, hogy az internet kommunikáció titkosítva legyen. A TLS jóformán az amit, aDiffie-Hellman-kulcscserénél már elmondtam, hogy asszimetikus titkosítással megegyezünk egy közös titkos majd átállunk szimmetikusra ezzel a titokkal, mivel az gyorsabb. Azonban az olyan részleteket kihagytam, mint hogy azon is ilyenkor egyeznek meg milyen titkosítást használjanak, amelyre mindkettő eszköz képes.

Fontos feladata a TLS-nek az autentikáció is. Például hiába a sok erőfeszítés ha kivetelezhető egy MITM (Man-In-The-Middle) támadás. Az MITM során a támadó harmadik félként beáll Alice és Bob kommunikációja közé, úgy hogy ezt sem Alice nem veszi észre sem Bob. Ez úgy lehetséges, hogy a támadó, mondjuk József, azt állítja Alicenak, hogy ő Bob, Bobnak pedig azt mondja hogy ő Alice. Így Alice és Bob is József publikus kulcsával fofja titkosítani a kommunikációját, ezért József bármikor beleolvashat abba. Fontos, hogy ahhoz hogy az illúzió müködjön, József a kommunikációt továbbítja Alice és Bob között, így a két szereplőnek tényleg fogalma sem lehet hogy valójában valakin keresztül megy az üzenetük.

![mitm](/mitm.png)

Erre a megoldások voltak a tanusítványok. Tanúsítványokat megbízott harmadik felek állítják elő, és egy tanusítványban szerepel, hogy az adott félhez milyen publikus kulcs tartozik. Ezek az SSL tanusítványok, melyeket te is részletesen el tudsz olvasni bármelyik weboldalnál ha a böngésződben a lakatra kattintasz. Itt van például az enyém:

![cert](/cert.png)

#### Challange-response

Minden eszköz most már rendelkezésünkre áll ahhoz, hogy megoldjuk a korrupt recepciós problémát. A cél az, hogy a recepcióssal ne kelljen megosztanunk semmi titkosat, de ugyanakkor a recepciós 100%-ban biztos lehessen, hogy azok vagyunk akiknek állítjuk magunkat. Mi sem egyszerűbb: a recepciós a tanusítvány szerint Kurz Valentinhez tartozó publikus kulcssal letitkosít valami számára egyedi dolgot, mondjuk egy véletlenszámot, és elküldi azt nekem - ez a challange. Majd én ezt feloldom a titkos kulcsommal, és visszaküldöm neki az eredményt - ez a response. A jó eredményt valóban csak is én tudom megmondani (már ha tényleg én vagyok Kurcz Valentin :D), hiszen csak az én titkos kulcsommal lehet feloldani a hozzám tartozó publikus kulcssal titkosított üzeneteket. A korrupt recepciós pedig a kardjába dőlhet mert nem sikerült kicsalnia belőlünk a titkunkat.

Ezt a challange-response rendszert használja a legtöbb SSH kliens is, amikor távolról szeretnénk elérni egy shell-t. A szerver autentikálja a klienst, hogy meggyőződjön jogosultságairól.

#### Digitális aláírás

Autentikációra van más módszer és erre példa a digitális aláírás. Hasznossága kétségbevonhatatlan, hazánkban is már az Ügyfélkapun keresztül alá tudunk írni hivatalos okmányokat, és nemhogy legalább annyira hiteles mint a tinta és papír, de megfelelő konfiguráció mellett, meghamisíthatlan nem úgy mint az írásunk.

Az RSA című elbeszélő költeményemben már szó esett arról, hogy a titkos és a publikus kulcs funkcionalitása felcserélhető, nem úgy mint például a Diffie-Hellman esetében.
