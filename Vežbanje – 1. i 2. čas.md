# Stringovi i datumi u JS - priprema za proveru

Web programiranje · 3. razred · Nov 18, 2026 · @Vanja

## Kako radimo

Blok od tri časa prati prezentacija „Manipulacija stringovima i datumima u JS“: prva dva časa vežbamo, a treći čas radimo probnu proveru u istom obliku kao prava provera sledeće nedelje — deo na papiru i deo u konzoli.

| Čas | Tema | Raspored (45 min) |
| --- | --- | --- |
| 1. čas | Stringovi | A. Predvidi, pa proveri · 10 min — B. Zadaci u konzoli · 25 min — C. Nađi grešku · 10 min |
| 2. čas | Datumi | A. Predvidi, pa proveri · 10 min — B. Zadaci u konzoli · 25 min — C. Nađi grešku · 10 min |
| 3. čas | Probna provera | Deo A na papiru · 15 min — Deo B u konzoli · 20 min — zajednička analiza · 10 min |

**Rad u konzoli**

1. Otvori bilo koju stranicu u pregledaču (može i prazna, `about:blank`) i pritisni **F12** → kartica **Console**.
2. **Enter** izvršava kod, a **Shift + Enter** prelazi u novi red — tako pišeš funkcije u više redova.
3. Ako konzola javi `Identifier '…' has already been declared`, promenljiva već postoji: osveži stranicu (**F5**) ili kod zadatka stavi u vitičaste zagrade `{ … }`.
4. Funkciju napiši jednom, pa je pozivaj sa svim datim primerima i uporedi rezultat sa očekivanim.

**Podsetnik**

| Stringovi | Datumi |
| --- | --- |
| `length`, `s[i]` — indeksi kreću od 0 | `new Date(g, m, d, h, min)` — mesec od 0 do 11 |
| `indexOf` (pozicija ili −1), `includes`, `startsWith`, `endsWith` | `getFullYear`, `getMonth`, `getDate`, `getDay` (0 = nedelja), `getHours`, `getMinutes`, `getTime` |
| `slice(od, do)` — „do“ se ne uzima; `slice(-n)` — poslednjih n | `setDate`, `setMonth`, `setFullYear` — menjaju original; kopija: `new Date(d)` |
| `toUpperCase`, `toLowerCase`, `trim` | razlika: `Math.round((d2 - d1) / DAN)` |
| `replace` (prvo), `replaceAll` (sva), `split`, `join` | `toLocaleDateString("sr-Latn-RS", {…})`, nula ispred: `("0" + n).slice(-2)` |
| broj + string = string; računanje stavi u zagrade | `"2026-12-24" + "T00:00"` → lokalno vreme, ne UTC |

String metode nikada ne menjaju original — rezultat čuvaj u promenljivu. Kod datuma je obrnuto: `set` metode menjaju sam objekat.

## 1. čas — Stringovi

Cilj časa: bez greške predvideti rezultat string izraza i napisati kratke funkcije sa string metodama, kao na proveri.

### A. Predvidi, pa proveri (10 min)

Prvo upiši odgovor na papir, pa tek onda proveri u konzoli. Pazi na navodnike: `"8"` i `8` nisu isto.

```javascript
const grad = "Subotica";
```

| # | Izraz | Tvoj odgovor |
| --- | --- | --- |
| 1 | `grad.length` |  |
| 2 | `grad[3]` |  |
| 3 | `grad[grad.length - 1]` |  |
| 4 | `grad.slice(0, 4)` |  |
| 5 | `grad.slice(-3)` |  |
| 6 | `grad.slice(2, 5)` |  |
| 7 | `grad.indexOf("o")` |  |
| 8 | `grad.indexOf("O")` |  |
| 9 | `grad.toUpperCase().includes("TICA")` |  |
| 10 | `grad.startsWith("sub")` |  |
| 11 | `"7" + 3` |  |
| 12 | `"Zbir: " + 4 + 6` |  |
| 13 | `"Zbir: " + (4 + 6)` |  |
| 14 | `Number("7") + 3` |  |
| 15 | `"  JS  ".trim().length` |  |
| 16 | `"a-b-c".split("-")` |  |
| 17 | `"ana voli ananas".replace("ana", "ANA")` |  |
| 18 | `"ana voli ananas".replaceAll("ana", "X")` |  |

### B. Zadaci u konzoli (25 min)

Zadaci idu od lakših ka težim. Osnovne i srednje rade svi; bonus ko stigne.

**B1. Pozdrav** · osnovni

Napravi poruku na dva načina: operatorom `+` i template literalom. Očekivano: `"Zdravo, Jovana! Ti si u 3. razredu."`

```javascript
const ime = "Jovana";
const razred = 3;
```

**B2. Provera lozinke** · osnovni

Napiši funkciju `proveriLozinku(lozinka)` koja vraća:

- `"Lozinka je prekratka."` ako ima manje od 8 znakova;
- `"Lozinka ne sme da sadrži reč „lozinka“."` ako sadrži reč lozinka, bez obzira na velika i mala slova;
- `"Lozinka je prihvatljiva."` u ostalim slučajevima.

Testiraj sa: `"kula2026"`, `"kula"`, `"MojaLozinka1"`.

**B3. Ime fajla** · osnovni

Za `const fajl = "Slika_Leto.JPG";` ispiši: (a) da li je JPG slika, bez obzira na velika i mala slova → `true`; (b) ime bez ekstenzije → `"Slika_Leto"`; (c) samo ekstenziju → `"JPG"`. Pomoć: `indexOf(".")` i `slice`.

**B4. E-mail adresa** · srednji

Za `const email = "  Petar.Petrovic@Skola.RS  ";` ispiši: (a) očišćenu adresu malim slovima → `"petar.petrovic@skola.rs"`; (b) korisničko ime, deo pre `@` → `"petar.petrovic"`; (c) domen, deo posle `@` → `"skola.rs"`.

**B5. Red iz CSV fajla** · srednji

Podeli `const red = "Marko;Marković;17;Kula";` u niz, pa ispiši dve poruke: `"Marko Marković (17) živi u mestu Kula."` i `"Sledeće godine ima 18 godina."` Pitanje: šta dobiješ ako zaboraviš `Number(…)`?

**B6. Zamena reči** · osnovni

U `const poruka = "Ovaj zadatak je težak, baš težak.";` zameni svaku reč „težak“ rečju „lak“. Zatim probaj `replace` umesto `replaceAll` i zapiši razliku.

**B7. Lepo ime** · srednji

Napiši funkciju `lepoIme(unos)`: `"   mARKO   "` → `"Marko"`, `"aNA"` → `"Ana"`, `"  petar"` → `"Petar"`.

**B8. Palindrom** · srednji

Napiši funkciju `palindrom(rec)` koja vraća `true` ako se reč isto čita s obe strane, bez obzira na velika i mala slova: `"Kapak"` → `true`, `"Kula"` → `false`, `"Anavolimilovana"` → `true`.

**B9. Skraćivanje teksta** · bonus

Napiši funkciju `skrati(tekst, n)`: ako je tekst duži od n znakova, vrati prvih n znakova i `"..."`, inače ceo tekst. `skrati("Web programiranje", 7)` → `"Web pro..."`, `skrati("JS", 7)` → `"JS"`.

**B10. Broj reči** · bonus

Koliko reči ima u `"  Danas učimo JavaScript u Kuli  "`? Očekivano: `5`. Pomoć: prvo `trim`, pa `split`.

### C. Nađi i ispravi grešku (10 min)

Na papiru napiši šta kod stvarno ispisuje, zašto, i kako da se ispravi. Tek onda proveri u konzoli.

**C1.**

```javascript
const unos = "  ana  ";
unos.trim();
console.log("[" + unos + "]");   // želimo: [ana]
```

**C2.**

```javascript
const a = "5";
const b = "10";
console.log("Zbir je " + a + b);   // želimo: Zbir je 15
```

**C3.**

```javascript
const jezik = "JavaScript";
console.log(jezik.slice(0, 3));   // želimo: Java
```

**C4.** · teži

```javascript
const recenica = "Volim JS";
if (recenica.indexOf("Volim")) {
  console.log("Nađeno!");
}
// ništa se ne ispisuje — zašto?
```

## 2. čas — Datumi

Cilj časa: napraviti datum bez greške sa mesecom, čitati i menjati njegove delove, izračunati razliku u danima i ispisati datum u obliku dd.mm.gggg.

Na početku časa unesi u konzolu dve pomoćne konstante — trebaju u većini zadataka:

```javascript
const dani = ["nedelja", "ponedeljak", "utorak", "sreda", "četvrtak", "petak", "subota"];
const DAN = 1000 * 60 * 60 * 24;   // 86 400 000 ms
```

### A. Predvidi, pa proveri (10 min)

Prvo upiši odgovor na papir, pa proveri u konzoli. Kod pitanja „koji je datum“ napiši ga rečima, npr. „4. jun 2026.“

```javascript
const d = new Date(2026, 10, 20, 14, 5);
```

| # | Izraz ili pitanje | Tvoj odgovor |
| --- | --- | --- |
| 1 | `d.getFullYear()` |  |
| 2 | `d.getMonth()` |  |
| 3 | `d.getDate()` |  |
| 4 | `d.getDay()` — napiši i naziv dana |  |
| 5 | `d.getHours()` |  |
| 6 | `d.getMinutes()` |  |
| 7 | `("0" + d.getMinutes()).slice(-2)` |  |
| 8 | `"Mesec: " + d.getMonth() + 1` |  |
| 9 | `"Mesec: " + (d.getMonth() + 1)` |  |
| 10 | Koji je mesec u `new Date(2026, 11, 25)`? |  |
| 11 | Koji je datum `new Date(2026, 9, 32)`? |  |
| 12 | `new Date(2026, 2, 0).getDate()` |  |
| 13 | `new Date(2027, 0, 1) - new Date(2026, 11, 31)` |  |
| 14 | `new Date(2026, 0, 1) == new Date(2026, 0, 1)` |  |
| 15 | `new Date(2026, 0, 1) < new Date(2026, 0, 2)` |  |
| 16 | Posle `const x = new Date(2026, 4, 10); x.setDate(x.getDate() + 25);` koji je datum `x`? |  |

### B. Zadaci u konzoli (25 min)

Zadaci idu od lakših ka težim. Osnovne i srednje uradi svi; bonus ko stigne.

**D1. Delovi datuma** · osnovni

Napravi datum 31. decembar 2026. u 23:59. Ispiši godinu, mesec kao broj od 1 do 12, dan, sat, minut i naziv dana u nedelji. Očekivano: `2026`, `12`, `31`, `23`, `59`, `"četvrtak"`.

**D2. Dan u nedelji** · osnovni

Napiši funkciju `danUNedelji(datum)` koja vraća naziv dana. Testiraj: 25. 12. 2026. → `"petak"`, 1. 9. 2026. → `"utorak"`, 1. 1. 2027. → `"petak"`.

**D3. Datum u obliku dd.mm.gggg.** · osnovni

Napiši funkciju `formatiraj(datum)` i funkciju `formatirajSaVremenom(datum)`. Za 5. mart 2026. u 08:07 očekivano je `"05.03.2026."` i `"05.03.2026. 08:07"`. Savet: napravi pomoćnu funkciju `dvocifreno(n)`.

**D4. Razlika u danima** · srednji

Koliko dana ima od 1. septembra do 31. decembra 2026? Prvo ispiši rezultat bez `Math.round`, pa sa njim. Očekivano: `121.0416…`, pa `121`. Pitanje: zašto prvi broj nije ceo?

**D5. Rok za predaju** · srednji

Napiši funkciju `rok(datum, brojDana)` koja vraća **nov** datum, a original ostaje isti. 20. 11. 2026. + 15 dana → `"05.12.2026."`, a originalni datum je i dalje `"20.11.2026."` (proveri funkcijom `formatiraj`).

**D6. Broj dana u mesecu** · srednji

Napiši funkciju `daniUMesecu(godina, mesec)`, gde je mesec „ljudski“, od 1 do 12. `(2026, 2)` → `28`, `(2028, 2)` → `29`, `(2026, 11)` → `30`, `(2026, 12)` → `31`. Pomoć: dan `0`.

**D7. Datum iz polja forme** · srednji

Polje `input type="date"` vraća tekst kao `const tekst = "2026-12-24";`. Napravi lokalni datum i ispiši `"24. decembar 2026."` na dva načina: sa `toLocaleDateString("sr-Latn-RS", …)` i sa nizom naziva meseci. Zatim proveri `new Date(tekst).getHours()` i objasni zašto nije `0`.

**D8. Vikend** · osnovni

Napiši funkciju `jeVikend(datum)`: 21. 11. 2026. → `true`, 22. 11. 2026. → `true`, 23. 11. 2026. → `false`.

**D9. Godine starosti** · bonus

Napiši funkciju `godine(rodjen, danas)`. Za dan 20. 11. 2026: rođen 27. 11. 2009. → `16`, rođen 15. 3. 2009. → `17`, rođen 20. 11. 2009. → `17`.

**D10. Datum iz srpskog zapisa** · bonus

Napiši funkciju `izTeksta(t)` koja od `"24.12.2026."` pravi objekat `Date`. Provera: `formatiraj(izTeksta("24.12.2026."))` → `"24.12.2026."`, a `danUNedelji(…)` → `"četvrtak"`. Pomoć: `split(".")`, `Number`, mesec − 1.

### C. Nađi i ispravi grešku (10 min)

Na papiru napiši šta kod stvarno daje, zašto, i kako da se ispravi. Tek onda proveri u konzoli.

**E1.**

```javascript
const novaGodina = new Date(2027, 1, 1);   // želimo: 1. januar 2027.
```

**E2.**

```javascript
const danas = new Date(2026, 10, 20);
const zaNedelju = danas;
zaNedelju.setDate(zaNedelju.getDate() + 7);
console.log(danas.getDate());   // želimo: 20
```

**E3.**

```javascript
const d = new Date(2026, 10, 20);
console.log("Mesec: " + d.getMonth() + 1);   // želimo: Mesec: 11
```

**E4.**

```javascript
const dani = ["ponedeljak", "utorak", "sreda", "četvrtak", "petak", "subota", "nedelja"];
console.log(dani[new Date(2026, 10, 20).getDay()]);   // 20. 11. 2026. je petak
```

**E5.** · teži

```javascript
const d1 = new Date(2026, 0, 1);
const d2 = new Date(2026, 0, 1);
if (d1 == d2) {
  console.log("Isti dan!");
}
// ništa se ne ispisuje — zašto?
```
