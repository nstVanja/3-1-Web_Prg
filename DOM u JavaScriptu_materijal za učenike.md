# DOM u JavaScriptu — materijal za učenike

Web programiranje · 3. razred, smer IT · Nov 26, 2026 · @Vanja

## 1. Šta je DOM

**DOM (Document Object Model) je stablo objekata koje pregledač napravi od HTML-a stranice.** JavaScript preko DOM-a čita i menja stranicu: tekst, slike i stilove, a može i da dodaje i briše elemente.

Kad pregledač učita HTML fajl, u memoriji napravi model stranice. Na ekranu se uvek prikazuje taj model, a ne sam fajl. Zato važi sledeće:

- kad JavaScript promeni DOM, promena se odmah vidi na stranici;
- HTML fajl na disku ili serveru ostaje isti, pa posle osvežavanja stranice (F5) promene nestaju.

Sama skraćenica objašnjava pojam:

| Slovo | Reč | Značenje |
| --- | --- | --- |
| D | Document | cela veb-stranica, onakva kakvu ju je pregledač učitao |
| O | Object | svaki tag postaje objekat sa svojstvima (tekst, klase, stil) i metodama |
| M | Model | objekti su povezani u stablo: roditelji, deca, braća i sestre |

U JavaScriptu do DOM-a stižemo preko objekta `document`. Otvori bilo koju stranicu, pritisni F12, izaberi karticu Console i probaj:

```js
document.title                           // naslov stranice
document.body                            // element <body>
document.body.style.background = "pink"; // promeni boju pozadine
```

**Zapamti:** DOM nije deo samog jezika JavaScript, nego ga JavaScriptu nudi pregledač. Zato u Node.js-u ne postoji objekat `document`.

## 2. DOM stablo i čvorovi

**Svaki tag napisan unutar drugog taga postaje njegovo dete u stablu.** Na vrhu stabla je koren, objekat `document`. Pogledaj ovaj HTML i stablo koje pregledač od njega napravi:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Moja stranica</title>
  </head>
  <body>
    <h1>Dobrodošli</h1>
    <ul>
      <li>HTML</li>
      <li>CSS</li>
    </ul>
  </body>
</html>
```

&#91;embedded content: DOM stablo primera · 6 nivoa\]

Uvlačenje u kodu odgovara nivoima u stablu. Tekst unutar taga je poseban, tekstualni čvor (isprekidani okvir).

U stablu postoje tri vrste čvorova koje treba da razlikuješ:

- **Element čvor** — svaki HTML tag (`<body>`, `<h1>`, `<li>`). Sa njima radimo najčešće.
- **Tekstualni čvor** — tekst unutar taga, na primer „Dobrodošli“ unutar `<h1>`.
- **Koren** — objekat `document`, vrh stabla. Odatle počinje svaka pretraga.

Odnose između čvorova opisujemo kao u porodici. Engleske nazive treba da znaš jer se pojavljuju u imenima svojstava.

| Odnos | Primer iz našeg stabla |
| --- | --- |
| roditelj (parent) | `<body>` je roditelj od `<h1>` |
| dete (child) | `<h1>` i `<ul>` su deca od `<body>` |
| brat/sestra (sibling) | `<head>` i `<body>` imaju istog roditelja |
| potomak (descendant) | `<li>` je potomak od `<body>` (dete deteta) |
| predak (ancestor) | `<html>` je predak svih elemenata |

To isto stablo možeš da vidiš u pregledaču: F12, kartica Elements. Klikom na trouglić otvaraš granu stabla.

## 3. Pronalaženje elemenata

**Pre nego što promeniš element, moraš da ga pronađeš.** Za to služe metode objekta `document`. Mi ćemo najčešće koristiti `querySelector` i `querySelectorAll`, jer rade sa CSS selektorima koje već znaš.

| Metoda | Šta traži | Šta vraća |
| --- | --- | --- |
| `getElementById("naslov")` | element sa id="naslov" | jedan element ili `null` |
| `getElementsByClassName("stavka")` | sve elemente sa klasom stavka | HTMLCollection (živa lista) |
| `getElementsByTagName("li")` | sve `<li>` elemente | HTMLCollection (živa lista) |
| `querySelector(".stavka")` | prvi element koji odgovara CSS selektoru | jedan element ili `null` |
| `querySelectorAll(".stavka")` | sve elemente koji odgovaraju selektoru | NodeList (statična lista) |

Ime metode pišeš tačno ovako. JavaScript razlikuje velika i mala slova, pa `getElementByID` ne postoji.

### querySelector i CSS selektori

Selektor pišeš isto kao u CSS fajlu. Ako mu odgovara više elemenata, `querySelector` vraća samo prvi.

```js
document.querySelector("#naslov");            // element sa id="naslov"
document.querySelector(".stavka");            // prvi element sa klasom stavka
document.querySelector("p");                  // prvi paragraf
document.querySelector("nav a");              // prvi link unutar <nav>
document.querySelector("input[type='text']"); // prvo tekstualno polje
document.querySelectorAll("li.vazno");        // SVI <li> sa klasom vazno
```

**Pazi:** `querySelector("naslov")` ne traži id, nego tag `<naslov>`, i vratiće `null`. Za id piši tarabu (`#naslov`), a za klasu tačku (`.stavka`).

### Jedan element ili lista?

`querySelector` vraća jedan element, a `querySelectorAll` listu elemenata (NodeList). Lista ima svojstvo `length` i metodu `forEach`.

```js
// jedan element
const naslov = document.querySelector("h1");
naslov.textContent = "Zdravo!";

// lista elemenata
const stavke = document.querySelectorAll("li");
console.log(stavke.length);   // broj stavki

stavke.forEach(function (li) {
  li.style.color = "blue";
});
```

Lista nema stil, pa `stavke.style.color = "blue"` ne radi. Kroz listu moraš da prođeš petljom i menjaš svaki element posebno.

Lista koju vraćaju `getElementsByClassName` i `getElementsByTagName` (HTMLCollection) nema `forEach`. Kroz nju prolaziš običnom `for` petljom:

```js
const stavke = document.getElementsByTagName("li");
for (let i = 0; i < stavke.length; i++) {
  stavke[i].style.color = "blue";
}
```

**Savet:** element koji koristiš više puta pronađi jednom i sačuvaj ga u konstantu (`const naslov = …`). Kod je brži i pregledniji.

## 4. Menjanje sadržaja, atributa i izgleda

**Kad imaš element, menjaš ga preko njegovih svojstava i metoda.** Isto svojstvo služi i za čitanje i za upisivanje vrednosti.

### Sadržaj elementa

```js
const poruka = document.querySelector("#poruka");

console.log(poruka.textContent);                 // čitanje
poruka.textContent = "Podaci su sačuvani";       // samo tekst
poruka.innerHTML = "<b>Podaci</b> su sačuvani";  // tumači HTML tagove
```

| Svojstvo | Šta radi | Kada ga koristiš |
| --- | --- | --- |
| `textContent` | čita i upisuje čist tekst; tagovi se prikažu kao obični znaci | po pravilu, uvek kad menjaš tekst |
| `innerText` | kao `textContent`, ali vraća samo tekst koji se vidi na ekranu | kad ti treba baš vidljiv tekst |
| `innerHTML` | čita i upisuje HTML; pregledač tumači tagove | samo za HTML koji si sam napisao |

**Bezbednost:** nikad ne ubacuj tekst koji je uneo korisnik preko `innerHTML`. Ako korisnik upiše HTML sa skriptom, pregledač će je izvršiti (napad se zove XSS). Tekst korisnika uvek upisuj preko `textContent`.

### Atributi

Atribute čitaš, postavljaš i brišeš metodama `getAttribute`, `setAttribute` i `removeAttribute`. Česte atribute (`src`, `href`, `alt`, `id`) možeš da menjaš i direktno, kao svojstvo.

```js
const slika = document.querySelector("#logo");

slika.setAttribute("src", "macka.png");
console.log(slika.getAttribute("alt"));
slika.removeAttribute("title");

slika.src = "macka.png";   // kraći zapis, isto radi
slika.alt = "Mačka";
```

### Vrednost polja forme

Ono što je korisnik upisao u `<input>` čitaš preko svojstva `value`, a ne preko `textContent`.

```js
const unos = document.querySelector("#ime");

console.log(unos.value);   // šta je korisnik upisao
unos.value = "";           // isprazni polje
unos.disabled = true;      // isključi polje
```

`value` je uvek string, čak i kad korisnik upiše broj. Za računanje ga pretvori u broj: `Number(unos.value)`.

### Izgled: style i classList

Svojstvo `style` menja jedno CSS svojstvo direktno na elementu. Imena se pišu kao camelCase: `background-color` postaje `backgroundColor`. Vrednost je string i mora da ima jedinicu (`"32px"`, a ne `32`).

```js
const kutija = document.querySelector(".kutija");

kutija.style.backgroundColor = "gold";
kutija.style.fontSize = "32px";
kutija.style.display = "none";   // sakrij element
```

Svojstvo `classList` dodaje i uklanja CSS klase. Izgled ostaje opisan u CSS fajlu, pa je ovo bolji način.

```js
kutija.classList.add("aktivna");
kutija.classList.remove("skrivena");
kutija.classList.toggle("tamna");       // ima je: ukloni; nema je: dodaj
kutija.classList.contains("aktivna");   // true ili false
```

**Pravilo dobre prakse:** JavaScript menja klasu, a CSS odlučuje kako ta klasa izgleda. Primer tamne teme:

```css
/* stil.css */
.tamna {
  background: #222;
  color: white;
}
```

```js
// skripta.js
const dugme = document.querySelector("#tema");
dugme.addEventListener("click", function () {
  document.body.classList.toggle("tamna");
});
```

## 5. Kreiranje, ubacivanje i brisanje elemenata

**Novi element praviš u tri koraka: napravi, popuni, ubaci.** Dok ga ne ubaciš u stablo, element postoji samo u memoriji i ne vidi se na stranici.

1. **Napravi** — `document.createElement("li")` pravi novi, prazan element.
2. **Popuni** — dodaj mu tekst, klasu ili atribute, isto kao postojećem elementu.
3. **Ubaci** — `roditelj.append(element)` ga stavlja u stablo. Tek tada se vidi.

```js
const lista = document.querySelector("#kupovina");

const li = document.createElement("li");   // 1. napravi
li.textContent = "Mleko";                  // 2. popuni
li.classList.add("stavka");
lista.append(li);                          // 3. ubaci
```

Ako te korake staviš u petlju, od niza podataka dobijaš celu listu:

```js
const namirnice = ["Mleko", "Hleb", "Jaja"];
const lista = document.querySelector("#kupovina");

namirnice.forEach(function (naziv) {
  const li = document.createElement("li");
  li.textContent = naziv;
  lista.append(li);
});
```

### Gde ubaciti, kako obrisati

| Metoda | Šta radi |
| --- | --- |
| `roditelj.append(el)` | dodaje `el` kao poslednje dete roditelja |
| `roditelj.prepend(el)` | dodaje `el` kao prvo dete roditelja |
| `el.before(novi)` | ubacuje `novi` ispred `el` |
| `el.after(novi)` | ubacuje `novi` iza `el` |
| `el.remove()` | briše `el` sa stranice |

```js
const prva = document.querySelector("li");
prva.remove();   // prva stavka nestaje sa stranice
```

Ako metodi `append` proslediš element koji je već na stranici, on se premešta na novo mesto, a ne kopira.

U starijim primerima na internetu srećeš metode `appendChild`, `insertBefore` i `removeChild`. Rade isto, samo se duže pišu.

## 6. Kretanje kroz stablo

**Od jednog elementa možeš da dođeš do njegovog roditelja, dece i braće, bez nove pretrage.** Svako od ovih svojstava vraća element, ili `null` ako tamo nema ničega.

| Svojstvo | Vodi do |
| --- | --- |
| `el.parentElement` | roditelja |
| `el.children` | sve dece (HTMLCollection) |
| `el.firstElementChild` | prvog deteta |
| `el.lastElementChild` | poslednjeg deteta |
| `el.nextElementSibling` | sledećeg brata/sestre |
| `el.previousElementSibling` | prethodnog brata/sestre |

```js
const lista = document.querySelector("ul");

console.log(lista.parentElement);       // roditelj, npr. <body>
console.log(lista.children.length);     // broj stavki u listi
console.log(lista.firstElementChild);   // prvi <li>
console.log(lista.nextElementSibling);  // element posle liste
```

Ovo je korisno kad dugme treba da obriše red u kome se nalazi: `dugme.parentElement.remove()`.

Koristi svojstva sa rečju **Element** u imenu. Slična svojstva `parentNode`, `childNodes` i `nextSibling` vraćaju i tekstualne čvorove, a to su često samo razmaci i novi redovi iz HTML-a.

## 7. Događaji

**Događaj je nešto što se desi na stranici: klik, kucanje, slanje forme.** Na događaj reaguješ funkcijom za obradu događaja, koju elementu dodeliš metodom `addEventListener`.

```js
element.addEventListener("vrsta događaja", funkcija);
```

Funkcija se ne izvršava odmah, nego svaki put kad se događaj desi. Primer: brojač klikova.

```js
const dugme = document.querySelector("#dugme");
const brojac = document.querySelector("#brojac");
let klikovi = 0;

dugme.addEventListener("click", function () {
  klikovi++;
  brojac.textContent = klikovi;
});
```

| Događaj | Kada se desi |
| --- | --- |
| `click` | klik mišem |
| `input` | svaka promena teksta u polju |
| `change` | promena izbora (select, checkbox) |
| `submit` | slanje forme |
| `keydown` | pritisak tastera |
| `mouseover` | miš pređe preko elementa |

Događaj `input` se dešava pri svakom otkucanom slovu, pa stranica može da reaguje dok korisnik kuca:

```js
const ime = document.querySelector("#ime");
const pozdrav = document.querySelector("#pozdrav");

ime.addEventListener("input", function () {
  pozdrav.textContent = "Zdravo, " + ime.value + "!";
});
```

### Funkciju prosledi bez zagrada

Ako funkciju imaš sa imenom, proslediš je bez zagrada. Sa zagradama se ona izvrši odmah, a ne na klik.

```js
function pozdravi() {
  alert("Zdravo!");
}

dugme.addEventListener("click", pozdravi);     // ispravno
dugme.addEventListener("click", pozdravi());   // greška: izvrši se odmah
```

### Objekat događaja

Funkcija za obradu događaja može da primi parametar, obično nazvan `e`. U njemu su podaci o događaju: `e.target` je element na kome se događaj desio, a `e.key` taster koji je pritisnut.

```js
const unos = document.querySelector("#unos");

unos.addEventListener("keydown", function (e) {
  if (e.key === "Enter") {
    console.log("Pritisnut je Enter, a polje je", e.target);
  }
});
```

Kod forme, `e.preventDefault()` u funkciji za događaj `submit` sprečava ponovno učitavanje stranice.

## 8. Gde postaviti `<script>`

**Skripta može da pronađe element tek kad ga je pregledač pročitao iz HTML-a.** Ako se skripta izvrši ranije, `querySelector` vraća `null`, a sledeći red javlja grešku:

```
TypeError: Cannot read properties of null (reading 'textContent')
```

Postoje dva ispravna rešenja. U primerima koristimo drugo.

**Rešenje 1:** tag `<script>` stavi na kraj, neposredno pre `</body>`.

```html
<body>
  <h1 id="naslov">Zdravo</h1>
  <script src="skripta.js"></script>
</body>
```

**Rešenje 2:** tag `<script>` ostavi u `<head>` i dodaj mu atribut `defer`. Pregledač tada preuzme skriptu odmah, ali je izvrši tek kad pročita ceo HTML.

```html
<head>
  <script src="skripta.js" defer></script>
</head>
```

## 9. Kompletan primer: lista zadataka

**Aplikacija koristi sve iz ove lekcije: pronalazi elemente, pravi nove, menja klase i reaguje na klik.** Upišeš zadatak i klikneš Dodaj, pa se zadatak pojavi u listi. Klik na zadatak ga precrta, a drugi klik vraća nazad.

Napravi folder `lista-zadataka` i u njemu tri fajla. Kad ih sačuvaš, otvori `index.html` u pregledaču.

**index.html**

```html
<!DOCTYPE html>
<html lang="sr">
<head>
  <meta charset="UTF-8">
  <title>Moji zadaci</title>
  <link rel="stylesheet" href="stil.css">
  <script src="zadaci.js" defer></script>
</head>
<body>
  <h1>Moji zadaci</h1>
  <input id="unos" type="text" placeholder="Novi zadatak">
  <button id="dodaj">Dodaj</button>
  <ul id="lista"></ul>
</body>
</html>
```

**stil.css**

```css
body {
  font-family: Arial, sans-serif;
  max-width: 500px;
  margin: 40px auto;
}

li {
  cursor: pointer;
  padding: 6px 0;
}

.gotovo {
  text-decoration: line-through;
  color: gray;
}
```

**zadaci.js**

```js
const unos = document.querySelector("#unos");       // 1
const dugme = document.querySelector("#dodaj");
const lista = document.querySelector("#lista");

dugme.addEventListener("click", function () {      // 2
  const tekst = unos.value.trim();                  // 3
  if (tekst === "") return;

  const li = document.createElement("li");          // 4
  li.textContent = tekst;
  li.addEventListener("click", function () {
    li.classList.toggle("gotovo");
  });
  lista.append(li);
  unos.value = "";                                  // 5
});
```

Kako kod radi, po označenim redovima:

1. Elemente koje ćemo koristiti pronađemo jednom, na početku.
2. Funkcija za obradu klika na dugme Dodaj.
3. Pročitamo unos; `trim()` skida razmake sa početka i kraja. Ako je polje prazno, `return` prekida funkciju i ništa se ne dodaje.
4. Napravimo `<li>`, upišemo tekst preko `textContent` i dodamo mu klik koji uključuje i isključuje klasu `gotovo`. Zatim ga ubacimo u listu.
5. Ispraznimo polje za sledeći zadatak.

Lista je u HTML-u prazna, sve stavke pravi JavaScript. Klasa `.gotovo` postoji u CSS-u od početka, a JavaScript je samo uključuje i isključuje.

## 10. Česte greške

**Kad nešto ne radi, prvo otvori konzolu (F12 → Console) i pročitaj poruku.** `null` znači da element nije pronađen, a „is not a function“ da je ime metode pogrešno napisano.

| Napisano | Šta se desi | Ispravno |
| --- | --- | --- |
| `getElementByID("naslov")` | greška: „… is not a function“ | `getElementById` — tačno tim slovima |
| `querySelector("naslov")` za id | vraća `null` | `querySelector("#naslov")` |
| `stavke.style.color = "red"` na listi | ništa se ne promeni | `stavke.forEach(…)` i menjaj svaki element |
| `addEventListener("click", obradi())` | funkcija se izvrši odmah, a ne na klik | `addEventListener("click", obradi)` |
| `<script>` u `<head>` bez `defer` | „Cannot read properties of null“ | dodaj `defer` ili skriptu stavi na kraj `<body>` |
| `createElement` bez `append` | element se ne vidi | `lista.append(li)` |
| `textContent` na polju `<input>` | dobijaš prazan string | `unos.value` |
| tekst korisnika preko `innerHTML` | rizik od XSS napada | `textContent` |

## 11. Pitanja za proveru znanja

Odgovori u svesci, svojim rečima. Ako ne znaš odgovor, vrati se na deo lekcije naveden u zagradi.

1. Šta je DOM i ko ga pravi? (deo 1)
2. Zašto promena koju napraviš u konzoli nestane posle osvežavanja stranice? (deo 1)
3. Ko je roditelj, a ko su deca elementa `<body>` u primeru iz dela 2? (deo 2)
4. Koja je razlika između `querySelector` i `querySelectorAll`? (deo 3)
5. Šta vraća `document.querySelector(".stavka")` ako na stranici ima pet takvih elemenata? A ako nema nijednog? (deo 3)
6. Zašto je `textContent` bezbedniji od `innerHTML`? (deo 4)
7. Kako se u JavaScriptu zove CSS svojstvo `font-size`? (deo 4)
8. Šta radi `classList.toggle("tamna")`? (deo 4)
9. Šta moraš da uradiš da bi se novi element video na stranici? (deo 5)
10. Zašto u `addEventListener("click", obradi)` pišemo `obradi` bez zagrada? (deo 7)
11. Navedi dva razloga zbog kojih `querySelector` može da vrati `null`. (delovi 3 i 8)

## 12. Zadaci za vežbu

**Zadaci idu od lakših ka težim, a zadatak 5 nastavlja kompletan primer sa časa.** Svaki zadatak radi u posebnom folderu, sa odvojenim HTML i JS fajlom. Posle svake promene sačuvaj fajl, osveži stranicu i pogledaj konzolu.

### Zadatak 1 — Dobar dan, dobro veče (lak)

Početni kod, `index.html`:

```html
<!DOCTYPE html>
<html lang="sr">
<head>
  <meta charset="UTF-8">
  <title>Zadatak 1</title>
  <style>
    .istaknuto { background: gold; padding: 8px; }
  </style>
  <script src="zadatak1.js" defer></script>
</head>
<body>
  <h1 id="naslov">Dobar dan</h1>
  <p class="opis">Ovo je moja prva DOM stranica.</p>
  <button id="promeni">Promeni</button>
</body>
</html>
```

U fajlu `zadatak1.js` napiši kod tako da se posle klika na dugme:

- naslov promeni u „Dobro veče“;
- paragrafu doda klasa `istaknuto`;
- dugme isključi (svojstvo `disabled`).

### Zadatak 2 — Prekidač sijalice (lak)

U folder stavi dve slike po izboru, `ugasena.png` i `upaljena.png`. Na stranici su slika i dugme:

```html
<img id="sijalica" src="ugasena.png" alt="Ugašena sijalica" width="200">
<button id="prekidac">Upali</button>
```

Klik na dugme pali i gasi sijalicu: menja `src` i `alt` slike i tekst dugmeta („Upali“ / „Ugasi“). Pomoć: stanje pamti u promenljivoj `let upaljena = false;`.

### Zadatak 3 — Lista iz niza (srednji)

Na stranici je prazna lista `<ul id="predmeti"></ul>`, a u JS fajlu niz:

```js
const predmeti = ["Matematika", "Srpski jezik", "Web programiranje", "Engleski jezik", "Fizičko vaspitanje"];
```

- Za svaki predmet napravi `<li>` i ubaci ga u listu.
- Stavka „Web programiranje“ dobija klasu `omiljeni`, koju sam definišeš u CSS-u.
- Ispod liste napravi paragraf „Ukupno predmeta: 5“. Broj ne piši ručno, nego ga pročitaj iz liste (`children.length`).

### Zadatak 4 — Brojač karaktera (srednji)

```html
<textarea id="poruka" rows="4" cols="40"></textarea>
<p id="brojac">0 / 100</p>
```

Dok korisnik kuca (događaj `input`), paragraf pokazuje koliko je znakova upisano, npr. „37 / 100“. Kad broj pređe 100, paragraf dobija klasu `previse` (crvena boja), a kad se vrati ispod, klasa se uklanja. Pomoć: broj znakova je `poruka.value.length`.

### Zadatak 5 — Proširi listu zadataka (teži)

Pođi od kompletnog primera iz dela 9 i dodaj, redom:

1. Svaka stavka dobija dugme „Obriši“ koje briše samo svoju stavku. Pomoć: dugme napravi sa `createElement("button")`, ubaci ga u `li`, a u njegovoj funkciji za obradu događaja pozovi `li.remove()`. Tekst zadatka upiši u poseban `<span>`, da ga dugme ne prekrije.
2. Zadatak se dodaje i pritiskom na Enter u polju za unos. Pomoć: događaj `keydown` i `e.key === "Enter"`. Da ne bi pisao isti kod dva puta, dodavanje premesti u posebnu funkciju `dodajZadatak()`.
3. Ispod liste piše „Preostalo: 3“ — broj stavki koje nisu precrtane. Pomoć: napiši funkciju koja prebroji stavke i pozivaj je posle svake promene.
4. Dugme „Obriši završene“ uklanja sve precrtane stavke odjednom. Pomoć: `document.querySelectorAll(".gotovo")` i `forEach`.

**Dodatno:** napravi dugme koje uključuje i isključuje tamnu temu cele stranice (primer je u delu 4).
