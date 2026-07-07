# Captura Tributário

> Landing page de captação e qualificação de leads para o curso de tributação para marketplaces — tráfego pago.

![Status](https://img.shields.io/badge/status-ativo-22c55e)
![Frontend](https://img.shields.io/badge/frontend-HTML%2FCSS%2FJS-1f6feb)
![Automação](https://img.shields.io/badge/automacao-n8n%20Webhook-0f766e)
![Deploy](https://img.shields.io/badge/deploy-HostGator%2FcPanel-7c3aed)

🌐 **[Ver página ao vivo](https://tributariomarketplace.metodop4.com.br/)** · 📁 **[Repositório GitHub](https://github.com/taysouzaa/captura-tributario)**

## Visão do Projeto

O **Captura Tributário** foi construído para transformar tráfego pago em leads qualificados para o curso de tributação para marketplaces do Método P4, conectando aquisição, coleta de dados e automação em um fluxo único.

### O que o sistema resolve

- Evita perda de lead entre formulário e automação.
- Centraliza captação com validação de dados e qualificação por medalha e regime tributário.
- Preserva origem de tráfego (UTM) para análise de performance de campanhas.
- Classifica automaticamente o lead como MQL (qualificado) ou não qualificado.

## O Que Foi Desenvolvido

### 1. Captação e Tracking
- Captura de origem (`channel`, `source`, `medium`, `campaign`, `content`, `referrer`, `page_url`) via script de tracking first-touch.
- Persistência de parâmetros UTM no navegador para reaproveitamento no submit.

### 2. Formulário de Qualificação
- Captura de nome, e-mail, WhatsApp e confirmação.
- Pergunta de qualificação: **medalha no Mercado Livre** e **regime tributário atual**.
- Validação local de consistência de telefone (WhatsApp e confirmação).
- Interface responsiva para mobile e desktop.

### 3. Lógica de Qualificação (MQL)
- `MQL = "Sim"` quando: medalha ≠ sem_medalha **e** regime ≠ MEI.
- `MQL = "Não"` para leads fora do perfil ideal.
- Campo enviado no payload para o webhook n8n.

### 4. Integração com Automação
- Envio de payload para webhook n8n (`/webhook/lead-Tributario`).
- Deduplicação por telefone no n8n antes de gravar na planilha.
- Gravação simultânea na aba **pago** e na aba **banco de dados** do Google Sheets.

## Stack Técnica

- **Frontend:** HTML5, CSS3, JavaScript (vanilla)
- **Tipografia:** Sora (local via `@font-face`)
- **Tracking:** Google Tag Manager + Microsoft Clarity
- **Integração:** Webhook n8n via **proxy PHP** server-side (`api/lead-proxy.php`) — o segredo do webhook fica no servidor, fora do JS público
- **Deploy:** HostGator/cPanel

## Arquitetura (Resumo)

| Camada | Responsabilidade |
| --- | --- |
| `index.html` | LP principal |
| `assets/` | Imagens, vídeos e ícones |
| `fonts/` | Tipografia local (Sora) |
| `depoimentos/` | Screenshots de depoimentos |
| `docs/` | Workflow n8n e documentação de UTMs |
| `DOCUMENTACAO.md` | Documentação técnica completa |

## Funcionamento do Sistema

1. Usuário acessa a LP via anúncio pago.
2. Tracking first-touch inicializa e persiste UTMs.
3. Usuário preenche formulário de qualificação.
4. Aplicação calcula MQL e monta payload com dados + tracking.
5. Payload é enviado para o webhook n8n.
6. n8n deduplica por telefone consultando o banco de dados.
7. Lead é gravado nas abas **pago** e **banco de dados** do Google Sheets.

```mermaid
flowchart TD
    subgraph LP["🌐 Landing Page (navegador)"]
        IN["Inputs do formulário<br/>nome · e-mail · WhatsApp<br/>Medalha ML + Regime tributário<br/>+ tracking: utm_* · fbclid · src · sck · a_id"]
    end
    subgraph SRV["🔒 Servidor HostGator (cPanel)"]
        PHP["api/lead-proxy.php<br/>anexa header x-p4-webhook-secret"]
    end
    subgraph WF["⚙️ n8n — workflow lead-tributario"]
        WH(["▶ TRIGGER · Webhook<br/>POST /webhook/lead-Tributario · headerAuth"])
        PARSE["Parse<br/>normaliza telefone · recalcula MQL no servidor<br/>timestamp do servidor · tracking completo"]
        VAL["Validar campos obrigatórios"]
        DVAL{"Lead válido?"}
        READ["Ler banco de dados<br/>HTTP → Google Sheets API · só coluna E"]
        REF["Reformatar leitura"]
        DUP["Filtrar duplicata<br/>dedup por WhatsApp"]
        DDUP{"Duplicata?"}
        FMT["Format lead<br/>calcula Orgânico? (sim/não)"]
        LCRM["Ler CRM"]
        BDUP["Buscar duplicata CRM<br/>e-mail → Telefone 1"]
        DCRM{"Existe no CRM?"}
        PREP["Preparar arquivo"]
        ARC["Arquivar CRM antigo"]
        UPD["Atualizar CRM (in-place)<br/>Funil · Data · MQL · Pergunta"]
        FCRM["Format CRM"]
        NEW["Criar lead no CRM<br/>Telefone 1 · Orgânico?"]
        DESC["Formatar descarte"]
        REG["Registrar descarte"]
    end
    subgraph SHEETS["📊 Google Sheets · credencial Cockpit"]
        SBD[("banco de dados<br/>100% dos leads")]
        SSEG[("aba Pago")]
        SARC[("Archived Leads")]
        SDESC[("Descarte")]
    end
    subgraph CRMBOX["📇 CRM comercial · credencial Cockpit"]
        CRMDB[("aba CRM<br/>Telefone 1 · Orgânico?")]
    end
    subgraph OUT["🔌 Integrações e saídas"]
        GMAIL["✉️ Gmail<br/>alerta de descarte"]
        CONS["p4-consultoria<br/>2ª instância n8n"]
        ERR[["🚨 Error Workflow<br/>Alerta de Erro - Leads"]]
    end

    IN --> PHP
    IN -. 2º destino .-> CONS
    PHP --> WH --> PARSE --> VAL --> DVAL
    DVAL -- inválido --> DESC
    DVAL -- válido --> READ --> REF --> DUP --> DDUP
    DDUP -- duplicata --> DESC
    DDUP -- novo --> FMT
    FMT --> SBD
    FMT --> SSEG
    FMT --> LCRM --> BDUP --> DCRM
    DCRM -- existe --> PREP --> ARC
    ARC --> SARC
    ARC --> UPD --> CRMDB
    DCRM -- novo --> FCRM --> NEW --> CRMDB
    DESC --> REG --> SDESC
    REG --> GMAIL
    READ -. exceção .-> ERR
    SBD -. exceção .-> ERR
    CRMDB -. exceção .-> ERR
```

## Estrutura do Projeto

```text
.
├─ assets/
│  ├─ hero-bg.png
│  ├─ logo-p4-nav.png
│  ├─ logo-p4-footer.png
│  ├─ equipe-p4.jpg
│  ├─ equipe-p4-reuniao.jpg
│  ├─ poster-aula.webp
│  └─ video-aula.mp4
├─ depoimentos/
├─ fonts/
│  └─ static/
├─ docs/
│  ├─ n8n-workflow-tributario.json
│  └─ links-utms-facebook-google.md
├─ index.html
├─ DOCUMENTACAO.md
└─ LICENSE
```

## Automação de leads (estado atual — 2026-07)

O lead **não é mais enviado direto do navegador ao n8n**. A página chama o **proxy PHP**
`api/lead-proxy.php` (mesmo domínio, sem nenhum segredo); o proxy anexa o header de autenticação
`x-p4-webhook-secret` **no servidor** e repassa ao webhook n8n `POST /webhook/lead-Tributario`.

O workflow n8n **`lead-tributario`** executa o pipeline completo (funil pago → aba **Pago**):

- **Valida** os obrigatórios e **deduplica por WhatsApp** — sem descarte silencioso.
- Grava 100% dos leads em `banco de dados` e na aba de segmentação **Pago** (classificação por
  funil pareado).
- Alimenta o **CRM comercial**: dedup por e-mail → `Telefone 1`, arquiva o registro antigo em
  `Archived Leads` e atualiza in-place, ou cria um novo — preenchendo a coluna **`Orgânico?`**
  (`sim`/`não`) conforme a origem do lead.
- **Recalcula MQL no servidor** (nunca confia no cliente) e usa **timestamp do servidor**.
- Captura tracking completo: `utm_source/medium/campaign/content/term/id`, `fbclid`, `src`, `sck`,
  `a_id`, `channel`, `referrer`, `page_url`.
- Registra descartes (inválido/duplicata) na aba `Descarte` e **notifica por e-mail** (Gmail).
- Erros reais caem no **Error Workflow** `Alerta de Erro - Leads`.

📄 Documentação detalhada da automação: a **Sticky Note "Documentação da automação"** dentro do
workflow `lead-tributario` no n8n, e — quando presente — o `automation/README.md` do projeto.

## Licença

A licença **permanece inalterada** e segue os termos proprietários definidos em [LICENSE](./LICENSE).
