# novo-sistema-mapas-2026

Sistema de entrega automatizada de mapas astrais, numerológicos e produtos digitais.

## Visão Geral

Automação 100% do fluxo de venda → geração → entrega de infoprodutos low-ticket.
Do webhook de compra até o WhatsApp e e-mail do cliente, sem intervenção manual.

## Documentação

| Arquivo | Conteúdo |
|---|---|
| [ARCHITECTURE.md](docs/ARCHITECTURE.md) | Arquitetura geral, decisões técnicas |
| [FLOW.md](docs/FLOW.md) | Fluxo detalhado de cada produto |
| [STACK.md](docs/STACK.md) | Stack completa com justificativas |
| [API.md](docs/api/API.md) | Documentação da Astrology API |

## Estrutura do Projeto

```
novo-sistema-mapas-2026/
├── docs/               # Documentação e arquitetura
│   ├── api/            # Coleção Postman + docs da API
│   └── flows/          # Fluxos por produto
├── src/
│   ├── webhooks/       # Recebimento de pedidos (Yampi)
│   ├── collectors/     # Coleta de dados do cliente
│   ├── generators/     # Geração de conteúdo (textos, imagens, PDF)
│   ├── delivery/       # Entrega (WhatsApp + Email)
│   ├── tracking/       # Google Sheets + Meta Pixel
│   └── utils/          # Logger, helpers, validações
├── templates/          # HTML dos mapas (→ PDF)
└── config/             # Variáveis e configurações
```

## Fluxo Principal

```
Yampi Webhook → Coleta Dados → Astrology API → Haiku (textos)
→ Sonnet (auditoria) → DALL-E (imagens) → Puppeteer (PDF)
→ Resend (email) + Evolution API (WhatsApp)
→ Google Sheets (registro) + Meta Pixel (conversão)
```

## Produtos

| Produto | Status |
|---|---|
| Mapa Astral Base | Em desenvolvimento |
| Mapa Numerológico | Em desenvolvimento |
| Mapa Kármico | Planejado |
| Mapa Astral de Carreira | Planejado |

## Stack

- **Runtime**: Node.js v18+ (Hostinger VPS Ubuntu 22.04)
- **AI**: Claude Haiku (textos) + Sonnet (auditoria) + DALL-E (imagens)
- **PDF**: Puppeteer
- **Entrega**: Evolution API (WhatsApp) + Resend (email)
- **Dados**: Google Sheets
- **Pagamentos**: Yampi
- **Rastreio**: Meta Pixel
