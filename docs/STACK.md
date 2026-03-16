# Stack Completa

## Infraestrutura

| Componente | Tecnologia | Detalhes |
|---|---|---|
| Servidor | Hostinger VPS KVM 2 | Ubuntu 22.04 |
| Runtime | Node.js v18+ | |
| Versionamento | GitHub | este repositório |

## Integrações Externas

| Integração | Uso | Docs |
|---|---|---|
| Yampi | Webhook de pedidos aprovados | [docs/api/yampi.md](api/yampi.md) |
| Astrology API | Cálculos astrológicos (JSON) | [docs/api/API.md](api/API.md) |
| Claude Haiku | Geração de textos personalizados | [docs/api/claude.md](api/claude.md) |
| Claude Sonnet | Auditoria de qualidade do conteúdo | [docs/api/claude.md](api/claude.md) |
| DALL-E | Geração de imagens/criativos | |
| Puppeteer | Renderização de HTML → PDF | |
| Evolution API | Envio via WhatsApp | |
| Resend | Envio de email com PDF | |
| Google Sheets | Base de dados de pedidos e métricas | |
| Google Drive | Armazenamento de PDFs e assets | |
| Meta Pixel (Conversions API) | Rastreamento de conversão | |

## Modelos de IA

| Modelo | Papel | Motivo |
|---|---|---|
| `claude-haiku-4-5` | Geração de textos em massa | Custo baixo, rápido |
| `claude-sonnet-4-6` | Auditoria e QA do conteúdo | Análise mais profunda |
| DALL-E 3 | Imagens dos mapas | Integração direta OpenAI |

## Dependências Node.js (planejadas)

```json
{
  "dependencies": {
    "@anthropic-ai/sdk": "latest",
    "puppeteer": "latest",
    "resend": "latest",
    "googleapis": "latest",
    "axios": "latest",
    "winston": "latest",
    "dotenv": "latest",
    "express": "latest",
    "zod": "latest"
  }
}
```
