# Listat ja suodatus

## HTML-rakenne

- HTML:llä rakennetaan sivun rakenne: otsikot, listat, napit, inputit ja divit.
- Tärkeää:
  - `id`-attribuutit, joilla JavaScript voi löytää elementit.
  - `class`-attribuutit tyylittämistä varten.
  - Listat: `<ul>` (unordered list) ja `<ol>` (ordered list).
  - Syöttökenttä: `<input type="text">`.

## CSS-perusteet

- CSS määrittää ulkoasun: värit, fontit, marginaalit, leveydet.
- CSS:n perusrakenne eli “malli” on täsmälleen tämä:

```css
selektori {
  tyylimäärittely: arvo;
}
```

- Selektori määrittää mihin tyylimäärittely kohdistuu

```
| Selektori   | Käyttö              | Esimerkki   | Kohdistuu                   |
-------------------------------------------------------------------------------
| element	  | HTML-elementtiin    |	p {}	  | kaikki <p>                  |
| .luokka	  | class-attribuuttiin	| .nimi {}	  | kaikki, joilla class="nimi" |
| #id	      | id-attribuuttiin    | #otsikko {}	| yhteen elementtiin          |
-------------------------------------------------------------------------------
```

- Jos sama elementti osuu useaan sääntöön, tarkempi selektori voittaa (#id > .luokka > elementti).
- Esimerkikkejä tyylimäärittelyistä:
  - `display: flex;` asettaa elementit vierekkäin.
  - `padding: 10px;` ja `margin: 20px;` antavat tilaa elementtien ympärille.
  - `width: 100%` venyttää elementin käytettävissä olevaan tilaan.

## addEventListener — tapahtumakuuntelija

`addEventListener()` liittää HTML-elementtiin **kuuntelijan**, joka reagoi, kun käyttäjä tekee jotain (esim. klikkaa, kirjoittaa, vie hiiren päälle).

**Muoto**

```js
elementti.addEventListener("tapahtuma", funktio);
```

- tapahtuma: esim. "click", "input", "change", "keydown".

- funktio: mitä tapahtuu, kun tapahtuma aktivoituu.

**Esimerkki**

```js
const nappi = document.getElementById("nappi");

nappi.addEventListener("click", () => {
  console.log("Nappia klikattiin!");
});
```

- Tämä koodi tulostaa aina, kun käyttäjä klikkaa nappia.

## forEach — taulukonn läpikäynti

`forEach()` on taulukkometodi, joka suorittaa annetun funktion jokaiselle taulukon alkiolle.

**Muoto**

```js
taulukko.forEach((alkio) => {
  // tee jotain jokaiselle alkiolle
});
```

**Esimerkki**

```js
const hedelmat = ["omena", "banaani", "kiivi"];

hedelmat.forEach((hedelma) => {
  console.log("Hedelmä:", hedelma);
});
```

- Tämä tulostaa kaikki taulukon arvot yksi kerrallaan.

## filter — taulukon suodatus

`filter()` palauttaa uuden taulukon, joka sisältää vain ne alkiot, jotka täyttävät annetun ehdon.

**Muoto**

```js
const uusiTaulukkko = taulukko.filter((alkio) => ehto);
```

- Jos ehto palauttaa true, alkio jää uuteen taulukkoon. Jos false, ei.

**Esimerkki**

```js
const hedelmat = ["omena", "banaani", "kiivi", "appelsiini"];
const suodatetut = hedelmat.filter((h) => h.startsWith("a"));
console.log(suodatetut); // ["appelsiini"]
```

- `filter()` ei muuta alkuperäistä taulukkoa, vaan luo kopion, jossa on vain halutut arvot.

---

## Esimerkki 1: JavaScript-listasta HTML-lista

```html
<!DOCTYPE html>
<html lang="fi">
  <head>
    <meta charset="UTF-8" />
    <title>Esimerkki Lista</title>
  </head>
  <body>
    <h1>Esimerkkilista</h1>
    <ul id="lista"></ul>

    <script>
      const listaElementti = document.getElementById("lista");
      const hedelmat = ["Omena", "Banaani", "Appelsiini", "Kiivi"];

      // Luodaan <li> jokaiselle listan alkiolle
      hedelmat.forEach((item) => {
        const li = document.createElement("li");
        li.textContent = item;
        listaElementti.appendChild(li);
      });
    </script>
  </body>
</html>
```

Selitys:

- forEach käy läpi kaikki taulukon alkiot

- document.createElement("li") luo uuden listaelementin.

- textContent lisää tekstin elementtiin.

- appendChild lisää elementin DOM:iin.

## Esimerkki 2: Input ja tapahtumakuuntelija, joka suodattaa listaa

```html
<!DOCTYPE html>
<html lang="fi">
  <head>
    <meta charset="UTF-8" />
    <title>Suodatus</title>
  </head>
  <body>
    <h1>Suodata lista</h1>
    <input type="text" id="haku" placeholder="Hae hedelmiä..." />
    <ul id="lista"></ul>

    <script>
      const hakuInput = document.getElementById("haku");
      const listaElementti = document.getElementById("lista");
      const hedelmat = ["Omena", "Banaani", "Appelsiini", "Kiivi"];

      function naytaLista(items) {
        listaElementti.innerHTML = ""; // tyhjennetään lista
        items.forEach((item) => {
          const li = document.createElement("li");
          li.textContent = item;
          listaElementti.appendChild(li);
        });
      }

      // Alustetaan lista
      naytaLista(hedelmat);

      // Tapahtumakuuntelija inputille
      hakuInput.addEventListener("input", () => {
        const teksti = hakuInput.value.toLowerCase();
        const suodatetut = hedelmat.filter((item) =>
          item.toLowerCase().includes(teksti)
        );
        naytaLista(suodatetut);
      });
    </script>
  </body>
</html>
```

Selitys:

- addEventListener("input", ...) reagoi aina, kun käyttäjä kirjoittaa.

- filter() luo uuden taulukon niistä alkioista, jotka sisältävät haun tekstin.

- Funktio naytaLista päivittää HTML-listan aina haun mukaan.

## Tehtävät

- Luo oma lista kelikameroista (Hae lista kelikamoroista 1-api-tiedoston esimerkin avulla).

- Näytä lista HTML-sivulla samalla tavalla kuin esimerkissä.

- Lisää input-kenttä ja tee suodatustoiminto, joka piilottaa alkioita, jotka eivät vastaa käyttäjän syötettä.

- Kokeile muuttaa listan sisältöä dynaamisesti (input-kenttää käyttämällä) ja tarkkailla, miten suodatus toimii.
