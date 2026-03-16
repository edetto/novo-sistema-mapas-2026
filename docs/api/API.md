# Astrology API

Documentação da API de cálculos astrológicos.

> **Coleção Postman**: adicionar arquivo `postman-collection.json` nesta pasta.
> Quando recebida, documentar aqui os endpoints, parâmetros e exemplos de resposta.

---

## Endpoints Identificados

_A preencher conforme envio do Postman._

### Estrutura Esperada do JSON de Resposta

O JSON retornado pela API deve conter (a confirmar):

```json
{
  "sun": { "sign": "Áries", "degree": 15.3, "house": 1 },
  "moon": { "sign": "Touro", "degree": 22.1, "house": 2 },
  "ascendant": { "sign": "Peixes", "degree": 5.7 },
  "planets": {
    "mercury": { "sign": "...", "degree": 0, "house": 0, "retrograde": false },
    "venus":   { "sign": "...", "degree": 0, "house": 0, "retrograde": false },
    "mars":    { "sign": "...", "degree": 0, "house": 0, "retrograde": false },
    "jupiter": { "sign": "...", "degree": 0, "house": 0, "retrograde": false },
    "saturn":  { "sign": "...", "degree": 0, "house": 0, "retrograde": false },
    "uranus":  { "sign": "...", "degree": 0, "house": 0, "retrograde": false },
    "neptune": { "sign": "...", "degree": 0, "house": 0, "retrograde": false },
    "pluto":   { "sign": "...", "degree": 0, "house": 0, "retrograde": false }
  },
  "houses": {
    "1": { "sign": "...", "degree": 0 },
    "...": {}
  },
  "aspects": [
    { "planet1": "Sol", "planet2": "Lua", "type": "Trígono", "orb": 2.1 }
  ],
  "nodes": {
    "north": { "sign": "...", "degree": 0, "house": 0 },
    "south": { "sign": "...", "degree": 0, "house": 0 }
  }
}
```

---

## Como Adicionar o Postman

1. Exporte a coleção no Postman: `File → Export → Collection v2.1`
2. Salve como `docs/api/postman-collection.json`
3. Documente os endpoints principais neste arquivo
