# HTML & CSS peruspohja

## Teoria

### HTML-rakenne

- HTML:llä rakennetaan sivun rakenne: otsikot, listat, napit, inputit ja divit.
- Tärkeää:
  - `id`-attribuutit, joilla JavaScript voi löytää elementit.
  - `class`-attribuutit tyylittämistä varten.
  - Listat: `<ul>` (unordered list) ja `<ol>` (ordered list).
  - Syöttökenttä: `<input type="text">`.

### CSS-perusteet

- CSS määrittää ulkoasun: värit, fontit, marginaalit, leveydet.
- Esimerkiksi:
  - `display: flex` asettaa elementit vierekkäin.
  - `padding` ja `margin` antavat tilaa elementtien ympärille.
  - `width: 100%` venyttää elementin käytettävissä olevaan tilaan.

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

- filter() luo uuden listan niistä alkioista, jotka sisältävät haun tekstin.

- Funktio naytaLista päivittää HTML-listan aina haun mukaan.

## Tehtävät

- Luo oma lista kelikameroista (Hae lista kelikamoroista 1-api-tiedoston esimerkin avulla).

- Näytä lista HTML-sivulla samalla tavalla kuin esimerkissä.

- Lisää input-kenttä ja tee suodatustoiminto, joka piilottaa alkioita, jotka eivät vastaa käyttäjän syötettä.

- Kokeile muuttaa listan sisältöä dynaamisesti (input-kenttää käyttämällä) ja tarkkailla, miten suodatus toimii.
