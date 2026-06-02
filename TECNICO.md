# Lead Tributário — Documento Técnico

## Visão Geral

Página de captura de leads para o funil pago do curso "Tributário para Marketplaces". Complementa a LP orgânica (`LP-Tributario`) com rastreamento específico para campanhas pagas.

## Stack

| Camada | Tecnologia |
|--------|-----------|
| Frontend | HTML + CSS + JavaScript puro |
| Automação | n8n Webhook |
| Deploy | HostGator |

## Estrutura

```
lead-tributario/
├── index.html
├── assets/
├── fonts/
├── depoimentos/              # Imagens/vídeos de depoimentos
├── hostgator_upload_pago/    # Arquivos prontos para upload
├── DOCUMENTACAO.md           # Documentação detalhada do projeto
└── links-utms-facebook-google.md  (em hostgator_upload_pago)
```

## Deploy

```bash
# Fazer upload do conteúdo de hostgator_upload_pago/ para o domínio via FTP
```

## Referência cruzada

Ver também: `landing-pages/LP-Tributario/` — versão orgânica do mesmo funil.
