```js
return items.map(item => {
  // Skúsime viacero polí na nájdenie textu s Google Drive linkom
  const possibleFields = [
    item.json.description,
    item.json.poznamky, 
    item.json.mp3Url,
    item.json.summary,
    item.json.text
  ];
  
  let raw = "";
  
  // Nájdeme prvé pole, ktoré obsahuje text
  for (let i = 0; i < possibleFields.length; i++) {
    if (possibleFields[i] && possibleFields[i].toString().trim()) {
      raw = possibleFields[i].toString();
      break;
    }
  }
  
  if (!raw) {
    return {
      json: {
        ...item.json,
        error: "No text found in any field"
      }
    };
  }
  
  // Vyčistenie HTML
  let cleanedText = raw
    .replace(/<[^>]*>/g, ' ')
    .replace(/&nbsp;/g, ' ')
    .replace(/&amp;/g, '&')
    .replace(/&lt;/g, '<')
    .replace(/&gt;/g, '>')
    .replace(/&quot;/g, '"')
    .replace(/&#39;/g, "'")
    .replace(/\s+/g, ' ')
    .trim();
  
  // Hľadanie Google Drive linkov
  const patterns = [
    /https:\/\/drive\.google\.com\/file\/d\/([a-zA-Z0-9_-]{25,})/g,
    /https:\/\/drive\.google\.com\/open\?id=([a-zA-Z0-9_-]{25,})/g,
    /drive\.google\.com\/file\/d\/([a-zA-Z0-9_-]{25,})/g,
    /drive\.google\.com\/open\?id=([a-zA-Z0-9_-]{25,})/g
  ];
  
  let fileId = null;
  
  for (let pattern of patterns) {
    const match = cleanedText.match(pattern);
    if (match) {
      const idMatch = match[0].match(/([a-zA-Z0-9_-]{25,})/);
      if (idMatch) {
        fileId = idMatch[0].split(/[?&#]/)[0];
        break;
      }
    }
  }
  
  if (fileId) {
    return {
      json: {
        ...item.json,
        cleanedLink: `https://drive.google.com/file/d/${fileId}/view`
      }
    };
  }
  
  // Fallback - hľadáme iba ID v kontexte
  const idPattern = /[a-zA-Z0-9_-]{25,}/g;
  const possibleIds = cleanedText.match(idPattern);
  
  if (possibleIds) {
    for (const id of possibleIds) {
      const textAroundId = cleanedText.toLowerCase();
      const idIndex = textAroundId.indexOf(id.toLowerCase());
      
      if (idIndex > -1) {
        const contextBefore = textAroundId.substring(Math.max(0, idIndex - 50), idIndex);
        const contextAfter = textAroundId.substring(idIndex, Math.min(textAroundId.length, idIndex + 50));
        const fullContext = contextBefore + contextAfter;
        
        if (fullContext.includes('drive') || fullContext.includes('google')) {
          return {
            json: {
              ...item.json,
              cleanedLink: `https://drive.google.com/file/d/${id}/view`
            }
          };
        }
      }
    }
  }
  
  // Ak nič nenašlo
  return {
    json: {
      ...item.json,
      error: "Google Drive link not found"
    }
  };
});
```