
---

# 🇭🇺 Bramework v1.0 – Fejlesztői Dokumentáció

## 1. A Rendszer Filozófiája és Architektúrája

A **Bramework** egy pehelysúlyú, egyedi fejlesztésű PHP keretrendszer, amely a **Front Controller** tervezési mintára épül.  
Minden kliensoldali kérés (HTTP request) egyetlen központi belépési ponton, a `public/index.php` fájlon halad keresztül.

Ez a megközelítés lehetővé teszi:

- a **tiszta URL-ek** (Clean URLs) használatát  
- a robusztus **útvonalválasztást** (routing)  
- a globális konfigurációk és biztonsági ellenőrzések (pl. session indítás) központosítását  

---

## 2. Mappastruktúra és Rendszerezés

A keretrendszer szigorúan elválasztja:

- az üzleti logikát  
- a megjelenítést  
- a publikus, böngészőből elérhető fájlokat  

```
/bramework
├── /docs
│   ├── documentation_HU.md     # A részletes magyar leírás
│   ├── documentation_EN.md     # A részletes angol leírás
│   └── database_setup.sql      # Az adatbázis táblák sémája (SQL export)
├── /config                     # Globális beállítások (DB, URL, Debug)
│   └── config.php              # Az egyetlen fájl, amit a telepítőnek módosítania kell
├── /app                        # Az alkalmazás megjelenítési rétege
│   ├── /components             # A rendszer beépített, alapvető építőkockái
│   │   ├── header.php          # HTML fej, meta tagek, CSS linkelés
│   │   ├── footer.php          # Lábléc, JS szkriptek betöltése
│   │   ├── nav.php             # Reszponzív navigációs sáv
│   │   ├── start.php           # Alapértelmezett kezdőoldal
│   │   ├── login.php           # Beépített bejelentkezési űrlap
│   │   └── register.php        # Beépített regisztrációs űrlap
│   └── /views                  # Projekt-specifikus aloldalak (pl. about.php)
├── /core                       # A keretrendszer működtető motorja (Core API)
│   ├── Auth.php                # Felhasználói munkamenet és jogosultságkezelés
│   ├── Database.php            # Singleton PDO adatbázis-kapcsolat
│   ├── Router.php              # URL feldolgozó és végrehajtó motor
│   └── View.php                # Sablon (template) renderelő osztály
├── /public                     # A Document Root
│   ├── /css                    # Stíluslapok
│   ├── /js                     # Kliensoldali szkriptek
│   ├── .htaccess               # Apache URL újraíró szabályok
│   └── index.php               # Központi belépési pont és útvonal-regisztráció
└── README.md                   # Gyorsindítási útmutató
```

**Fontos tervezési döntés:**  
A `start.php`, `login.php` és `register.php` fájlok a `/components` mappában találhatók, mivel a Bramework alapból felhasználókezelésre felkészített rendszer. Ezek a “mag” részét képezik, nem projekt-specifikus nézetek.

---

## 3. A Core (Mag) Osztályok Működése

### 3.1. Routing (Router.php és index.php)

A rendszer lelke az útvonalválasztó.  
A `.htaccess` minden kérést az alábbi formára irányít át:

```
index.php?url=...
```

A `Router::add()` metódussal rendelhetünk egy URL-hez egy névtelen függvényt (closure).  
A `dispatch()` felel a megfelelő kódblokk futtatásáért vagy a 404-es hiba megjelenítéséért.

**Példa az alapértelmezett kezdőoldal betöltésére:**

```php
$router->add('/', function() {
    View::render('header', ['title' => 'Üdvözöl a Bramework!'], true);
    View::render('nav', [], true);
    View::render('start', [], true); // A components mappából töltődik be
    View::render('footer', [], true);
});
```

---

### 3.2. A Megjelenítés Kezelése (View.php)

A `View::render()` statikus metódus felel a HTML fájlok beemeléséért.

Paraméterei:

- **$name (string):** a betöltendő fájl neve (pl. `start`)  
- **$data (array):** asszociatív tömb, amelynek kulcsai lokális változókká válnak az `extract()` segítségével  
- **$isComponent (bool):**  
  - `false` → `/views` mappából tölt  
  - `true` → `/components` mappából tölt  

---

### 3.3. Konfiguráció és Adatbázis (Hordozhatóság)

A rendszer hordozhatóságát a `/config/config.php` fájl biztosítja. 
Bármilyen szerverre való költözéskor **kizárólag ebben a fájlban** kell módosítani az adatokat.

**Telepítési lépések:**
1. Másold a fájlokat a szerverre.
2. Importáld a `/docs/database_setup.sql` fájlt.
3. Nyisd meg a `/config/config.php`-t és add meg:
   - Az adatbázisod nevét, felhasználóját és jelszavát.
   - A `BASE_URL` értéket (pl. `http://domain.hu/public`).
---

### 3.4. Autentikáció és Biztonság (Auth.php)

A beépített autentikációs modul kezeli a felhasználók munkamenetét.

Elérhető funkciók:

- `Auth::init()` – munkamenet indítása  
- `Auth::login()` – bejelentkeztetés  
- `Auth::logout()` – kijelentkeztetés  
- `Auth::check()` – bejelentkezési állapot ellenőrzése  

**Jelszókezelés:**

- jelszavak **soha nem nyers formában** kerülnek tárolásra  
- regisztrációkor: `password_hash()`  
- belépéskor: `password_verify()`  

---
