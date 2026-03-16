# Fluxo Detalhado por Produto

## Status dos Produtos

| Produto | Fluxo | Template HTML | Prompts IA | Status |
|---|---|---|---|---|
| Mapa Astral | [fluxo abaixo](#mapa-astral) | A definir | A definir | Em desenvolvimento |
| Mapa Numerológico | A documentar | A definir | A definir | Em desenvolvimento |
| Mapa Kármico | A documentar | A definir | A definir | Planejado |
| Mapa Astral de Carreira | A documentar | A definir | A definir | Planejado |

---

## Mapa Astral

### 1. Entrada
```
POST /webhook/yampi
{
  "order_id": "...",
  "product_id": "...",
  "customer": {
    "name": "...",
    "email": "...",
    "phone": "5511999999999"
  }
}
```

### 2. Coleta de Dados (WhatsApp)
```
Bot: "Olá [nome]! Para gerar seu Mapa Astral preciso de 3 informações:"
Bot: "1. Data de nascimento (DD/MM/AAAA)"
Cliente: "15/03/1990"
Bot: "2. Horário de nascimento (HH:MM) — se não souber, responda 'não sei'"
Cliente: "14:30"
Bot: "3. Cidade e estado de nascimento"
Cliente: "São Paulo, SP"
Bot: "Perfeito! Estou gerando seu mapa, em alguns minutos envio aqui."
```

### 3. Chamada da API Astrológica
- Endpoint: a definir (ver Postman)
- Input: data + hora + cidade/coordenadas
- Output: JSON com posições (Sol, Lua, Ascendente, planetas, casas, aspectos)

### 4. Geração de Conteúdo
Para cada elemento do mapa:
```
Haiku → texto de 150-250 palavras para a posição
Sonnet → revisa: está coerente com [planeta] em [signo] na [casa]?
         se não → reescreve / sinaliza para correção
```

### 5. Geração de Imagem (DALL-E)
- Prompt base: definido no briefing do produto
- Personalizado com: elementos do mapa, cores correspondentes

### 6. Geração do PDF
- Template HTML preenchido com todos os dados
- Puppeteer renderiza
- Salva em Google Drive `/pdfs-gerados/mapa-astral/[order_id].pdf`

### 7. Entrega
```
WhatsApp: "Seu Mapa Astral está pronto! 🌟" + PDF
Email:    subject: "Seu Mapa Astral - [nome]" + PDF em anexo
```

### 8. Registro e Rastreio
```
Google Sheets "Pedidos - Mapa Astral":
  order_id | nome | email | phone | status | entregue_em

Meta Pixel: Purchase event
  value: [preço do produto]
  currency: BRL
  content_name: "Mapa Astral"
```

---

## Tratamento de Erros

| Etapa | Erro | Ação |
|---|---|---|
| Webhook | Assinatura inválida | Log + ignora |
| Coleta | Cliente não responde em 24h | Alerta manual + reenvio |
| API Astro | Timeout / erro | Retry 3x, depois alerta crítico |
| IA | Conteúdo reprovado na auditoria | Regera 1x, se falhar → alerta |
| PDF | Falha Puppeteer | Retry, alerta crítico |
| Entrega | WhatsApp falhou | Tenta email, alerta |
| Entrega | Email falhou | Alerta crítico |

### Alerta Crítico
Quando ocorre erro crítico em qualquer etapa:
- WhatsApp para número do operador
- Log de erro completo (stack trace)
- Pedido marcado como `ERRO` no Sheets para reprocessamento manual
