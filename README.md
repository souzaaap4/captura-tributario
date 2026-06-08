# Captura Tributário

> Landing page privada para captação e qualificação de leads para o curso de tributação para marketplaces — tráfego pago.

![Status](https://img.shields.io/badge/status-projeto%20privado-8b0000)
![Frontend](https://img.shields.io/badge/frontend-HTML%2FCSS%2FJS-1f6feb)
![Automação](https://img.shields.io/badge/automacao-n8n%20Webhook-0f766e)
![Deploy](https://img.shields.io/badge/deploy-HostGator%2FcPanel-7c3aed)

README de apresentação para GitHub.

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
- **Integração:** Webhook n8n (direto, sem proxy)
- **Deploy:** HostGator/cPanel — upload manual via `hostgator_upload_pago/`

## Arquitetura (Resumo)

| Camada | Responsabilidade |
| --- | --- |
| `index.html` | LP principal de desenvolvimento |
| `hostgator_upload_pago/` | Pacote pronto para upload em produção |
| `assets/` | Imagens, vídeos e ícones da LP |
| `fonts/` | Tipografia local (Sora) |
| `n8n-workflow-tributario.json` | Workflow de automação n8n exportado |
| `DOCUMENTACAO.md` | Documentação técnica completa |
| `TECNICO.md` | Guia técnico de manutenção |

## Funcionamento do Sistema

1. Usuário acessa a LP via anúncio pago.
2. Tracking first-touch inicializa e persiste UTMs.
3. Usuário preenche formulário de qualificação.
4. Aplicação calcula MQL e monta payload com dados + tracking.
5. Payload é enviado para o webhook n8n.
6. n8n deduplica por telefone consultando o banco de dados.
7. Lead é gravado nas abas **pago** e **banco de dados** do Google Sheets.

```mermaid
flowchart LR
    A[Acesso via Anúncio] --> B[Tracking UTM]
    B --> C[Preenchimento do formulário]
    C --> D[Cálculo de MQL]
    D --> E[Payload enviado ao webhook n8n]
    E --> F[Deduplicação por telefone]
    F --> G[Gravação na aba pago]
    F --> H[Gravação na aba banco de dados]
```

## Diferenciais de Engenharia

- Tracking first-touch desacoplado da lógica de formulário.
- Qualificação calculada no frontend antes do envio.
- Deduplicação server-side no n8n para evitar leads duplicados.
- Pacote de deploy isolado (`hostgator_upload_pago/`) para publicação direta.

## Estrutura do Projeto

```text
.
├─ assets/
│  ├─ 3.svg
│  └─ video-aula.mp4
├─ depoimentos/
├─ fonts/
│  └─ static/
├─ hostgator_upload_pago/
│  ├─ index.html
│  ├─ .htaccess
│  ├─ tributario-tay-assets/
│  ├─ tributario-tay-depoimentos/
│  └─ tributario-tay-fonts/
├─ index.html
├─ n8n-workflow-tributario.json
├─ DOCUMENTACAO.md
├─ TECNICO.md
├─ links-utms-facebook-google.md
└─ LICENSE
```

## Deploy

- **HostGator/cPanel:** upload manual do conteúdo de `hostgator_upload_pago/` via Gerenciador de Arquivos ou FTP.

## Licença

A licença **permanece inalterada** e segue os termos proprietários definidos em [LICENSE](./LICENSE).
