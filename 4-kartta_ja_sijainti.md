# Kartta ja sijainti (Leaflet)

## 1. Ulkoisen kirjaston lisääminen (CDN)

CDN eli _Content Delivery Network_ on tapa tuoda valmiita kirjastoja projektiin ilman paikallista asennusta.

_Leaflet_ on suosittu avoimen lähdekoodin JavaScript-kirjasto, jolla voi näyttää karttoja helposti.

Lisätään HTML-tiedoston `<head>`-osaan seuraavat rivit:

```html
<!-- Leaflet CSS -->
<link
  rel="stylesheet"
  href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css"
/>

<!-- Leaflet JS -->
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
```

Tämän jälkeen Leafletin funktiot (L.map, L.marker, L.tileLayer jne.) ovat käytettävissä.

## 2. Leafletin perusteet

**Kartta luodaan näin:**

```js
const map = L.map("map").setView([60.17, 24.94], 10);
```

- "map" on HTML-elementin id.

- [lat, lon] on aloitussijainti.

- 10 on zoom-taso.

**Taustakartta lisätään tileLayerilla:**

```js
L.tileLayer("https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png", {
  maxZoom: 19,
  attribution: "© OpenStreetMap",
}).addTo(map);
```

**Markkerin (osoitin) lisääminen:**

```js
L.marker([60.17, 24.94]).addTo(map).bindPopup("Helsinki").openPopup();
```

**Kartan liikuttaminen sijaintiin (animoidusti):**

```js
map.flyTo([62.2415, 25.7209], 12); // vie kartan Jyväskylään
```

**Kartan tilan hallinta**

Leaflet-kartta kannattaa luoda vain kerran.
Kun käyttäjä valitsee uuden sijainnin, päivitetään sama kartta:

- Poistetaan tai päivitetään vanha markkeri

- Siirretään kartta uuteen kohtaan (flyTo).

## Esimerkki: Kaupungit kartalla

```html
<!DOCTYPE html>
<html lang="fi">
  <head>
    <meta charset="UTF-8" />
    <title>Kartta ja sijainti</title>

    <!-- Leaflet-kirjaston linkit -->
    <link
      rel="stylesheet"
      href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css"
    />
    <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>

    <style>
      body {
        font-family: Arial, sans-serif;
        margin: 2rem;
      }
      #map {
        height: 400px;
        width: 100%;
        border: 2px solid #ccc;
        border-radius: 8px;
        margin-top: 1rem;
      }
      ul li {
        margin-bottom: 5px;
      }
      a {
        color: #0078d4;
        cursor: pointer;
      }
      a:hover {
        text-decoration: underline;
      }
    </style>
  </head>
  <body>
    <h1>Kaupungit kartalla</h1>
    <p>Valitse kaupunki listasta nähdäksesi sen sijainnin kartalla.</p>

    <ul id="lista"></ul>
    <div id="map"></div>

    <script>
      // Lista olioista (esim. kamerat, kaupungit tms.)
      const paikat = [
        { nimi: "Helsinki", lat: 60.1699, lon: 24.9384 },
        { nimi: "Tampere", lat: 61.4978, lon: 23.761 },
        { nimi: "Turku", lat: 60.4518, lon: 22.2666 },
        { nimi: "Oulu", lat: 65.0121, lon: 25.4651 },
        { nimi: "Raahe", lat: 64.6886, lon: 24.4799 },
      ];

      const lista = document.getElementById("lista");

      // Luodaan Leaflet-kartta
      const map = L.map("map").setView([62.0, 25.0], 6);

      L.tileLayer("https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png", {
        attribution: "© OpenStreetMap contributors",
      }).addTo(map);

      let markkeri = null;

      // Luodaan lista linkkeineen
      paikat.forEach((p) => {
        const li = document.createElement("li");
        const linkki = document.createElement("a");
        linkki.textContent = p.nimi;
        linkki.href = "#";
        linkki.onclick = (e) => {
          e.preventDefault();
          naytaSijainti(p);
        };
        li.appendChild(linkki);
        lista.appendChild(li);
      });

      // Funktio, joka päivittää kartan valitun sijainnin mukaan
      function naytaSijainti(paikka) {
        // Poistetaan vanha markkeri, jos sellainen on
        if (markkeri) {
          map.removeLayer(markkeri);
        }

        // Lisätään uusi markkeri
        markkeri = L.marker([paikka.lat, paikka.lon])
          .addTo(map)
          .bindPopup(`<b>${paikka.nimi}</b>`)
          .openPopup();

        // Siirretään kartta uuteen sijaintiin
        map.flyTo([paikka.lat, paikka.lon], 10);
      }
    </script>
  </body>
</html>
```

## Tehtävät

- Lisää Leaflet-kartta (ja siihen liittyvä css) kelikamerat-sovellukseesi

- Lisää `naytaSijainti(paikka)`-funktio. Sitä kutsuttaessa argumentteina välitetään aseman nimi ja sijaintikoordinaatit. Nimen ja koordinaatit saa fetch-kutsulla.

- Voit myös muokata funktion parametreja. Kartan keskittämiseen tarvitaan vain leveysaste (latitude) ja pituusaste(longitude).
