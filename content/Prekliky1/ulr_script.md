```js
const input = items[0].json;

// Získanie a spracovanie vstupných údajov
const typ = input["Typ"]?.toLowerCase()?.trim();           // "byt" alebo "dom"
const izby = parseInt(input["Izby"]);
const minCena = parseInt(input["Mincena"]);
const maxCena = parseInt(input["Maxcena"]);
const lokalita = input["Lokalita"]?.trim();                // napr. "Banska Bystrica"

// 🧼 Normalizácia reťazca (bez diakritiky, malé písmená)
function normalizeText(text) {
  return text?.toLowerCase()
              .normalize("NFD")
              .replace(/[\u0300-\u036f]/g, "")
              .replace(/[^a-z\s]/g, "")
              .trim();
}

// 🗺️ Mapa miest na TopReality "obec" ID (bez diakritiky)
const obecIdMap = {
  "banska bystrica": 1997,
  "ziar nad hronom": 2792,
  "bratislava": 1000000,
  "zvolen": 2714,
  "kosice": 100000,
  "senec": 104,
  "trencin": 946,
  "trnava": 459,
  "zarnovica": 2747,
  "presov": 3201
};

// 🗺️ Mapa miest na PSČ pre Bazoš
const pscMap = {
  "bratislava": "81101",
  "kosice": "04001",
  "presov": "08001",
  "zilina": "01001",
  "nitra": "94901",
  "banska bystrica": "97401",
  "trnava": "91701",
  "martin": "03601",
  "trencin": "91101",
  "poprad": "05801",
  "prievidza": "97101",
  "zvolen": "96001",
  "michalovce": "07101",
  "nove zamky": "94001",
  "levice": "93401",
  "komarno": "94501",
  "humenné": "06601",
  "piestany": "92101",
  "liptovsky mikulas": "03101",
  "lucenec": "98401",
  "topolcany": "95501",
  "ruzomberok": "03401",
  "cadca": "02201",
  "dunajska streda": "92901",
  "humenne": "06601",
  "ziar nad hronom": "96501",
  "hlinik nad hronom": "96601"
};

// 🔢 Mapovanie typu nehnuteľnosti na typeId pre TopReality
function getTopRealityTypeId(typ, izby) {
  if (typ === "byt") {
    switch (izby) {
      case 1: return 102;
      case 2: return 103;
      case 3: return 104;
      case 4: return 105;
      default: return 106;
    }
  } else {
    return 107;
  }
}

// Normalizovaná lokalita pre mapovanie
const normalizedLokalita = normalizeText(lokalita);
const obecId = obecIdMap[normalizedLokalita] || 0;
const typeId = getTopRealityTypeId(typ, izby);
const psc = pscMap[normalizedLokalita] || lokalita; // fallback na pôvodnú hodnotu ak PSČ nie je nájdené

// 🔧 Pomocná funkcia na vytvorenie "slug" z lokality
function slugifyLokalita(lok) {
  return normalizeText(lok).replace(/\s+/g, "-");
}

// 🔗 Generovanie URL pre portály

function getNehnutelnostiUrl() {
  const typSlug = `${izby}-izbove-${typ === 'byt' ? 'byty' : 'domy'}`;
  const lokSlug = slugifyLokalita(lokalita);
  return `https://www.nehnutelnosti.sk/vysledky/${typSlug}/${lokSlug}/predaj?priceTo=${maxCena}&priceFrom=${minCena}`;
}

function getTopRealityUrl() {
  return `https://www.topreality.sk/vyhladavanie-nehnutelnosti.html?form=1&type%5B%5D=${typeId}&obec=${obecId}&searchType=string&cena_od=${minCena}&cena_do=${maxCena}&vymera_od=0&vymera_do=0&n_search=search&page=estate`;
}

// NOVÁ dynamická generácia Bazoš URL presne podľa zadania
function getBazosUrl() {
  const bazosTyp = typ === "byt" ? "byt" : "dom";
  // Medzery v hledat nahradíme pluskami podľa príkladu (3+izbovy+byt)
  const hledat = `${izby}+izbovy+${bazosTyp}`;
  
  // Vytvorenie URL manuálne, presne podľa príkladu
  return `https://reality.bazos.sk/predam/${bazosTyp}/?hledat=${hledat}&hlokalita=${psc}&humkreis=10&cenaod=${minCena}&cenado=${maxCena}&order=`;
}

function getZoznamRealitUrl() {
  const typSlug = `${izby}-izbovy-${typ}`;
  const lokSlug = slugifyLokalita(lokalita);
  return `https://www.zoznamrealit.sk/reality?ref=qs&q=okres-${lokSlug}|druh-${typSlug}|typ-predaj|cenaod-${minCena}|cenado-${maxCena}`;
}

// 📦 Výstup: všetky URL
const urls = {
  "Nehnutelnosti.sk": getNehnutelnostiUrl(),
  "TopReality.sk": getTopRealityUrl(),
  "Bazos.sk": getBazosUrl(),
  "ZoznamRealit.sk": getZoznamRealitUrl()
};

return [
  {
    json: {
      vstup: input,
      urls
    }
  }
];

```