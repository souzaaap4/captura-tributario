# Auditoria técnica — `lead-tributario` (piloto de padronização)

> Documento de auditoria gerado ao comparar o workflow `lead-Tributario` (`rD0LOINTnTAGGvrm`)
> contra o padrão de referência **Imersão** (`UDKdJvXqzq8Yi5jr`).
> Estado auditado: export ao vivo da instância `n8n.srv1095468.hstgr.cloud` em 2026-07-07.
> **Nenhuma alteração foi aplicada à produção** — este é o artefato de revisão (modo *staged*).

---

## 0. Descoberta que reenquadra a padronização: arquitetura de funis pareados

Antes de listar divergências, um achado estrutural que muda o significado de "padronizar para a Imersão":

A família **não** replica a Imersão (1 workflow que segmenta Pago/Organico internamente via IF em `fbclid`).
Ela usa **funis pareados** — dois workflows/landing pages separados por produto:

| Produto | Workflow orgânico (`LP-*`) | Workflow pago (`lead-*`) |
|---|---|---|
| Tributário | `LP-Tributario` → aba **Organico**, Funil `tributario-organico` | **`lead-tributario`** → aba **Pago**, Funil `Tributario` |
| Diagnóstico | `LP-diagnostico` → Organico | `lead-diagnostico` → Pago |
| Ads / Isca | `LP-IscaAds` → Organico | `lead-ads` → Pago |

**Consequência:** o comportamento "100% dos leads vão para a aba Pago" em `lead-tributario`
**não é bug** — é a classificação de origem feita *no nível do funil*, não por um IF interno.
Portanto a padronização **não** deve forçar o IF `É pago?` da Imersão aqui.
Isso é uma **regra de negócio a preservar**, não uma inconsistência a corrigir.

Fica em aberto (decisão do time — ver §6) se o time prefere **consolidar** cada par em um único
workflow estilo Imersão, ou **manter os pares** e apenas aplicar as melhorias de robustez.

---

## 1. Sumário executivo

`lead-tributario` é um esqueleto de 8 nós (Webhook → Parse → Ler banco → Filtrar duplicata →
2× Format → 2× Sheets). Ele **funciona** para o caminho feliz, mas fica muito abaixo do padrão
Imersão em robustez, observabilidade e captura de dados. Os problemas mais graves:

1. **Descarte silencioso** — duplicatas somem via `return []`, sem log nem alerta (a Imersão registra + notifica).
2. **Sem validação de campos obrigatórios** — lead com nome/e-mail/WhatsApp inválido é gravado assim mesmo.
3. **Perda de tracking** — `fbclid` e `utm_term` são capturados na LP mas descartados no Parse.
4. **Secret do webhook exposto no JS público** da landing page (exatamente a falha que a Imersão corrigiu com proxy PHP).
5. **MQL confia no cliente** — o Parse aceita o `mql` calculado no navegador, sem recalcular no servidor.
6. **Leitura de planilha não otimizada** — `Ler banco de dados` lê a linha inteira (a Imersão lê só a coluna E via HTTP).
7. **Sem documentação** — nenhum Sticky Note no canvas, nenhum README de automação.

Nenhum desses é conflitante com a regra de negócio do funil pago — todos são melhorias de
qualidade/robustez diretamente transplantáveis da Imersão.

---

## 2. Comparação arquitetural (lead-tributario × Imersão)

| Capacidade | Imersão (referência) | lead-tributario (hoje) | Ação |
|---|---|---|---|
| Nós | 25 (+1 sticky) | 8 | Expandir |
| Autenticação do webhook | `headerAuth` + secret via **proxy PHP** (server-side) | `headerAuth` **com secret no JS público** | Corrigir (proxy) |
| Validação de obrigatórios | Nó `Validar` + IF `Lead válido?` | ausente | Adicionar |
| Descarte | marcado `_descartado` → log `Descartes` + e-mail | `return []` silencioso | Adicionar log + e-mail |
| Dedup WhatsApp | HTTP Request lê só col. E (~24× menos dado) | node Sheets lê linha inteira | Otimizar |
| Segmentação Pago/Organico | IF interno em `fbclid` | **por funil (pareado)** — sempre Pago | **Preservar (não é bug)** |
| Captura UTM | source/medium/campaign/content/term/id + fbclid/src/sck/a_id | source/medium/campaign/content/channel/referrer/page_url | Completar (term, fbclid) |
| MQL | recalculado no servidor | confia no cliente | Recalcular no servidor |
| CRM | dedup → arquiva → update in-place + merge histórico | **inexistente** | **Decisão do time (§6)** |
| Error Workflow | `Alerta de Erro - Leads` | idem (`g9jQ4oRB4Z2blFpx`) — **já OK** | — |
| Documentação | README de automação + Sticky Note + Mermaid | ausente | Adicionar |

---

## 3. Rastreio campo a campo (Landing Page → destino)

Formulário `#regForm` em `index.html`. Campos e destino atual:

| Campo LP (`name`) | Origem | Enviado no payload? | Extraído no Parse? | Destino Sheets | Perda? |
|---|---|---|---|---|---|
| `nome` | input texto | sim | sim (`Nome completo`) | Nome completo | não |
| `email` | input email | sim | sim (`E-mail`) | E-mail | não |
| `whatsapp` | input tel (mascarado) | sim | sim → normalizado `(DD) XXXXX-XXXX` | WhatsApp | não |
| `whatsapp_confirmacao` | input tel | sim | sim (`Confirmar WhatsApp`) | Confirmar WhatsApp | não |
| `nivel_ml` | select | sim | sim → legenda + concat em Pergunta Adicional | Pergunta Adicional | não |
| `regime_tributario` | select | sim | sim → legenda + concat em Pergunta Adicional | Pergunta Adicional | não |
| `mql` (calc. no cliente) | JS (`qualified`) | sim | **sim, sem recalcular** | MQL | ⚠ confia no cliente |
| `channel`/`canal` | querystring | sim | sim | Channel | não |
| `utm_source/medium/campaign/content` | querystring | sim | sim | Source/Medium/Campaign/Content | não |
| `utm_term` | querystring | **sim** | **não** | — | 🔴 **perdido** |
| `fbclid` | querystring | **sim** | **não** | — | 🔴 **perdido** |
| `referrer` / `page_url` / `timestamp` | JS | sim | sim | Referrer / Page URL / Timestamp | não |
| `utm_id`, `gclid`, `src`, `sck`, `a_id` | — | **não capturados na LP** | — | — | ⚠ vs. Imersão |

**Regra de negócio específica (preservar):** `nivel_ml` + `regime_tributario` são traduzidos para
legendas (ex.: `ouro`→`Ouro`, `simples_nacional`→`Simples Nacional`) e concatenados em
`Pergunta Adicional`. MQL do tributário = **qualificado se tem medalha ML *e* não é MEI**.

**Segundo webhook:** a LP dispara para **dois** endpoints — `lead-Tributario` (esta instância)
e `p4-consultoria` (instância `ecomidia-n8n.misrsj.easypanel.host`, **outra infra**). Precisa de
contexto do time (ver §6) — não mexo nisso sem confirmação.

---

## 4. Mapeamento Google Sheets (planilha `1FdV_…doV3WKZA`, credencial `Cockpit`)

Grava em **Pago** (gid 255850863) e **banco de dados** (gid 411493318) — mesma planilha
compartilhada com a Imersão e os demais funis; segregação lógica pela coluna `Funil`.

| Coluna | Nó responsável | Expressão | Valor esperado | Pode ser nulo? |
|---|---|---|---|---|
| Data de entrada de leads | Parse | relógio do servidor (America/Sao_Paulo) | `dd/mm/aaaa` | não |
| Hora | Parse | relógio do servidor | `HH:MM` | não |
| Nome completo / E-mail / WhatsApp | Parse | `pick()` do payload | texto | só se inválido (hoje não barrado) |
| Confirmar WhatsApp | Parse | `pick()` normalizado | texto | frequentemente vazio |
| Pergunta Adicional | Parse | concat medalha+regime | texto | pode vazio |
| Funil | Parse | literal `'Tributario'` | `Tributario` | não |
| MQL | Parse | valor do cliente | `Sim`/`Não` | ⚠ depende do cliente |
| Channel/Source/Medium/Campaign/Content | Parse | `pick()` querystring | texto | sim (orgânico) |
| Referrer / Page URL / Timestamp | Parse | JS/ISO | texto/URL | não |

Sem `utm_term`, `fbclid`, `utm_id` nas colunas — mesmo quando presentes na URL (perda do §3).

---

## 5. Lista de inconsistências (ranqueada por severidade)

| # | Severidade | Achado | Correção proposta (transplante da Imersão) |
|---|---|---|---|
| I1 | 🔴 Alta | Duplicata descartada com `return []` — sem auditoria nem alerta | Marcar `_descartado`, IF de roteamento, gravar em `Descartes` + e-mail Gmail |
| I2 | 🔴 Alta | Sem validação de nome/e-mail/WhatsApp — lead inválido é gravado | Nó `Validar campos obrigatórios` + IF `Lead válido?` → ramo de descarte |
| I3 | 🔴 Alta | `WEBHOOK_SECRET` literal no JS público da LP | Proxy PHP (`api/lead-proxy.php`) guardando o secret server-side |
| I4 | 🟠 Média | `fbclid` e `utm_term` capturados mas descartados no Parse | Estender `pick()` no Parse para incluí-los (+ colunas) |
| I5 | 🟠 Média | MQL confia no valor do cliente | Recalcular no servidor: `medalha≠sem_medalha && regime≠mei` |
| I6 | 🟡 Baixa | `Ler banco de dados` lê linha inteira | Trocar por HTTP Request lendo só `banco de dados!E:E` |
| I7 | 🟡 Baixa | Sem Sticky Note / README de automação | Adicionar (este pacote) |
| I8 | 🟡 Baixa | `_wppDigits` guarda telefone formatado, não dígitos (nome enganoso) | Renomear/ajustar para dígitos crus |
| I9 | ⚪ Info | `utm_id`/`src`/`sck`/`a_id`/`gclid` não capturados na LP | Alinhar com Imersão se as campanhas usarem esses parâmetros |

---

## 6. Decisões de negócio necessárias antes do build (não assumo)

1. **CRM.** A Imersão alimenta o CRM comercial (dedup → arquiva → update in-place). Os funis
   pagos/orgânicos **hoje não gravam no CRM**. Adicionar CRM muda o que o time comercial vê.
   → Estes funis **devem** passar a alimentar o CRM como a Imersão, ou só planilha?
2. **Consolidar vs. manter pares.** Manter `lead-tributario` + `LP-Tributario` separados
   (aplicando só as melhorias de robustez), ou consolidar cada par em 1 workflow estilo Imersão
   com IF `É pago?` interno?
3. **Segundo webhook `p4-consultoria`** (outra instância n8n) — é intencional (duplo destino)?
   Devo preservar, remover, ou está fora do escopo?

---

## 7. Regras de negócio a PRESERVAR (não padronizar para fora)

- Funil = `Tributario`; classificação de origem feita no nível do funil (pareado) → aba **Pago**.
- Campos `nivel_ml` + `regime_tributario` → legendas concatenadas em `Pergunta Adicional`.
- Regra de MQL do tributário (medalha ML presente **e** regime ≠ MEI).
- Checkout Greenn/payfast pré-populado (`fn`/`em`/`ph`) + propagação de UTMs/fbclid.
