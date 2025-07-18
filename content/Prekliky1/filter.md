// --- Tvoja pôvodná logika vstupu ---
const inputData = $input.first().json.data.markdown;
let inputText = typeof inputData === 'string' ? inputData : JSON.stringify(inputData || '');

if (!inputText || inputText.length < 100) {
  return [{
    json: {
      error: 'Žiadny alebo príliš krátky text na spracovanie',
      cleanText: '',
      properties: [],
      stats: { total: 0, available: 0, filtered: 0 }
    }
  }];
}

// --- Rozšírené regexy na nové informácie ---
const extraPatterns = {
  elevator: /(?:s\s+)?výťah(?:om)?|bez\s+výťahu/gi,
  condition: /pôvodný stav|novostavba|kompletná rekonštrukcia|čiastočná rekonštrukcia|zrekonštruovaný|developerský projekt/gi,
  balcony: /balkón|lodžia|terasa/gi,
  parking: /parkovanie|garáž(ové)?\s+státie|miesto na parkovanie/gi,
  furnished: /zariadený|nezariadený/gi,
  floor: /(?:na\s+)?(\d{1,2})(?:\.|.)?\s*poschodí?/i,
  totalFloors: /z\s+(\d{1,2})\s+poschodiach?/i
};

// --- Zvyšok pôvodného kódu (iba rozšírené property) ---
function extractAvailableProperties(text) {
  const patterns = {
    propertyBlocks: /## ([^#\n]+)([\s\S]*?)(?=## |$)/g,
    isReservedOrSold: /REZERVOVAN[ÉÁ]|PREDAN[ÉÁ]|SOLD|RESERVED/gi,
    titleReservedOrSold: /^##\s+.*?(REZERVOVAN[ÉÁ]|PREDAN[ÉÁ]|SOLD|RESERVED)/i,
    price: /(\d+\s+\d+|\d+)\s+€/g,
    pricePerM2: /(\d+\s+\d+|\d+)[,.](\d+)\s+€\/m²/g,
    area: /(\d+\.?\d*)\s+m²/g,
    roomType: /(\d+)\s+izbov[ýá]\s+(byt|dom)/gi,
    location: /([A-ZÁČĎÉÍĹĽŇÓŔŠŤÚÝŽ][a-záčďéíĺľňóŕšťúýž\s,.-]+),\s*okres/g,
    address: /^([A-ZÁČĎÉÍĹĽŇÓŔŠŤÚÝŽ][a-záčďéíĺľňóŕšťúýž\s]+\s+\d+)/m,
    nehnutelnostiLinks: /https:\/\/www\.nehnutelnosti\.sk\/detail\/[A-Za-z0-9_-]+\/[A-Za-z0-9_-]+/g,
    allNehnutelnostiLinks: /https:\/\/www\.nehnutelnosti\.sk\/(?!_next\/static|.*\.(?:jpg|jpeg|png|gif|svg|css|js))[^\s\)]+/g,
    phone: /(\+421\s?)?(\d{3})\s?(\d{3})\s?(\d{3})/g,
    email: /[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}/g,
    agency: /^([A-ZÁČĎÉÍĹĽŇÓŔŠŤÚÝŽ][A-ZÁČĎÉÍĹĽŇÓŔŠŤÚÝŽ\s&.]+\s+(s\.r\.o\.|a\.s\.|spol\.|reality|Real|REAL))$/m
  };

  const matches = Array.from(text.matchAll(patterns.propertyBlocks));
  const availableProperties = [];
  let filteredCount = 0;

  matches.forEach((match) => {
    const title = match[1].trim();
    const content = match[2].trim();
    const fullBlock = match[0];

    if (patterns.titleReservedOrSold.test(title) || patterns.isReservedOrSold.test(content)) {
      filteredCount++;
      return;
    }

    const cleanContent = content
      .replace(/\n\s*\n/g, '\n')
      .replace(/^\s+|\s+$/g, '')
      .replace(/\\\\/g, '')
      .replace(/\[[^\]]*\]\([^)]*\)/g, '')
      .replace(/\[([^\]]*)\]/g, '$1')
      .replace(/!\[[^\]]*\]\([^)]*\)/g, '')
      .replace(/https?:\/\/[^\s]+/g, '')
      .split('\n')
      .filter((line, i, arr) => {
        const t = line.trim();
        return t.length > 5 && !t.match(/^[^\w]*$/) &&
          !t.match(/^[\d\s€.,²]+$/) &&
          arr.findIndex(l => l.trim() === t) === i;
      })
      .join('\n');

    const get = (pattern) => {
      const m = fullBlock.match(pattern);
      return m ? m[0] : null;
    };

    const getFirstMatchGroup = (pattern, groupIndex = 1) => {
      const m = fullBlock.match(pattern);
      return m && m[groupIndex] ? m[groupIndex] : null;
    };

    const property = {
      id: availableProperties.length + 1,
      title,
      content: cleanContent,
      price: get(patterns.price),
      pricePerM2: get(patterns.pricePerM2),
      area: get(patterns.area),
      roomType: get(patterns.roomType),
      location: getFirstMatchGroup(patterns.location),
      address: get(patterns.address),
      links: get(patterns.nehnutelnostiLinks) ? [get(patterns.nehnutelnostiLinks)] : [],
      allLinks: [...new Set(fullBlock.match(patterns.allNehnutelnostiLinks) || [])],
      phone: get(patterns.phone),
      email: get(patterns.email),
      agency: get(patterns.agency),
      fullText: fullBlock,
      // ✅ Nové polia navyše:
      condition: get(extraPatterns.condition),
      elevator: get(extraPatterns.elevator),
      balcony: get(extraPatterns.balcony),
      parking: get(extraPatterns.parking),
      furnished: get(extraPatterns.furnished),
      floor: getFirstMatchGroup(extraPatterns.floor),
      totalFloors: getFirstMatchGroup(extraPatterns.totalFloors)
    };

    availableProperties.push(property);
  });

  return {
    properties: availableProperties,
    stats: {
      total: matches.length,
      available: availableProperties.length,
      filtered: filteredCount
    }
  };
}

function formatCleanOutput(properties) {
  return properties.map(prop => {
    const cena = prop.price ? prop.price.replace(/\s+/g, '') : '';
    const cenaZaM2 = prop.pricePerM2 ? prop.pricePerM2.replace(/\s+/g, '') : '';
    const poschodie = prop.floor && prop.totalFloors ? `${prop.floor} / ${prop.totalFloors}` : prop.floor || '';

    let popis = '';
    if (prop.content) {
      const firstLine = prop.content.split('\n')[0];
      const snippet = prop.content.slice(0, 300).split('\n').join(' ');
      popis = snippet.length > 20 ? snippet.trim().replace(/\s+/g, ' ') : firstLine.trim();
    }

    let output = `## ${prop.title}\n\n`;

    if (prop.location) output += `📍 ${prop.location}\n`;
    if (prop.area) output += `📐 ${prop.area}\n`;
    if (cena) output += `💰 ${cena} €`;
    if (cena && cenaZaM2) output += ` (${cenaZaM2} €/m²)\n`;
    else if (cena) output += `\n`;
    if (prop.condition) output += `🏗️ Stav: ${prop.condition}\n`;
    if (prop.elevator) output += `🛗 Výťah: áno\n`;
    if (prop.balcony) output += `🌇 Balkón: áno\n`;
    if (prop.parking) output += `🚗 Parkovanie: ${prop.parking}\n`;
    if (prop.furnished) output += `🛋️ Zariadenie: ${prop.furnished}\n`;
    if (poschodie) output += `📶 Poschodie: ${poschodie}\n`;

    if (popis) {
      output += `\n📝 **Stručný popis:**\n${popis}\n`;
    }

    if (prop.links && prop.links.length > 0) {
      output += `\n🔗 [Zobraziť inzerát](${prop.links[0]})\n`;
    }

    return output.trim();
  }).join('\n\n');
}

const result = extractAvailableProperties(inputText);
const cleanOutput = formatCleanOutput(result.properties);

const stats = {
  ...result.stats,
  withPrice: result.properties.filter(p => p.price).length,
  withContact: result.properties.filter(p => p.phone || p.email).length,
  withLinks: result.properties.filter(p => p.links && p.links.length > 0).length,
  totalLinks: result.properties.reduce((sum, p) => sum + (p.links ? p.links.length : 0), 0),
  totalAllLinks: result.properties.reduce((sum, p) => sum + (p.allLinks ? p.allLinks.length : 0), 0)
};

return [{
  json: {
    cleanText: cleanOutput,
    properties: result.properties,
    stats: stats,
    success: true,
    processedAt: new Date().toISOString()
  }
}];
