# Johdanto: Mikä on API ja mitä tehdään

## Teoria

### Mikä on API?

- **API (Application Programming Interface)** on rajapinta, jonka avulla eri ohjelmistot voivat kommunikoida keskenään.
- REST API on yleinen tyyppi: se käyttää HTTP-pyyntöjä ja palauttaa yleensä dataa **JSON-muodossa**.
- Esimerkiksi kelikameratiedot Digitrafficin API:sta:  
  `https://tie.digitraffic.fi/api/weathercam/v1/stations`

### JSON

- JSON (JavaScript Object Notation) on kevyt tapa esittää tietoa rakenteisessa muodossa.
- JSON koostuu avain-arvo -pareista.
- Esimerkki yhdestä asemasta API:n vastauksessa:

```json
{
  "type": "Feature",
  "geometry": {
    "type": "Point",
    "coordinates": [24.93545, 60.16952]
  },
  "properties": {
    "id": "kam001",
    "name": "Helsinki, Mannerheimintie",
    "presets": [
      {
        "id": "preset1",
        "presentationName": "Eteläinen suunta"
      }
    ]
  }
}
```

## Esimerkki 1: Datan hakeminen .then-rakenteella

```javascript
const API_BASE = "https://tie.digitraffic.fi/api/weathercam/v1/stations";
fetch(API_BASE)
  .then((response) => response.json()) // muunnetaan JSON-objektiksi
  .then((data) => {
    console.log("Kelikameradata:", data);
  })
  .catch((error) => {
    console.error("Virhe haettaessa dataa:", error);
  });
```

Selitys:

- fetch() palauttaa lupauksen (Promise), joka ratkaistaan, kun HTTP-pyyntö onnistuu.

- .then() -funktiot käsittelevät tuloksen askel askeleelta.

- .catch() käsittelee virheet.

## Esimerkki 2: Datan hakeminen async/await-rakenteella

```javascript
async function haeKamerat() {
  const API_BASE = "https://tie.digitraffic.fi/api/weathercam/v1/stations";

  try {
    const response = await fetch(API_BASE);
    const data = await response.json();
    console.log("Kelikameradata (async/await):", data);
  } catch (error) {
    console.error("Virhe haettaessa dataa:", error);
  }
}

haeKamerat();
```

Selitys:

- async merkitsee, että funktio palauttaa lupauksen (Promise).

- await pysäyttää funktion suorittamisen, kunnes fetch tai JSON-muunnos on valmis.

- try/catch käsittelee mahdolliset virheet.

## Tehtävät

- Avaa selaimen konsoli (F12 → Console).

- Kopioi ja suorita esimerkkikoodit.

- Tarkista, mitä dataa saat näkyviin.

- Kokeile hakea vain ensimmäisen aseman nimi seuraavasti:

.then-rakenteella:

```js
console.log(data.features[0].properties.name);
```

async/await-rakenteella:

```js
console.log(data.features[0].properties.name);
```

- Kokeile hakea kaikki asemat lista-muodossa:

```js
data.features;
```

- Lisävinkki: Mieti, mitä muita tietoja asemasta voisi hyödyntää sovelluksessa (esim. koordinaatit, preset-kuvat).
