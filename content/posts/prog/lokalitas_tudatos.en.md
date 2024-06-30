+++
title= "Lokalitástudatos programozás"
date= 2024-06-28T13:20:06+02:00
draft= true
summary = "Egyáltalán nem mindegy, hogy egy mátrixot, oszloponként, vagy soronkét, nehogy Isten véletlenszerűen járjuk be. Ebben a cikkben körüljárjuk, hogy hogyan lehet számtógépes ismereteinket igába hajtani, a programunk előnyére."
description = "Programunk sokban tud fejlődni azzal, ha lokalitástudatosan programozunk. Lokalitástudatosan programozni annyit tesz , mint hogy nem teszünk keresztbe a számítópünknek figyelmetlenségből. Miként tudunk a számítógépünk barátja lenni azt az alábbiakban olvashatod."
tags = ['programozás', 'C', 'számítógép']
+++

Neumann architektúrája óta, egy számítógép müködése során, a futtatandó utasításokat, és ezen utasítások által hivatkozott címek tartalmát is egy memóriában tárolja. A programban használt adatok, úgynevezett adatmemóriból érhetőek el, ez pedig tárhierarchiába rendeződik.

## Tárhierarchia

A számítógépünk teljesítménye lényegében a processzor és a memória órajeléből tevődik össze, és eme két egység közül, a memória az ami a processzor mögött kullog. Ez azt jelenti, hogy hiába a magas CPU órajel, ha a memória szűk keresztmetszetként viselkedik mellette, mivel ha az éppen végrehajtandó utasításnál, nincsen kéznél a használandó adat, akkor a processzor várakozásra kényszerűl, ezáltal nem használja ki teljes potenciálját, így suboptimális lesz a kihasználtsága.

Több memória technológiát is ismerünk, mint például a magas adatsűrűségű és olcsóbb, de lasúbb DRAM, vagy az alacsony adatsűrűségű és drága, de gyors SRAM. Egyiknél sem teljesülhet viszont egyszerre az, hogy olcsó, gyors és nagy, így mindig az igényekhez igaztva, a kompromisszum kötés elkerülhetetlen. Itt jön be a tárhierarchia miszerint több szintből építjük fel a memóriát úgy, hogy az adott szintet minél többször használja a processzor, anál gyorsabb és ezáltal kisebb legyen. 

KÉP IDE BESZÚRÁS - TÁRHIERARCHIA PIRAMIS

Ezek szerint kulcsfontosságú, hogy a processzor kihasználás érdekében, a programozó teljesen a tárhierarchia keze alá dolgozzon.


