
---

## 🚀 Bramework v1.0 – Fejleszd az ötleteidet, ne a vázat!

**Eleged van a túlbonyolított, nehézkes keretrendszerekből?** A **Bramework** azoknak készült, akik értékelik a sebességet, a tisztaságot és a teljes kontrollt. Ez nem egy újabb "mindent is tudó" monstrum, hanem egy precíziós eszköz a kezedben.

* **Azonnali Start:** Beépített regisztrációs és bejelentkezési rendszerrel érkezik – csak klónozd és kész!
* **Pehelysúlyú Architektúra:** Nincs felesleges sallang, csak az, amire valóban szükséged van.
* **Biztonság Alapból:** Singleton PDO kapcsolat és modern jelszótitkosítás védi az adataidat.
* **Hordozhatóság:** Egyetlen konfigurációs fájl (`config.php`) segítségével bármilyen szerverre pillanatok alatt áttelepítheted.

**Bramework: Ahol a minimalizmus találkozik a professzionalizmussal.**

---

## 💡 Mi is az a Bramework? (Magyarázat)

A Bramework egy **egyedi PHP keretrendszer**, amely a modern webfejlesztés legjobb gyakorlatait ötvözi a végtelen egyszerűséggel. A rendszer lelke a **Front Controller** minta: ez azt jelenti, hogy minden látogató egyetlen "kapun" (az `index.php`-n) lép be, ahol a **Router** (útvonalválasztó) dönti el, hogy melyik tartalom jelenjen meg neki.



### Hogyan működik a gyakorlatban?

1.  **Tiszta URL-ek:** Felejtsd el a csúnya `index.php?page=contact` linkeket! A Bramework-kel egyszerűen csak `balthes.hu/kapcsolat` lesz az elérési út.
2.  **Komponens-alapú szemlélet:** A weboldalad elemeit (fejléc, menü, lábléc) egyszer írod meg, és bárhol újrahasználhatod őket.
3.  **Biztonságos Adatkezelés:** Az adatbázis-kapcsolat nem épül fel feleslegesen többször; a **Singleton** minta biztosítja, hogy a szerver erőforrásaival takarékosan bánjunk, a **PDO** pedig kivédi a rosszindulatú SQL támadásokat.
4.  **Központosított beállítás:** Minden, ami a környezettől függ (szerver név, jelszavak, alap URL), egyetlen külön mappában (`/config`) lakik. Ez teszi a rendszert "hordozhatóvá": fejlesztés után csak átírod a configot, és már élesítheted is a projektet.

### Kinek ajánlott?
Ideális választás **tanuláshoz**, **kisebb-közepes webalkalmazásokhoz**, vagy olyan fejlesztőknek, akik szeretnék pontosan érteni, mi történik a kódjuk "motorházteteje" alatt, anélkül, hogy több ezer sornyi külső könyvtárat kellene importálniuk.

---
