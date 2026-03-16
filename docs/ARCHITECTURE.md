# Arquitetura do Sistema

## Princípios

- **Zero intervenção manual**: do pedido à entrega, tudo automático
- **Modular por produto**: cada mapa tem seu próprio módulo, mas compartilham a infraestrutura
- **Fail-safe**: alertas de erro crítico, retentativas automáticas, logs completos
- **Auditoria de qualidade**: Sonnet revisa estrutura e coerência do conteúdo antes de entregar

---

## Diagrama de Fluxo Geral

```
┌─────────────────────────────────────────────────────────────┐
│                     ENTRADA                                  │
│  Yampi Webhook → pedido aprovado + dados do produto         │
└──────────────────────────┬──────────────────────────────────┘
                           │
                           ▼
┌─────────────────────────────────────────────────────────────┐
│                   ORQUESTRADOR                               │
│  - Valida webhook (assinatura Yampi)                        │
│  - Identifica produto (ID Yampi → tipo de mapa)             │
│  - Direciona para módulo correto                            │
└──────────┬──────────────────────────┬───────────────────────┘
           │                          │
           ▼                          ▼
    [Mapa Astral]             [Mapa Numerológico]    ... outros
           │
           ▼
┌─────────────────────────────────────────────────────────────┐
│                  COLETA DE DADOS                             │
│  - WhatsApp (Evolution API): solicita data/hora/local       │
│  - Valida e normaliza os dados recebidos                    │
│  - Salva estado no Google Sheets                            │
└──────────────────────────┬──────────────────────────────────┘
                           │
                           ▼
┌─────────────────────────────────────────────────────────────┐
│                  CÁLCULOS                                    │
│  Astrology API → JSON com posições planetárias, casas,      │
│  aspectos, nodos, etc.                                      │
└──────────────────────────┬──────────────────────────────────┘
                           │
                           ▼
┌─────────────────────────────────────────────────────────────┐
│                  GERAÇÃO DE CONTEÚDO                         │
│  1. Claude Haiku  → gera textos para cada posição/aspecto  │
│  2. Claude Sonnet → audita: estrutura correta? conteúdo     │
│                     coerente com a posição astrológica?     │
│  3. DALL-E        → gera imagens/criativos do mapa          │
└──────────────────────────┬──────────────────────────────────┘
                           │
                           ▼
┌─────────────────────────────────────────────────────────────┐
│                  GERAÇÃO DO PDF                              │
│  - Template HTML preenchido com dados + textos + imagens    │
│  - Puppeteer renderiza e exporta PDF                        │
│  - Salva no Google Drive (/pdfs-gerados/)                   │
└──────────────────────────┬──────────────────────────────────┘
                           │
                           ▼
┌─────────────────────────────────────────────────────────────┐
│                  ENTREGA                                     │
│  - Resend: envia email com PDF                              │
│  - Evolution API: envia PDF via WhatsApp                    │
└──────────────────────────┬──────────────────────────────────┘
                           │
                           ▼
┌─────────────────────────────────────────────────────────────┐
│                  PÓS-ENTREGA                                 │
│  - Google Sheets: registra pedido como entregue             │
│  - Meta Pixel: dispara evento de conversão                  │
│  - Relatório automático (diário/semanal)                    │
│  - FAQ automático: responde dúvidas do cliente              │
│  - Alerta de erro crítico (se falha em qualquer etapa)      │
└─────────────────────────────────────────────────────────────┘
```

---

## Módulos

### `src/webhooks/`
- Recebe e valida payloads da Yampi
- Extrai: produto comprado, dados do comprador, ID do pedido
- Publica evento interno para o orquestrador

### `src/collectors/`
- Gerencia conversa via WhatsApp para coletar dados de nascimento
- State machine: aguarda → coletou data → coletou hora → coletou cidade → completo
- Timeout e reenvio de mensagem se cliente não responder

### `src/generators/`
- `text-generator.js` — chama Haiku com prompts por tipo de posição
- `content-auditor.js` — Sonnet valida qualidade e coerência
- `image-generator.js` — DALL-E via prompt estruturado
- `pdf-generator.js` — Puppeteer + template HTML

### `src/delivery/`
- `whatsapp.js` — Evolution API (envio de texto e arquivo)
- `email.js` — Resend (email com PDF em anexo)

### `src/tracking/`
- `sheets.js` — leitura e escrita no Google Sheets
- `pixel.js` — Conversions API do Meta

### `src/utils/`
- `logger.js` — Winston: info/warn/error/debug
- `validator.js` — validação de dados de entrada
- `alerter.js` — alerta por WhatsApp/email em erros críticos

---

## Decisões de Arquitetura

| Decisão | Escolha | Motivo |
|---|---|---|
| Geração de texto | Claude Haiku | Custo baixo, velocidade alta para volume |
| Auditoria | Claude Sonnet | Capacidade analítica superior para QA |
| PDF | Puppeteer | Controle total sobre layout HTML/CSS |
| WhatsApp | Evolution API | API não-oficial mas estável, sem custo por mensagem |
| Email | Resend | Developer-first, confiável, fácil de usar |
| Dados | Google Sheets | Cliente já usa, sem custo extra de banco de dados |
| Logs | Winston | Padronizado, múltiplos transportes |

---

## Segurança

- Variáveis sensíveis em `.env` (nunca no git)
- Validação e sanitização em todas as entradas
- Rate limiting nos endpoints públicos
- Encryption AES-256-CBC para dados sensíveis
- Audit logs de todas as operações críticas
- Webhook com validação de assinatura Yampi

---

## Convenções de Código

```
Arquivos:    kebab-case      (ex: text-generator.js)
Funções:     camelCase       (ex: generateMapText)
Constantes:  UPPER_SNAKE_CASE (ex: MAX_RETRIES)
Async:       sempre try-catch
Logs:        logger.info/warn/error/debug
```
