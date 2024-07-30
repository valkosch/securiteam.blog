+++
title= "Kriptográfia alapok - Nyílvános kulcsú titkosítás"
date= 2024-07-30T14:42:35+02:00
draft= false
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

Az asszimetrikus kriptográfiában, avagy nyílvános kulcsú titkosításnál kulcs párokról beszélünk, ahol van egy **privát kulcs** és egy **publikus kulcs**. 

![pkc](/pkc1.png)

enigma nem volt skálázható tengeralatjáró
ssh - challange and response
tls
digitális aláirás- pl számítógépes frissitések - világ uralom ha NP problémát megfejted- pl számítógépes frissitések - világ uralom ha NP problémát megfejted
