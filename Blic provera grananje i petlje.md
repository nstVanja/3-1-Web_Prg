# Blic provera: grananje i petlje

Oct 22, 2026 · @Vanja

| Ime i prezime | Odeljenje | Bodovi |
| --- | --- | --- |
|  |  |  |

## Uputstvo

- Program pišeš i pokrećeš u konzoli pregledača: **F12 → Console**.
- Podatke unosiš preko prozora `prompt`, a sve poruke ispisuješ preko prozora `alert`.
- Poruke neka budu iste kao u zadatku.
- Zadaci su poređani od lakšeg ka težem. Rešavaj ih redom.

## Zadatak 1 — osnovni nivo: Agregatno stanje vode

**Koristi:** `if`, `else if`, `else`

Napiši program koji od korisnika traži da unese temperaturu vode u °C. Program zatim u prozoru ispisuje u kom je stanju voda:

| Uslov | Poruka |
| --- | --- |
| unos nije broj | Unos nije broj! |
| temperatura je manja od −273.15 | Takva temperatura ne postoji! |
| temperatura je 0 ili manja | Voda je u čvrstom stanju (led). |
| temperatura je veća od 0, a manja od 100 | Voda je u tečnom stanju. |
| temperatura je 100 ili veća | Voda je u gasovitom stanju (para). |

**Primer:** korisnik unese `25` → program ispiše *Voda je u tečnom stanju.*

**Pomoć:** da li unos nije broj, proveravaš funkcijom isNaN, na primer: `isNaN(t)`.

## Zadatak 2 — srednji nivo: Temperature u toku nedelje

**Koristi:** `for`, `if`

Napiši program koji od korisnika traži temperaturu za svaki od 7 dana u nedelji. Svaka temperatura se unosi u posebnom prozoru, sa porukom *Temperatura za dan 1:*, *Temperatura za dan 2:* i tako do 7.

Posle unosa program u **jednom** prozoru ispisuje:

1. koliko je dana temperatura bila ispod nule,
2. najvišu unetu temperaturu,
3. prosečnu temperaturu, zaokruženu na jednu decimalu.

**Primer:** korisnik unese −3, −1, 0, 2, 5, 4, 1 → program ispiše:

```
Dana sa minusom: 2
Najviša temperatura: 5
Prosek: 1.1
```

**Pomoć:** broj se na jednu decimalu zaokružuje sa `prosek.toFixed(1)`, a novi red u poruci dobijaš sa `"\n"`. Proveri program i za nedelju u kojoj je svih 7 temperatura u minusu.

## Zadatak 3 — napredni nivo: Kviz tablice množenja

**Koristi:** `do…while`, `for`, `if`

Napiši program za vežbanje tablice množenja.

1. Program pita *Koliko pitanja želiš (1-10)?* Ako unos nije ceo broj od 1 do 10, ispisuje *Unesi ceo broj od 1 do 10!* i pita ponovo, sve dok unos ne bude ispravan.
2. Za svako pitanje program bira dva slučajna broja od 1 do 10 i pita koliki je njihov proizvod, na primer *Pitanje 2/5:  7 x 8 = ?*
3. Posle svakog odgovora ispisuje *Tačno!* ili *Netačno! 7 x 8 = 56*.
4. Na kraju ispisuje broj tačnih odgovora, procenat i ocenu po tabeli:

| Procenat tačnih odgovora | Ocena |
| --- | --- |
| 90% i više | 5 |
| 75% do 89% | 4 |
| 60% do 74% | 3 |
| 50% do 59% | 2 |
| manje od 50% | 1 |

**Primer završne poruke:**

```
Tačnih: 3/4 (75%)
Ocena: 4
```

**Pomoć:**

- slučajan ceo broj od 1 do 10: `Math.floor(Math.random() * 10) + 1`
- ceo broj iz unosa: `parseInt(prompt("..."))`
- procenat zaokružuješ sa `Math.round(procenat)`
