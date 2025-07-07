```JSON
{

  "name": "Skuska skusenosti",

  "nodes": [

    {

      "parameters": {

        "operation": "getAll",

        "calendar": {

          "__rl": true,

          "value": "696c4d0d6812d9b7e15680eff162b0410a385c0c1d49df27e2cbad6a8915f335@group.calendar.google.com",

          "mode": "list",

          "cachedResultName": "PEDRO APP"

        },

        "returnAll": true,

        "timeMin": "={{ $now.plus({ days: 1 }).startOf('day') }}",

        "timeMax": "={{ $now.plus({ days: 1 }).endOf('day') }}",

        "options": {}

      },

      "type": "n8n-nodes-base.googleCalendar",

      "typeVersion": 1.3,

      "position": [

        -1240,

        980

      ],

      "id": "cbef9c95-cc4a-41dd-bf3b-5b5478221224",

      "name": "GET EVENTS",

      "credentials": {

        "googleCalendarOAuth2Api": {

          "id": "ZARvA52c0ZAFLqQ7",

          "name": "Google Calendar account"

        }

      }

    },

    {

      "parameters": {},

      "type": "@n8n/n8n-nodes-langchain.toolThink",

      "typeVersion": 1,

      "position": [

        760,

        1100

      ],

      "id": "92457ab9-e516-42fb-ad23-b4a99a2fa9c0",

      "name": "Think"

    },

    {

      "parameters": {

        "errorMessage": "=Chyba mp3 subor"

      },

      "id": "dd7a3d93-9f37-469b-9377-ff23be0f0507",

      "name": "Error Handler1",

      "type": "n8n-nodes-base.stopAndError",

      "typeVersion": 1,

      "position": [

        -800,

        1260

      ]

    },

    {

      "parameters": {

        "modelId": {

          "__rl": true,

          "value": "gpt-4o",

          "mode": "list",

          "cachedResultName": "GPT-4O"

        },

        "messages": {

          "values": [

            {

              "content": "=Si asistent obchodníka. Na základe nasledujúceho textu z hovoru vytvor stručné zhrnutie pre obchodníka, ktoré mu pomôže lepšie pochopiť osobnosť klienta a ako k nemu pristupovať.\n\nVýstup rozdeľ do dvoch častí:\n\n1. Charakteristika osobnosti – popíš hlavné črty osobnosti klienta, napríklad či je viac analytický, pragmatický, priateľský, rýchly v rozhodovaní, otvorený novinkám a pod.\n\n2. Hlavné potreby, plány a možné námietky – zhrni, čo klient očakáva, na čo si dáva pozor, čo ho môže brzdiť alebo odradiť a nejake navrhy pre obchodnika, co by mu mohol poskytnut \n\n---\n\nText hovoru:  \n{{ $json.text }}",

              "role": "system"

            }

          ]

        },

        "options": {}

      },

      "type": "@n8n/n8n-nodes-langchain.openAi",

      "typeVersion": 1.8,

      "position": [

        180,

        860

      ],

      "id": "b3c47163-db90-4585-ac7c-7a5538b3aeec",

      "name": "ANALYZA OSOBNOSTI A NAVRHY",

      "credentials": {

        "openAiApi": {

          "id": "n2yZc1G5Qst6SEWp",

          "name": "skuska skusenosti"

        }

      }

    },

    {

      "parameters": {

        "jsCode": "return items.map(item => {\n  // Skúsime viacero polí na nájdenie textu s Google Drive linkom\n  const possibleFields = [\n    item.json.description,\n    item.json.poznamky, \n    item.json.mp3Url,\n    item.json.summary,\n    item.json.text\n  ];\n  \n  let raw = \"\";\n  \n  // Nájdeme prvé pole, ktoré obsahuje text\n  for (let i = 0; i < possibleFields.length; i++) {\n    if (possibleFields[i] && possibleFields[i].toString().trim()) {\n      raw = possibleFields[i].toString();\n      break;\n    }\n  }\n  \n  if (!raw) {\n    return {\n      json: {\n        ...item.json,\n        error: \"No text found in any field\"\n      }\n    };\n  }\n  \n  // Vyčistenie HTML\n  let cleanedText = raw\n    .replace(/<[^>]*>/g, ' ')\n    .replace(/&nbsp;/g, ' ')\n    .replace(/&amp;/g, '&')\n    .replace(/&lt;/g, '<')\n    .replace(/&gt;/g, '>')\n    .replace(/&quot;/g, '\"')\n    .replace(/&#39;/g, \"'\")\n    .replace(/\\s+/g, ' ')\n    .trim();\n  \n  // Hľadanie Google Drive linkov\n  const patterns = [\n    /https:\\/\\/drive\\.google\\.com\\/file\\/d\\/([a-zA-Z0-9_-]{25,})/g,\n    /https:\\/\\/drive\\.google\\.com\\/open\\?id=([a-zA-Z0-9_-]{25,})/g,\n    /drive\\.google\\.com\\/file\\/d\\/([a-zA-Z0-9_-]{25,})/g,\n    /drive\\.google\\.com\\/open\\?id=([a-zA-Z0-9_-]{25,})/g\n  ];\n  \n  let fileId = null;\n  \n  for (let pattern of patterns) {\n    const match = cleanedText.match(pattern);\n    if (match) {\n      const idMatch = match[0].match(/([a-zA-Z0-9_-]{25,})/);\n      if (idMatch) {\n        fileId = idMatch[0].split(/[?&#]/)[0];\n        break;\n      }\n    }\n  }\n  \n  if (fileId) {\n    return {\n      json: {\n        ...item.json,\n        cleanedLink: `https://drive.google.com/file/d/${fileId}/view`\n      }\n    };\n  }\n  \n  // Fallback - hľadáme iba ID v kontexte\n  const idPattern = /[a-zA-Z0-9_-]{25,}/g;\n  const possibleIds = cleanedText.match(idPattern);\n  \n  if (possibleIds) {\n    for (const id of possibleIds) {\n      const textAroundId = cleanedText.toLowerCase();\n      const idIndex = textAroundId.indexOf(id.toLowerCase());\n      \n      if (idIndex > -1) {\n        const contextBefore = textAroundId.substring(Math.max(0, idIndex - 50), idIndex);\n        const contextAfter = textAroundId.substring(idIndex, Math.min(textAroundId.length, idIndex + 50));\n        const fullContext = contextBefore + contextAfter;\n        \n        if (fullContext.includes('drive') || fullContext.includes('google')) {\n          return {\n            json: {\n              ...item.json,\n              cleanedLink: `https://drive.google.com/file/d/${id}/view`\n            }\n          };\n        }\n      }\n    }\n  }\n  \n  // Ak nič nenašlo\n  return {\n    json: {\n      ...item.json,\n      error: \"Google Drive link not found\"\n    }\n  };\n});"

      },

      "type": "n8n-nodes-base.code",

      "typeVersion": 2,

      "position": [

        -380,

        960

      ],

      "id": "536905c9-c48d-42f0-ab41-4b9fc8e693a0",

      "name": "Code"

    },

    {

      "parameters": {},

      "type": "n8n-nodes-base.noOp",

      "typeVersion": 1,

      "position": [

        -600,

        760

      ],

      "id": "bbaffb97-8793-48f3-ae16-12238bda7599",

      "name": "No Operation, do nothing"

    },

    {

      "parameters": {

        "conditions": {

          "options": {

            "caseSensitive": true,

            "leftValue": "",

            "typeValidation": "strict",

            "version": 1

          },

          "conditions": [

            {

              "id": "c1d2e3f4-a5b6-7890-cdef-123456789abc",

              "leftValue": "={{ $json.description }}",

              "rightValue": "drive.google",

              "operator": {

                "type": "string",

                "operation": "contains"

              }

            }

          ],

          "combinator": "and"

        },

        "options": {}

      },

      "id": "7a7ee789-8b9f-4bd7-94f1-e9f99bc0a5b0",

      "name": "FILTER MP3",

      "type": "n8n-nodes-base.if",

      "typeVersion": 2,

      "position": [

        -820,

        980

      ],

      "onError": "continueRegularOutput"

    },

    {

      "parameters": {

        "assignments": {

          "assignments": [

            {

              "id": "d3e4f5a6-b7c8-9012-def3-456789012345",

              "name": "firmaNazov",

              "value": "={{ $json.summary }}",

              "type": "string"

            },

            {

              "id": "e4f5a6b7-c8d9-0123-ef45-6789012345ab",

              "name": "ico",

              "value": "={{ $json.description.replace(/<[^>]*>/g, '').match(/IČO[:\\s]*([0-9]{8})/i)?.[1] || '' }}",

              "type": "string"

            },

            {

              "id": "f5a6b7c8-d9e0-1234-fa56-789012345abc",

              "name": "=datumHovoru",

              "value": "={{ $json.start.dateTime || $json.start.date }}",

              "type": "string"

            },

            {

              "id": "a6b7c8d9-e0f1-2345-ab67-89012345abcd",

              "name": "poznamky",

              "value": "={{ $json.description }}",

              "type": "string"

            },

            {

              "id": "b7c8d9e0-f1a2-3456-bc78-9012345abcde",

              "name": "mp3Url",

              "value": "={{ $json.description.replace(/<[^>]*>/g, ' ').match(/https:\\/\\/drive\\.google\\.com\\/(?:file\\/d\\/|open\\?id=)([a-zA-Z0-9_-]{25,})/)?.[1] || '' }}",

              "type": "string"

            },

            {

              "id": "9d47e9c0-f8b4-4855-86bd-a7e0b4918660",

              "name": "eventid",

              "value": "={{ $('GET EVENTS').item.json.id }}",

              "type": "string"

            }

          ]

        },

        "options": {}

      },

      "id": "06e2958f-41d8-48fd-b910-297ddf07f602",

      "name": "EXTRACT DATA",

      "type": "n8n-nodes-base.set",

      "typeVersion": 3.3,

      "position": [

        -620,

        960

      ]

    },

    {

      "parameters": {

        "rule": {

          "interval": [

            {

              "triggerAtHour": 6

            }

          ]

        }

      },

      "type": "n8n-nodes-base.scheduleTrigger",

      "typeVersion": 1.2,

      "position": [

        -1440,

        980

      ],

      "id": "f7fe9dba-a203-4767-a843-e5331078314b",

      "name": "DAILY TRIGGER"

    },

    {

      "parameters": {

        "sendTo": "petercabanik154@gmail.com",

        "subject": "HOTOVO",

        "message": "Vsetky udalosti z kalendara boli rozdelene a vystupy vytvorene",

        "options": {

          "appendAttribution": false

        }

      },

      "type": "n8n-nodes-base.gmail",

      "typeVersion": 2.1,

      "position": [

        -780,

        760

      ],

      "id": "1c17d298-2cae-4482-b9bd-30284ffce9c1",

      "name": "HOTOVO",

      "webhookId": "5ee979a3-b5be-44a4-b6c1-8d80363083af",

      "credentials": {

        "gmailOAuth2": {

          "id": "CJHWscTS2Wljji9Q",

          "name": "Gmail account"

        }

      }

    },

    {

      "parameters": {

        "operation": "download",

        "fileId": {

          "__rl": true,

          "value": "={{ $json.cleanedLink }}",

          "mode": "url"

        },

        "options": {}

      },

      "type": "n8n-nodes-base.googleDrive",

      "typeVersion": 3,

      "position": [

        -160,

        860

      ],

      "id": "edec1318-d65d-447b-82bf-6370db28d5bb",

      "name": "DOWNLOAD MP3",

      "credentials": {

        "googleDriveOAuth2Api": {

          "id": "zfrbbJJguyCdWGLF",

          "name": "Google Drive account"

        }

      }

    },

    {

      "parameters": {

        "resource": "audio",

        "operation": "transcribe",

        "options": {}

      },

      "type": "@n8n/n8n-nodes-langchain.openAi",

      "typeVersion": 1.8,

      "position": [

        20,

        860

      ],

      "id": "dbf4c2aa-637b-461e-93e2-a0d03f525b30",

      "name": "PREPIS MP3",

      "credentials": {

        "openAiApi": {

          "id": "n2yZc1G5Qst6SEWp",

          "name": "skuska skusenosti"

        }

      }

    },

    {

      "parameters": {

        "method": "POST",

        "url": "https://api.firecrawl.dev/v1/scrape",

        "sendHeaders": true,

        "headerParameters": {

          "parameters": [

            {

              "name": "Authorization",

              "value": "Bearer fc-3bec82e791464cb7a635d046e2708ec6"

            }

          ]

        },

        "sendBody": true,

        "specifyBody": "json",

        "jsonBody": "={\n  \"url\": \"https://www.finstat.sk/{{ $('EXTRACT DATA').item.json.ico }}\",\n  \"formats\": [\n    \"markdown\"\n  ],\n  \"onlyMainContent\": true,\n  \"parsePDF\": true,\n  \"maxAge\": 14400000\n}",

        "options": {}

      },

      "type": "n8n-nodes-base.httpRequest",

      "typeVersion": 4.2,

      "position": [

        -100,

        1080

      ],

      "id": "74fc2efc-aa59-43c5-a813-6df3067310f6",

      "name": "WEB SCRAPE"

    },

    {

      "parameters": {

        "assignments": {

          "assignments": [

            {

              "id": "45ec527c-153e-4171-a34b-0db2cab054df",

              "name": "info",

              "value": "={{ $node[\"COMPANY ANALYSIS\"].json.choices[0].message.content }}",

              "type": "string"

            }

          ]

        },

        "options": {}

      },

      "type": "n8n-nodes-base.set",

      "typeVersion": 3.4,

      "position": [

        560,

        1080

      ],

      "id": "3b028f26-a3e2-490d-8787-9f70a00ddb9c",

      "name": "SET INFO"

    },

    {

      "parameters": {

        "assignments": {

          "assignments": [

            {

              "id": "6faa34b0-df02-481e-b02d-695eb7c90e70",

              "name": "content",

              "value": "={{ $json.message.content }}",

              "type": "string"

            }

          ]

        },

        "options": {}

      },

      "type": "n8n-nodes-base.set",

      "typeVersion": 3.4,

      "position": [

        500,

        860

      ],

      "id": "2100cb13-cf83-4d1e-9d82-611c9dbc44bb",

      "name": "SET CONTENT"

    },

    {

      "parameters": {

        "operation": "appendOrUpdate",

        "documentId": {

          "__rl": true,

          "value": "1crjXnMZbuPPQ-zymIsOFDmxMBTpr2y0C3SSgda0p0uA",

          "mode": "list",

          "cachedResultName": "Calendar Summary",

          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/1crjXnMZbuPPQ-zymIsOFDmxMBTpr2y0C3SSgda0p0uA/edit?usp=drivesdk"

        },

        "sheetName": {

          "__rl": true,

          "value": "gid=0",

          "mode": "list",

          "cachedResultName": "Hárok1",

          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/1crjXnMZbuPPQ-zymIsOFDmxMBTpr2y0C3SSgda0p0uA/edit#gid=0"

        },

        "columns": {

          "mappingMode": "defineBelow",

          "value": {

            "NÁZOV": "={{ $('EXTRACT DATA').item.json.firmaNazov }}",

            "IČO": "={{ $('EXTRACT DATA').item.json.ico }}",

            "EVENT ID": "={{ $('EXTRACT DATA').item.json.eventid }}",

            "SUMMARY": "={{ $json.message.content }}"

          },

          "matchingColumns": [

            "EVENT ID"

          ],

          "schema": [

            {

              "id": "NÁZOV",

              "displayName": "NÁZOV",

              "required": false,

              "defaultMatch": false,

              "display": true,

              "type": "string",

              "canBeUsedToMatch": true

            },

            {

              "id": "IČO",

              "displayName": "IČO",

              "required": false,

              "defaultMatch": false,

              "display": true,

              "type": "string",

              "canBeUsedToMatch": true

            },

            {

              "id": "EVENT ID",

              "displayName": "EVENT ID",

              "required": false,

              "defaultMatch": false,

              "display": true,

              "type": "string",

              "canBeUsedToMatch": true,

              "removed": false

            },

            {

              "id": "SUMMARY",

              "displayName": "SUMMARY",

              "required": false,

              "defaultMatch": false,

              "display": true,

              "type": "string",

              "canBeUsedToMatch": true

            }

          ],

          "attemptToConvertTypes": false,

          "convertFieldsToString": false

        },

        "options": {}

      },

      "id": "0d2c3c85-a2ee-4953-a845-0a44c2903cef",

      "name": "WRITE TO DATABASE",

      "type": "n8n-nodes-base.googleSheets",

      "typeVersion": 4.4,

      "position": [

        1060,

        940

      ],

      "credentials": {

        "googleSheetsOAuth2Api": {

          "id": "CAvXNNgdygk31AiM",

          "name": "Google Sheets account"

        }

      }

    },

    {

      "parameters": {

        "authentication": "oAuth2",

        "select": "channel",

        "channelId": {

          "__rl": true,

          "value": "C094XAXDZMX",

          "mode": "list",

          "cachedResultName": "obchodnik"

        },

        "text": "=-----------------------------------------\n\n{{ $json['NÁZOV'] }}\n\n{{ $json.SUMMARY }}\n\n-----------------------------------------",

        "otherOptions": {}

      },

      "type": "n8n-nodes-base.slack",

      "typeVersion": 2.3,

      "position": [

        1240,

        940

      ],

      "id": "a6769c1f-684e-4410-803e-16aea9cdd533",

      "name": "SEND A MESSAGE",

      "webhookId": "316c226d-cc6d-42e1-ba47-d72f6522b5f6",

      "credentials": {

        "slackOAuth2Api": {

          "id": "yPRg4DzNUIpZYc3E",

          "name": "Slack account"

        }

      }

    },

    {

      "parameters": {

        "options": {}

      },

      "id": "64d27f28-1482-4856-ab0c-5851c26cf13f",

      "name": "LOOP THROUGH EVENTS",

      "type": "n8n-nodes-base.splitInBatches",

      "typeVersion": 3,

      "position": [

        -1060,

        980

      ]

    },

    {

      "parameters": {

        "sendTo": "theerror64@gmail.com",

        "subject": "Dnesne Stretnutia",

        "emailType": "text",

        "message": "=-----------------------------------------\n{{ $('EXTRACT DATA').item.json.firmaNazov }}\n\n{{ $('BRIEFING CREATION').item.json.message.content }}\n\n-----------------------------------------",

        "options": {

          "appendAttribution": false

        }

      },

      "type": "n8n-nodes-base.gmail",

      "typeVersion": 2.1,

      "position": [

        1400,

        940

      ],

      "id": "6052a401-703a-4e7a-8e77-344afe9cadaa",

      "name": "SEND BRIEFING",

      "webhookId": "faec3357-c0f3-405b-b2a5-da1441b01e2d",

      "credentials": {

        "gmailOAuth2": {

          "id": "CJHWscTS2Wljji9Q",

          "name": "Gmail account"

        }

      }

    },

    {

      "parameters": {

        "modelId": {

          "__rl": true,

          "value": "gpt-4.1-mini",

          "mode": "list",

          "cachedResultName": "GPT-4.1-MINI"

        },

        "messages": {

          "values": [

            {

              "content": "=Udaje o firme(md format): {{ $json.data.markdown }}\nNa základe poskytnutých údajov o firme mi vygeneruj obchodný briefing pre obchodníka vo formáte nižšie. Použi jasný, zrozumiteľný jazyk a konkrétne odporúčania.\n\nVstupné údaje môžu obsahovať:\n\n    Názov firmy, sídlo, IČO\n\n    SK NACE kód + popis činnosti\n\n    Tržby, počet zamestnancov\n\n    Webstránka, verejne dostupné info o službách\n\nTvoja úloha:\n\n    Identifikuj hlavnú komoditu podnikania firmy\n\n    Zanalyzuj cieľový trh (B2B / B2C, segment)\n\n    Vytvor obchodný profil na základe veľkosti a oblasti\n\n    Navrhni odporúčania na obchodný rozhovor – čo riešiť, kde môže byť problém (napr. GDPR, IT, zmluvy...)\n\nVýstup generuj v tomto formáte:\n\n  Firma: [Názov spoločnosti]  \n  Komodita: [Laické zhrnutie činnosti firmy]  \n  SK NACE: [kód + popis]  \n  Obchodná analýza:\n- [Cieľový trh, segmentácia, veľkosť firmy]\n- [Ďalšie dôležité info – subdodávky, digitalizácia, rast]\n- [Potenciálne komplikácie alebo obchodné príležitosti]\n\n  Odporúčanie:\n- [Oblasti, na ktoré sa zamerať v obchodnom rozhovore]\n- [Možné problémy alebo námietky, ktoré môžu nastať]\n\n!Urob mi z toho 1 text z nazvom content"

            }

          ]

        },

        "simplify": false,

        "options": {}

      },

      "type": "@n8n/n8n-nodes-langchain.openAi",

      "typeVersion": 1.8,

      "position": [

        140,

        1080

      ],

      "id": "e454870e-61cb-49ef-9529-c5102efeb687",

      "name": "COMPANY ANALYSIS",

      "credentials": {

        "openAiApi": {

          "id": "n2yZc1G5Qst6SEWp",

          "name": "skuska skusenosti"

        }

      }

    },

    {

      "parameters": {

        "modelId": {

          "__rl": true,

          "value": "gpt-4.1-mini",

          "mode": "list",

          "cachedResultName": "GPT-4.1-MINI"

        },

        "messages": {

          "values": [

            {

              "content": "=Na základe výstupov z predchádzajúcich AI agentov (firma + hovor) priprav obchodný briefing pre obchodníka.\n\nPredtým ako začneš generovať odpoveď, **použi Think Tool** na hlboké zamyslenie sa nad vstupmi. Zamysli sa nad:\n- Aký typ klienta je táto firma?\n- Na čo by sa mal obchodník pripraviť v rozhovore?\n- Aké námietky by mohli nastať?\n- Čo by som ako obchodník určite chcel vedieť pred stretnutím?\n\nThink Tool slúži na zaznamenanie tvojho uvažovania – použi ho na vytvorenie súvislých poznámok, ktoré ti pomôžu pri následnom generovaní odpovede.\n\n---\n\n**Vstupy:**\n\n- Výstup o firme: {{ $json.info }}\n- Výstup z hovoru: {{ $('SET CONTENT').item.json.content }}\n\n---\n\n**Tvoja úloha:**\n\nNa základe týchto dvoch vstupov vytvor komplexný výstup rozdelený do častí:\n\na) Analýza komodity podnikania firmy  \nb) Analýza firmy (história, tržby, počet zam.)  \nc) Analýza osobnosti konateľa z hovoru  \nd) Analýza potrieb a predpokladaných námietok  \n\n+ **Sekcia pre obchodníka**: Odporúčania na čo sa zamerať, čo ponúknuť, aký môže byť ďalší postup. Použi jazyk priamo pre obchodníka, praktický a vecný.\n\n## príklad outputu:\n🏢 Firma: IBV Dvorníky s. r. o.\n📌 Komodita: Kúpa a predaj vlastných nehnuteľností, výstavba obytných a neobytných budov\n\n🛠️ SK NACE: 68100 – Kúpa a predaj vlastných nehnuteľností\n41209 – Výstavba obytných a neobytných budov i. n.\n\n💼 Analýza firmy:\n\n    História a vývoj: Firma založená v roku 2010, stabilne pôsobí na trhu realít v Bratislave a okolí.\n\n    Tržby a zamestnanci: Ročné tržby cca 5 miliónov EUR, zamestnáva približne 35 ľudí.\n\n    Kľúčové fakty: Firma sa zameriava na B2C segment a využíva lokálnych dodávateľov. V posledných rokoch stabilný rast napriek legislatívnym výzvam.\n\n🧑‍💼 Osobnostná analýza konateľa:\n\n    Konateľ prejavuje pragmatický a analytický prístup, je otvorený novým riešeniam, no kladie dôraz na overené postupy.\n\n    Rýchlo reaguje na otázky, preferuje jasné fakty a konkrétne riešenia.\n\n🎯 Potreby a námietky klienta:\n\n    Potrebuje spoľahlivé informácie o trhu a digitálne nástroje na efektívnejšie riadenie projektov.\n\n    Môže mať obavy z legislatívnych zmien a možných právnych komplikácií.\n\n💡 Odporúčania pre obchodníka:\n\n    Zamerať sa na diskusiu o aktuálnych trendoch v realitách a stavebníctve.\n\n    Ponúknuť riešenia zlepšujúce digitalizáciu a správu projektov.\n\n    Pripraviť sa na otázky ohľadom legislatívy a právnych rizík.\n\n!!Ako output napis len to co mas, nic ine okolo!!",

              "role": "system"

            }

          ]

        },

        "options": {}

      },

      "type": "@n8n/n8n-nodes-langchain.openAi",

      "typeVersion": 1.8,

      "position": [

        740,

        940

      ],

      "id": "b896ffb5-0817-4320-928b-89c409a1b823",

      "name": "BRIEFING CREATION",

      "credentials": {

        "openAiApi": {

          "id": "n2yZc1G5Qst6SEWp",

          "name": "skuska skusenosti"

        }

      }

    }

  ],

  "pinData": {},

  "connections": {

    "GET EVENTS": {

      "main": [

        [

          {

            "node": "LOOP THROUGH EVENTS",

            "type": "main",

            "index": 0

          }

        ]

      ]

    },

    "Think": {

      "ai_tool": [

        [

          {

            "node": "BRIEFING CREATION",

            "type": "ai_tool",

            "index": 0

          }

        ]

      ]

    },

    "ANALYZA OSOBNOSTI A NAVRHY": {

      "main": [

        [

          {

            "node": "SET CONTENT",

            "type": "main",

            "index": 0

          }

        ]

      ]

    },

    "Code": {

      "main": [

        [

          {

            "node": "DOWNLOAD MP3",

            "type": "main",

            "index": 0

          }

        ]

      ]

    },

    "FILTER MP3": {

      "main": [

        [

          {

            "node": "EXTRACT DATA",

            "type": "main",

            "index": 0

          }

        ],

        [

          {

            "node": "Error Handler1",

            "type": "main",

            "index": 0

          }

        ]

      ]

    },

    "EXTRACT DATA": {

      "main": [

        [

          {

            "node": "Code",

            "type": "main",

            "index": 0

          }

        ]

      ]

    },

    "DAILY TRIGGER": {

      "main": [

        [

          {

            "node": "GET EVENTS",

            "type": "main",

            "index": 0

          }

        ]

      ]

    },

    "HOTOVO": {

      "main": [

        [

          {

            "node": "No Operation, do nothing",

            "type": "main",

            "index": 0

          }

        ]

      ]

    },

    "DOWNLOAD MP3": {

      "main": [

        [

          {

            "node": "PREPIS MP3",

            "type": "main",

            "index": 0

          }

        ]

      ]

    },

    "PREPIS MP3": {

      "main": [

        [

          {

            "node": "ANALYZA OSOBNOSTI A NAVRHY",

            "type": "main",

            "index": 0

          }

        ]

      ]

    },

    "WEB SCRAPE": {

      "main": [

        [

          {

            "node": "COMPANY ANALYSIS",

            "type": "main",

            "index": 0

          }

        ]

      ]

    },

    "SET INFO": {

      "main": [

        [

          {

            "node": "BRIEFING CREATION",

            "type": "main",

            "index": 0

          }

        ]

      ]

    },

    "SET CONTENT": {

      "main": [

        [

          {

            "node": "WEB SCRAPE",

            "type": "main",

            "index": 0

          }

        ]

      ]

    },

    "WRITE TO DATABASE": {

      "main": [

        [

          {

            "node": "SEND A MESSAGE",

            "type": "main",

            "index": 0

          }

        ]

      ]

    },

    "SEND A MESSAGE": {

      "main": [

        [

          {

            "node": "SEND BRIEFING",

            "type": "main",

            "index": 0

          }

        ]

      ]

    },

    "LOOP THROUGH EVENTS": {

      "main": [

        [

          {

            "node": "HOTOVO",

            "type": "main",

            "index": 0

          }

        ],

        [

          {

            "node": "FILTER MP3",

            "type": "main",

            "index": 0

          }

        ]

      ]

    },

    "SEND BRIEFING": {

      "main": [

        [

          {

            "node": "LOOP THROUGH EVENTS",

            "type": "main",

            "index": 0

          }

        ]

      ]

    },

    "COMPANY ANALYSIS": {

      "main": [

        [

          {

            "node": "SET INFO",

            "type": "main",

            "index": 0

          }

        ]

      ]

    },

    "BRIEFING CREATION": {

      "main": [

        [

          {

            "node": "WRITE TO DATABASE",

            "type": "main",

            "index": 0

          }

        ]

      ]

    }

  },

  "active": false,

  "settings": {

    "executionOrder": "v1",

    "callerPolicy": "workflowsFromSameOwner",

    "executionTimeout": -1,

    "errorWorkflow": "U8hhzlNd3ZxSzbTB"

  },

  "versionId": "4a82e578-6ef3-48b1-9bda-c7c0b74d23d2",

  "meta": {

    "templateCredsSetupCompleted": true,

    "instanceId": "e162ea57b8d2fa6fe62ccd7df6777bf872d01616458b6ac8eeba09c33907f975"

  },

  "id": "oO7x5tipr2CEtwSQ",

  "tags": [

    {

      "createdAt": "2025-07-06T09:42:01.569Z",

      "updatedAt": "2025-07-06T09:42:01.569Z",

      "id": "aSOsiFgEyrvHQexm",

      "name": "basic-nodes"

    },

    {

      "createdAt": "2025-07-06T09:36:57.439Z",

      "updatedAt": "2025-07-06T09:36:57.439Z",

      "id": "bAS41ao9nJ3olNhO",

      "name": "ai-analysis"

    },

    {

      "createdAt": "2025-07-06T09:36:57.464Z",

      "updatedAt": "2025-07-06T09:36:57.464Z",

      "id": "gz2pUFfPkPKTIbXS",

      "name": "sales-automation"

    }

  ]

}
```