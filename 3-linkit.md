# Linkkien lisääminen listan alkioihin

- Linkit (`<a>`) tekevät listan interaktiiviseksi: käyttäjä voi klikata ja siirtyä uuteen osoitteeseen.
- JavaScriptilla voidaan luoda linkkejä dynaamisesti.
- `href` määrittää kohdeosoitteen, ja `target="_blank"` avaa linkin uuteen välilehteen.
- Voidaan myös käyttää linkkiä kutsumaan funktiota suoraan (`onclick`), jolloin ei välttämättä mennä uuteen sivuun.

---

## Esimerkki 1: Staattinen HTML-lista linkkeineen

```html
<!DOCTYPE html>
<html lang="fi">
  <head>
    <meta charset="UTF-8" />
    <title>Lista linkkeineen</title>
  </head>
  <body>
    <h1>Hedelmiä ja tietoa niistä</h1>
    <ul>
      <li>
        <a href="https://fi.wikipedia.org/wiki/Omena" target="_blank">Omena</a>
      </li>
      <li>
        <a href="https://fi.wikipedia.org/wiki/Banaani" target="_blank"
          >Banaani</a
        >
      </li>
      <li>
        <a href="https://fi.wikipedia.org/wiki/Appelsiini" target="_blank"
          >Appelsiini</a
        >
      </li>
      <li>
        <a href="https://fi.wikipedia.org/wiki/Kiivi" target="_blank">Kiivi</a>
      </li>
    </ul>
  </body>
</html>
```

Selitys:

- Jokainen <li> sisältää <a>-elementin.

- target="\_blank" avaa Wikipedian sivun uuteen välilehteen.

## Esimerkki 2: Dynaaminen lista JavaScriptilla

```html
<!DOCTYPE html>
<html lang="fi">
  <head>
    <meta charset="UTF-8" />
    <title>Dynaaminen lista linkkeineen</title>
  </head>
  <body>
    <h1>Hedelmiä ja tietoa niistä</h1>
    <ul id="lista"></ul>

    <script>
      const listaElementti = document.getElementById("lista");
      const hedelmat = [
        { nimi: "Omena", linkki: "https://fi.wikipedia.org/wiki/Omena" },
        { nimi: "Banaani", linkki: "https://fi.wikipedia.org/wiki/Banaani" },
        {
          nimi: "Appelsiini",
          linkki: "https://fi.wikipedia.org/wiki/Appelsiini",
        },
        { nimi: "Kiivi", linkki: "https://fi.wikipedia.org/wiki/Kiivi" },
      ];

      hedelmat.forEach((item) => {
        const li = document.createElement("li");
        const a = document.createElement("a");
        a.textContent = item.nimi;
        a.href = item.linkki;
        a.target = "_blank"; // avaa uuteen välilehteen
        li.appendChild(a);
        listaElementti.appendChild(li);
      });
    </script>
  </body>
</html>
```

Selitys:

- Luodaan ensin <li> ja sen sisälle <a>-elementti.

- appendChild lisää linkin listaelementtiin ja listan DOMiin.

- Tämä tapa mahdollistaa listan sisällön dynaamisen muuttamisen (API-data, käyttäjän syötteet jne.).

## Esimerkki 3: Linkki, joka kutsuu JavaScript-funktiota

```html
<ul id="lista"></ul>

<script>
  const listaElementti = document.getElementById("lista");
  const hedelmat = ["Omena", "Banaani", "Appelsiini", "Kiivi"];

  function naytaInfo(nimi) {
    alert("Valitsit hedelmän: " + nimi);
  }

  hedelmat.forEach((item) => {
    const li = document.createElement("li");
    const a = document.createElement("a");
    a.textContent = item;
    a.href = "#"; // estää selaimen siirtymisen
    a.onclick = () => naytaInfo(item); // kutsuu funktiota
    li.appendChild(a);
    listaElementti.appendChild(li);
  });
</script>
```

Selitys:

- Linkki ei vie toiseen osoitteeseen (href="#"), vaan kutsuu funktiota onclick.

- Voidaan käyttää esim. näyttämään lisätietoa, avaamaan kuvia tms.

## Esimerkki 4: Lista olioista – linkin klikkaus lisää kuvan sivulle

Tässä esimerkissä luodaan lista olioista, joissa on **nimi** ja **kuvan URL**.  
Kun käyttäjä klikkaa linkkiä, JavaScript lisää sivulle kuvan dynaamisesti.

````html
## Esimerkki 4: Lista olioista – linkin klikkaus ja “Näytä kaikki” -nappi Tässä
esimerkissä luodaan lista olioista, joissa on nimi ja kuvan osoite. Käyttäjä voi
klikata yksittäistä linkkiä lisätäkseen kuvan, tai painaa nappia, joka näyttää
kaikki kerralla. ```html
<!DOCTYPE html>
<html lang="fi">
  <head>
    <meta charset="UTF-8" />
    <title>Listasta kuva sivulle</title>
    <style>
      body {
        font-family: Arial, sans-serif;
        margin: 2rem;
      }

      #kuvat {
        margin-top: 1rem;
        display: flex;
        gap: 10px;
        flex-wrap: wrap;
      }

      #kuvat img {
        width: 200px;
        border-radius: 8px;
        box-shadow: 0 2px 6px rgba(0, 0, 0, 0.3);
      }

      button {
        margin-top: 1rem;
        padding: 0.5rem 1rem;
        font-size: 1rem;
        border: none;
        border-radius: 6px;
        background-color: #0078d4;
        color: white;
        cursor: pointer;
      }

      button:hover {
        background-color: #005fa3;
      }
    </style>
  </head>
  <body>
    <h1>Hedelmägalleria</h1>
    <ul id="lista"></ul>

    <button id="naytaKaikki">Näytä kaikki kuvat</button>

    <h2>Lisätyt kuvat:</h2>
    <div id="kuvat"></div>

    <script>
      const listaElementti = document.getElementById("lista");
      const kuvatDiv = document.getElementById("kuvat");
      const naytaKaikkiBtn = document.getElementById("naytaKaikki");

      // Lista olioista, joilla on nimi ja kuvan osoite
      const hedelmat = [
        {
          nimi: "Omena",
          kuva: "https://upload.wikimedia.org/wikipedia/commons/1/15/Red_Apple.jpg",
        },
        {
          nimi: "Banaani",
          kuva: "https://upload.wikimedia.org/wikipedia/commons/8/8a/Banana-Single.jpg",
        },
        {
          nimi: "Appelsiini",
          kuva: "https://upload.wikimedia.org/wikipedia/commons/c/c4/Orange-Fruit-Pieces.jpg",
        },
        {
          nimi: "Kiivi",
          kuva: "https://upload.wikimedia.org/wikipedia/commons/d/d3/Kiwi_aka.jpg",
        },
      ];

      // Luodaan lista linkkeineen
      hedelmat.forEach((item) => {
        const li = document.createElement("li");
        const a = document.createElement("a");
        a.textContent = item.nimi;
        a.href = "#";
        a.onclick = (e) => {
          e.preventDefault();
          lisaaKuva(item.kuva, item.nimi);
        };
        li.appendChild(a);
        listaElementti.appendChild(li);
      });

      // Funktio, joka lisää yksittäisen kuvan
      function lisaaKuva(url, nimi) {
        const img = document.createElement("img");
        img.src = url;
        img.alt = nimi;
        img.title = nimi;
        kuvatDiv.appendChild(img);
      }

      // Funktio, joka lisää kaikki kuvat
      function lisaaKaikkiKuvat() {
        kuvatDiv.innerHTML = ""; // Tyhjennetään ennen uusien lisäystä
        hedelmat.forEach((item) => lisaaKuva(item.kuva, item.nimi));
      }

      // Nappi: lisää tapahtumakuuntelija
      naytaKaikkiBtn.addEventListener("click", lisaaKaikkiKuvat);
    </script>
  </body>
</html>
````

Selitys

- Lista olioista: jokaisella on nimi ja kuva-kenttä.

- Klikkaus ei avaa uutta sivua (href="#" + e.preventDefault()), vaan kutsuu funktiota.

- Funktio lisaaKuva() luo uuden <img>-elementin ja lisää sen näkyviin sivulle.

- Kuvat pysyvät näkyvissä usean klikkauksen jälkeen — sivu ei nollaannu.

- Nappi tyhjentää kuvatkentän ja lisää kaikki kuvat forEach-silmukalla.

## Tehtävät

- Tee kelikameralistaan jokaiselle linkki, joka avaa uuden sivun (target="\_blank")

- Uusi sivu näyttää tarkemmat tiedot id:n perusteella (api/weathercam/v1/stations/C04507).

- Tutki mistä (uudella sivulla olevasta datasta) löytyy kameran id (oikeastaan lista, jossa kaikkien kameroiden idt).

- Muuta linkkiä siten, että se näyttää kaikki aseman kelikamerat. Kelikameran kuvan saa haettua id:n avulla (https://weathercam.digitraffic.fi/C1451601.jpg?thumbnail=true). Käytä thumpnail-kuvia.

- Lisää pieneen kuvaan linkki, joka avaa kuvan suurempana (uudelle sivulle).
