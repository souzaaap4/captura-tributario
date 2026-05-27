# Landing Page — Tributário para Marketplaces
## Método P4 | Documentação Técnica e de Design

---

## Índice

1. [Visão Geral do Projeto](#1-visão-geral-do-projeto)
2. [Estrutura de Arquivos](#2-estrutura-de-arquivos)
3. [Identidade Visual](#3-identidade-visual)
4. [Tipografia](#4-tipografia)
5. [Seções da Página](#5-seções-da-página)
6. [Formulário de Captação](#6-formulário-de-captação)
7. [Componentes Interativos](#7-componentes-interativos)
8. [Responsividade](#8-responsividade)
9. [Acessibilidade](#9-acessibilidade)
10. [SEO e Dados Estruturados](#10-seo-e-dados-estruturados)
11. [Changelog — Histórico de Alterações](#11-changelog--histórico-de-alterações)

---

## 1. Visão Geral do Projeto

**Objetivo:** Landing page de captação de leads para o curso "Tributário para Marketplaces" do Método P4.

**Instrutor:** Gabriel Pim  
**Produto:** Curso completo sobre tributação para vendedores em marketplaces (Mercado Livre, Shopee, Amazon).  
**Conversão desejada:** Inscrição via formulário modal — leads enviados para automação externa.

**Arquivo principal:** `index.html`  
**Tecnologia:** HTML + CSS + JavaScript vanilla (zero frameworks, zero dependências externas exceto Font Awesome CDN)

---

## 2. Estrutura de Arquivos

```
lead-cursoads/
│
├── index.html              → Página principal (tudo em arquivo único)
├── lp-tributario.png       → Foto do instrutor Gabriel Pim
├── Sora.zip                → Arquivo original da fonte
├── 3.svg                   → Paleta de cores da marca
├── DOCUMENTACAO.md         → Este arquivo
│
└── fonts/
    ├── Sora-VariableFont_wght.ttf
    └── static/
        ├── Sora-Light.ttf          (weight 300)
        ├── Sora-Regular.ttf        (weight 400)
        ├── Sora-Medium.ttf         (weight 500)
        ├── Sora-SemiBold.ttf       (weight 600)
        ├── Sora-Bold.ttf           (weight 700)
        ├── Sora-ExtraBold.ttf      (weight 800)
        ├── Sora-ExtraLight.ttf     (weight 200)
        └── Sora-Thin.ttf           (weight 100)
```

> **Nota:** A fonte Sora é carregada via `@font-face` apontando para os arquivos locais em `fonts/static/`. Isso garante funcionamento offline e sem dependência de CDN.

---

## 3. Identidade Visual

### Paleta de Cores (extraída de `3.svg`)

| Token CSS         | Valor Hex     | Uso                                              |
|-------------------|---------------|--------------------------------------------------|
| `--brand`         | `#56D44F`     | Cor principal da marca — CTAs, accents, ícones   |
| `--brand-light`   | `#82E57D`     | Hover de botões, gradiente claro                 |
| `--brand-dark`    | `#3AB534`     | Gradiente escuro de botões                       |
| `--brand-dim`     | `rgba(86,212,79,0.12)` | Fundo de badges, pill, ícones           |
| `--brand-border`  | `rgba(86,212,79,0.28)` | Bordas de destaque, rings                |
| `--bg-base`       | `#0E1520`     | Background principal da página                   |
| `--bg-surface`    | `#141D28`     | Background de seções alternadas                  |
| `--bg-card`       | `#1C242E`     | Background de cards — cor dark da marca          |
| `--bg-card-hover` | `#1F2A38`     | Estado hover dos cards                           |
| `--blue`          | `#4D90D6`     | Accent secundário — ícones de problemas          |
| `--text-1`        | `#EEE7DF`     | Texto principal — cream da marca                 |
| `--text-2`        | `#8A96A8`     | Texto secundário / subheadings                   |
| `--text-3`        | `#4A5568`     | Texto muted / labels / footer                    |
| `--border`        | `rgba(255,255,255,0.07)` | Bordas sutis de cards e divisores     |

### Gradientes de CTA
```css
/* Botão principal */
background: linear-gradient(135deg, #56D44F, #3AB534);

/* Hover */
background: linear-gradient(135deg, #82E57D, #56D44F);
```

---

## 4. Tipografia

**Fonte única:** `Sora` (Google Fonts / local)  
**Carregamento:** Via `@font-face` com arquivos locais em `fonts/static/`

| Uso                          | Peso     | Tamanho Desktop     | Tamanho Mobile     |
|------------------------------|----------|---------------------|--------------------|
| Headlines hero / H1          | 800      | `clamp(2.2rem, 5.2vw, 3.8rem)` | ~2rem |
| Títulos de seção             | 800      | `clamp(1.75rem, 3.2vw, 2.45rem)` | ~1.6rem |
| Título instrutor             | 800      | `2rem`              | `1.8rem`           |
| Números ticker / módulos     | 800      | `1.35rem / 2.3rem`  | —                  |
| Labels e subtítulos          | 700      | `0.98rem`           | —                  |
| Corpo de texto               | 300–400  | `1.02rem`           | `0.95rem`          |
| Labels de formulário         | 600      | `0.74rem`           | —                  |
| Eyebrow / seção label        | 700      | `0.68rem`           | —                  |

---

## 5. Seções da Página

### 5.1 — Navigation (Fixo no topo)

- **Comportamento:** Transparente ao carregar → blur + borda sutil ao rolar `> 40px`
- **Elementos:** Logo "MétodoP4", links de ancora para cada seção, CTA pill verde
- **Mobile:** Links ocultados → botão hambúrguer → drawer fullscreen com links e CTA

### 5.2 — Hero (Section principal)

- **Altura:** `min-height: 100vh` — ocupa toda a tela inicial
- **Background:** Dot-grid com máscara radial + 3 orbs de luz animados (verde, azul, verde escuro)
- **Elementos:**
  - Badge pill pulsante "Curso Completo"
  - H1 com highlight em gradiente verde
  - Subtítulo explicativo
  - Chip de preço com destaque para `R$97,00`
  - CTA primário com ícone de seta animado
  - Trust bar com 3 ícones (acesso imediato, sem cartão, certificado)

### 5.3 — Ticker Strip (Faixa de prova social)

- **Comportamento:** Scroll infinito animado via CSS (`@keyframes ticker`)
- **Pausa ao hover**
- **Dados:** +2.400 alunos, 6 módulos, conteúdo atualizado, R$97, Vitalício, +500 mentorados, 5★

### 5.4 — Problemas (Section dark)

- **Layout:** Grid 2 colunas — texto à esquerda, lista de problemas à direita
- **5 itens** com ícone em background azul-dim e texto explicativo
- **Callout** em fundo verde-dim com borda verde

### 5.5 — Módulos do Curso

- **Layout:** Grid 3×2 (6 cards)
- **Hover:** card sobe 5px, borda verde aparece, linha de gradiente no topo
- **Numeração** grande e translúcida como elemento decorativo de background
- **Conteúdo por módulo:**
  1. Simples Nacional
  2. Lucro Presumido e Lucro Real
  3. ICMS Substituição Tributária
  4. DRE e Formação de Preço
  5. Planejamento Tributário Legal
  6. Inteligência Fiscal para Marketplaces

### 5.6 — Como Funcionam as Aulas? (Section dark)

- **Layout:** Grid 2 colunas — features à esquerda, player à direita
- **Vídeo:** Player HTML5 nativo (`<video>`) com arquivo local `assets/video-aula.mp4` — aspect ratio 16:9 via `padding-top: 56.25%`
- **Thumbnail:** atributo `poster="assets/6c9bae32-d7b5-4352-9dbc-603d1d855d01.png"` nativo do browser
- **Efeito:** Glow verde sutil ao redor do player
- **Features listadas:**
  - Videoaulas gravadas — assista no seu ritmo
  - Módulos curtos e focados em resultado
  - Acesso em qualquer dispositivo, 24h
  - Revisão ilimitada

### 5.7 — Instrutor

- **Foto:** `lp-tributario.png` (Gabriel Pim) com `object-fit: cover`
- **Overlay:** Gradiente escuro na base da foto
- **Badge flutuante** com dot animado pulsante
- **Informações:** Nome, cargo, bio, 3 badge-pills de credenciais

### 5.8 — Depoimentos (Section dark)

- **Layout:** Grid 2×2 (4 cards)
- **Elementos por card:** 5 estrelas, aspas decorativas, texto do depoimento, avatar com iniciais, nome e cargo
- **Contador:** "+2.400 alunos já transformaram suas finanças" com linhas decorativas

### 5.9 — CTA / Pricing

- **Foco:** Conversão final — destaque total no formulário
- **Price box:** "Investimento: R$97,00" em verde
- **Checklist:** 5 benefícios com ícone check verde
- **Urgência:** "Vagas limitadas para manter a qualidade do suporte"

### 5.10 — FAQ (Accordion)

- **Layout:** Grid 2 colunas — título à esquerda, accordion à direita
- **5 perguntas** com animação de abertura suave via `max-height`
- **Ícone** rotaciona 45° ao abrir (+ → ×)
- **Teclado:** Suporte a Enter e Space

### 5.11 — Footer

- **Minimalista:** Logo, tagline, links legais, copyright

---

## 6. Formulário de Captação

### Campos do Formulário

| Campo                                    | Tipo      | Name attribute         | Obrigatório |
|------------------------------------------|-----------|------------------------|-------------|
| Nome Completo                            | `text`    | `nome`                 | ✅ Sim      |
| Qual o seu Whatsapp?                     | `tel`     | `whatsapp`             | ✅ Sim      |
| Confirme seu Whatsapp                    | `tel`     | `whatsapp_confirmacao` | ✅ Sim      |
| Qual seu e-mail?                         | `email`   | `email`                | ✅ Sim      |
| Qual é a sua medalha no Mercado Livre?   | `select`  | `nivel_ml`             | ✅ Sim      |
| Qual é o seu regime tributário atual?    | `select`  | `regime_tributario`    | ✅ Sim      |

### Opções — Medalha Mercado Livre

```
Não tenho medalha  (valor: sem_medalha)
Ouro               (valor: ouro)
Ouro Plus          (valor: ouro_plus)
Platinum           (valor: platinum)
```

### Opções — Regime Tributário

```
MEI                (valor: mei)
Simples Nacional   (valor: simples_nacional)
Lucro Presumido    (valor: lucro_presumido)
Lucro Real         (valor: lucro_real)
Não sei            (valor: nao_sei)
```

### Validações implementadas

- **WhatsApp:** Máscara automática `DD DDDDDDDDD` — formato sem código do país (ex: `19 987654321`)
- **Mínimo aceito:** 10 dígitos (cobre fixo 10 dígitos e celular 11 dígitos)
- **Placeholder:** `Ex: 19 987654321`
- **Confirmação de WhatsApp:** Validação em tempo real — erro visual se os números não baterem
- **HTML5 `required`** em todos os campos
- **`novalidate`** no `<form>` para controle customizado da UX

### Integração com Automação

O formulário usa `fetch` assíncrono — a página não recarrega, e o estado de sucesso aparece imediatamente após o envio. Para conectar um webhook:

**Passo único:** No arquivo `index.html`, localize a linha:

```js
const WEBHOOK_URL = '';
```

E substitua pela URL do seu webhook:

```js
// Make (Integromat)
const WEBHOOK_URL = 'https://hook.eu1.make.com/SEU_TOKEN';

// n8n
const WEBHOOK_URL = 'https://seu-n8n.com/webhook/SEU_ID';

// Zapier
const WEBHOOK_URL = 'https://hooks.zapier.com/hooks/catch/SEU_ID/';
```

**Payload enviado (JSON/form-data):**
```json
{
  "nome": "João Silva",
  "whatsapp": "(11) 99999-9999",
  "whatsapp_confirmacao": "(11) 99999-9999",
  "email": "joao@email.com",
  "nivel_ml": "platinum",
  "regime_tributario": "simples_nacional"
}
```

---

## 7. Componentes Interativos

### Modal de Inscrição
- Ativado por qualquer botão CTA da página
- Fundo: `backdrop-filter: blur(14px)` com overlay escuro
- Animação: scale + translateY ao abrir
- Fecha ao clicar fora, pressionar ESC, ou botão ×
- Estado de sucesso: mostra confirmação com ícone animado

### Scroll Reveal
- Implementado via `IntersectionObserver` (nativo — sem biblioteca)
- Classe `.reveal` → `.reveal.in` quando elemento entra em viewport
- Delays escalonados: `.d1` (100ms) → `.d6` (500ms)
- Todos os cards, títulos e listas de conteúdo têm a animação

### FAQ Accordion
- Abertura suave via `max-height` CSS transition
- Só um item aberto por vez
- Acessível: role="button", tabindex="0", aria-expanded

### Ticker
- CSS `@keyframes` puro — zero JS
- Pausa ao hover (`animation-play-state: paused`)
- Duplicação de conteúdo para loop seamless

### Nav Scroll
- `window.addEventListener('scroll')` com `{ passive: true }`
- Adiciona classe `.solid` após 40px de scroll

---

## 8. Responsividade

| Breakpoint       | Mudanças principais                                               |
|------------------|-------------------------------------------------------------------|
| `≤ 1060px`       | Módulos: 3→2 colunas; Instrutor: coluna mais estreita; Video: 1 coluna |
| `≤ 768px`        | Nav: hamburger; Problemas/Instructor/Testimonials: 1 coluna; Price box: vertical |
| `≤ 480px`        | Hero H1 reduzido; Price chip empilhado; Hero trust vertical       |

---

## 9. Acessibilidade

- `role` e `aria-label` em navegação, dialog, botões
- `aria-expanded` no accordion e no hamburger
- `aria-hidden="true"` em elementos decorativos
- `aria-live="polite"` no estado de sucesso do formulário
- Suporte completo a navegação por teclado (Tab, Enter, Space, Escape)
- `alt` descritivo na foto do instrutor
- `loading="lazy"` em imagens e iframe
- `font-display: swap` nas fontes para evitar FOIT

---

## 10. SEO e Dados Estruturados

### Meta tags implementadas

| Tag | Valor |
|-----|-------|
| `<title>` | Tributário para Marketplaces — Curso Completo \| Método P4 |
| `<meta description>` | Aprenda tributação para marketplaces. Simples Nacional, ICMS-ST, Lucro Real e muito mais... |
| `<link rel="canonical">` | `https://tributario.metodop4.com.br/` |
| `<meta name="robots">` | `index, follow` |
| `<meta name="author">` | Método P4 |
| `lang="pt-BR"` | No elemento `<html>` |

### Open Graph (Facebook / WhatsApp / LinkedIn)

```html
<meta property="og:type"        content="website">
<meta property="og:url"         content="https://tributario.metodop4.com.br/">
<meta property="og:title"       content="...">
<meta property="og:description" content="...">
<meta property="og:image"       content="...">  <!-- 1200×630 -->
<meta property="og:locale"      content="pt_BR">
<meta property="og:site_name"   content="Método P4">
```

### Twitter Card

```html
<meta name="twitter:card"        content="summary_large_image">
<meta name="twitter:title"       content="...">
<meta name="twitter:description" content="...">
<meta name="twitter:image"       content="...">
```

### JSON-LD — Dado Estruturado

**Tipo:** `Course` (Schema.org)

```json
{
  "@context": "https://schema.org",
  "@type": "Course",
  "name": "Tributário para Marketplaces",
  "description": "Curso sobre tributação para vendedores em marketplaces...",
  "provider": {
    "@type": "Organization",
    "name": "Método P4",
    "url": "https://tributario.metodop4.com.br/",
    "logo": "https://tributario.metodop4.com.br/assets/01%20(1).png",
    "sameAs": [
      "https://www.instagram.com/metodop4/",
      "https://www.linkedin.com/company/m%C3%A9todo-p4/",
      "https://www.youtube.com/@gabriel_pim"
    ]
  },
  "instructor": {
    "@type": "Person",
    "name": "Gabriel Pim",
    "jobTitle": "Especialista em E-commerce e Tributação para Marketplaces"
  },
  "educationalLevel": "Beginner",
  "inLanguage": "pt-BR",
  "url": "https://tributario.metodop4.com.br/"
}
```

**O que esse dado estruturado faz:** permite ao Google exibir o curso em resultados enriquecidos (rich results) de busca — incluindo nome, instrutor e disponibilidade diretamente na SERP.

**Sugestão futura:** adicionar `FAQPage` para a seção de perguntas frequentes — amplia a presença no Google com accordion de respostas diretamente nos resultados.

---

## 11. Changelog — Histórico de Alterações

### v2.0 — Refatoração completa (2025-05)

#### Design
- [x] Página inteira reconstruída do zero em arquivo único
- [x] Paleta de cores substituída para identidade da marca: verde `#56D44F` + dark navy `#1C242E`
- [x] Tipografia migrada de Inter/Syne+DM Sans para **Sora** (fontes locais)
- [x] Hero redesenhado: dot-grid animado + orbs de luz + badge pulsante
- [x] Ticker strip de prova social com animação CSS infinita
- [x] Cards de módulos com numeração decorativa e hover com linha-topo verde
- [x] Seção de instrutor com foto real `lp-tributario.png`, overlay gradiente e badge flutuante
- [x] Seção de depoimentos em grid 2×2 (substituiu carousel genérico)
- [x] FAQ com accordion acessível e animação suave

#### Funcionalidades novas
- [x] **Seção "Como funcionam as aulas?"** com embed Vimeo `800389026` responsivo
- [x] **Modal de inscrição** com backdrop blur, animação de entrada e estado de sucesso
- [x] **Scroll reveal** via IntersectionObserver com delays escalonados
- [x] Nav sticky com blur ao scroll
- [x] Menu mobile com drawer fullscreen
- [x] Botão WhatsApp flutuante fixo
- [x] Máscara de telefone no campo WhatsApp

#### Formulário
- [x] Campo "Confirme seu Whatsapp" adicionado
- [x] Validação de correspondência entre WhatsApp e confirmação
- [x] `name` attributes padronizados para integração com automação
- [x] Estrutura preparada para webhook (Make, n8n, Zapier)
- [x] Botão "ENVIAR" em destaque verde
- [x] Estado de sucesso com ícone animado após envio

#### Correções em relação à versão original
- [x] Hierarquia visual corrigida — fluxo de leitura claro
- [x] Espaçamento padronizado (`--py: 120px` desktop, `70px` mobile)
- [x] Responsividade totalmente refeita (mobile-first)
- [x] Poluição visual eliminada — cards desnecessários removidos
- [x] Percepção de valor elevada com depth effects, glows e glassmorphism

### v2.1 — Atualização Visual Estrutural (2026-05-21)

#### Escopo
- [x] Reposicionamento e limpeza visual da Hero principal
- [x] Substituição da marca textual por ícone (`assets/01 (1).png`) no navbar e footer
- [x] Favicon e apple-touch-icon atualizados para `assets/01 (1).png`
- [x] Intercalação de seções em blocos azul/branco para leitura em camadas
- [x] Redesign completo do footer com colunas de navegação, contato e redes sociais

#### Detalhamento técnico aplicado (`index.html`)
- Hero:
  - `background-image` passou a usar `assets/lp-tributario.png`
  - remoção da textura de linhas da Hero (`.hero::before { background: none; }`)
  - remoção do card de texto da Hero (`.hero .reveal` sem fundo, borda e sombra)
  - alinhamento do conteúdo para esquerda (`.hero-grid { justify-items: start; }`)
- Logos de marketplaces:
  - substituição de labels textuais por logos reais (ML, Shopee, Shein, Magalu, Amazon e TikTok)
- Sistema de seções:
  - alternância de fundos entre blocos claros e escuros para separar melhor contexto e reduzir fadiga visual
- Footer:
  - estrutura migrada de bloco minimalista para grid em 4 colunas (`marca`, `serviços`, `recursos`, `contato`)
  - adição de ícones sociais e barra inferior com links legais
  - manutenção de estilo próprio (inspirado em referência externa, sem reprodução literal)

### v2.2 — Ajuste Fino Solicitado (2026-05-21)

#### Solicitação atendida
- [x] Mover a imagem da Hero "um pouco mais para a direita"
- [x] Aumentar os ícones de marketplaces
- [x] Arredondar as pontas dos ícones
- [x] Registrar detalhadamente esta execução no `DOCUMENTACAO.md`

#### Alterações exatas realizadas (`index.html`)
- Hero (fundo):
  - Desktop: `.hero { background-position: 84% center; }`
  - Tablet: `@media (max-width: 900px) .hero { background-position: 80% center; }`
  - Mobile: `@media (max-width: 768px) .hero { background-position: 76% center; }`
- Logos de marketplaces:
  - Desktop: `.hero-logo-pill img { height: 30px; border-radius: 8px; }`
  - Mobile/Tablet: `@media (max-width: 768px) .hero-logo-pill img { height: 24px; border-radius: 7px; }`
  - Os "cards" de fundo das logos permanecem removidos (`background: none; border: none; box-shadow: none`)

#### Resultado esperado
- A face do instrutor fica mais deslocada para a direita, liberando área útil de leitura à esquerda.
- As logos dos marketplaces ficam visualmente identificáveis sem exigir zoom/atenção extra.
- O arredondamento reduz ruído visual e harmoniza com o restante da linguagem da página.

### v2.3 — Redução de Glow + Limpeza de Fundo + Escala da Seção Lojista (2026-05-21)

#### Solicitação atendida
- [x] Diminuir o glow verde global da landing page
- [x] Remover os "riscos" diagonais do fundo em todas as seções
- [x] Aumentar visualmente a seção "Feito de Lojista para Lojista"

#### Alterações exatas realizadas (`index.html`)
- Glow reduzido nos principais componentes:
  - Botões: `.btn-green` e `.btn-green:hover` com opacidades de sombra menores
  - CTA da navegação: `.nav-cta` com sombra reduzida
  - Overlay do menu mobile: `.mobile-nav` com radial verde mais suave
  - Hero: `.hero::after` com menor intensidade e menor blur
  - Conteúdo/Preço: `.conteudo`, `.check-icon`, `.price-card::before` com glow atenuado
  - Vídeo: `.video-sec` e `.video-wrap` com menor halo verde
  - Instrutor: `.instrutor-sec` e `.inst-photo-badge` com sombra verde reduzida
  - Footer: radiais verdes do `footer` suavizados
- Remoção dos riscos diagonais:
  - `.lojista::before` alterado para `content: none;` (elimina o `repeating-linear-gradient`)
- Aumento da seção "Lojista":
  - Desktop:
    - `.lojista` padding aumentado para `112px 0 124px`
    - `.lojista-grid` min-height aumentado para `620px`
    - `.lojista-grid` colunas ampliadas para melhor presença visual
    - `.lojista-media` min-height aumentado para `440px`
    - `.lojista-content` ampliado (`max-width`, `padding` e sobreposição)
  - Responsivo:
    - `@media (max-width: 900px)` com seção mais alta e bloco de conteúdo maior
    - `@media (max-width: 480px)` com imagem e card maiores para manter legibilidade

#### Resultado esperado
- Visual menos "estourado" em verde, mantendo identidade da marca sem excesso de glow.
- Fundo mais limpo e sofisticado, sem interferência de linhas diagonais.
- Seção "Lojista" com presença mais forte e leitura mais confortável.

### v2.4 — Refino Visual da Seção “Feito de Lojista para Lojista” (2026-05-21)

#### Solicitação atendida
- [x] Melhorar o visual geral da seção (imagem + card + hierarquia do texto)

#### Alterações exatas realizadas (`index.html`)
- Bloco da seção:
  - `.lojista` com radial reposicionado para equilíbrio visual (menos concentração no canto esquerdo)
  - `.lojista-grid` com `gap` maior para respiro entre imagem e card
- Imagem lateral (`.lojista-media`):
  - sobreposição verde intensa substituída por composição mais elegante:
    - gradiente escuro de contraste
    - brilho verde localizado (radial), sem “chapar” toda a foto
  - cantos mais arredondados (`34px`)
  - sombra aprofundada para melhorar separação do fundo
- Card de conteúdo (`.lojista-content`):
  - cartão ampliado e com sobreposição mais refinada (`margin-left` ajustado)
  - fundo claro com leve glass effect e sombra mais premium
  - tipografia e espaçamento melhorados para leitura
  - label “Sobre o Curso” ganhou marcador visual (dot) para reforço de hierarquia
  - CTA com largura mínima em desktop e 100% no mobile
- Responsivo:
  - tablet/mobile com ajuste de alturas, overlap e paddings para manter proporção da composição
- Copy:
  - texto da seção reescrito para ficar mais direto e natural, mantendo o mesmo contexto

#### Resultado esperado
- Seção com aparência mais profissional e menos “neon”.
- Melhor legibilidade e foco no conteúdo sem perder a identidade visual verde/azul.
- Composição mais equilibrada em desktop e mais consistente em mobile.

### v2.5 — Ajuste da Seção “O que você vai aprender” + Redesign do Card de Preço (2026-05-21)

#### Solicitação atendida
- [x] Ajustar visual da seção “O que você vai aprender”
- [x] Ajustar card de preço com hierarquia mais forte e visual de oferta

#### Alterações exatas realizadas (`index.html`)
- Bloco “O que você vai aprender” (`.conteudo-left`):
  - card com novo acabamento (fundo, borda, raio e sombra refinados)
  - label com marcador visual (dot) para reforço de hierarquia
  - título ajustado para leitura mais direta: “Garanta sua vaga e aprenda, na prática:”
  - subtítulo de apoio adicionado (`.conteudo-sub`)
  - lista de tópicos convertida para itens em mini-cards (`.check-item`) com melhor escaneabilidade
- Card de preço (`.price-card`):
  - visual refeito para referência de oferta com:
    - “De: R$97,00” em destaque laranja com risco forte
    - “por” em destaque intermediário
    - valor `R$97,00` com peso máximo
    - selo de destaque lateral (`.price-badge`) em formato burst
  - CTA principal em formato pill e largura total do card
  - selos de confiança reestruturados com ícone + texto:
    - Compra Segura
    - Satisfação Garantida
    - Privacidade Protegida
- Responsivo:
  - tablet com redução de paddings/fontes do card de preço
  - mobile com ajuste do valor, posição do burst e selos de confiança em coluna

#### Resultado esperado
- Seção de conteúdo com leitura mais clara e percepção de organização superior.
- Card de preço com hierarquia de conversão mais forte (foco no valor “R$97,00”).
- Melhor consistência visual com o restante da landing em desktop e mobile.

### v2.6 — Simplificação de Cards + Evidência Máxima no Card de Valor (2026-05-21)

#### Solicitação atendida
- [x] Reduzir excesso de cards visuais na seção de aprendizado
- [x] Dar mais evidência para o card de valor

#### Alterações exatas realizadas (`index.html`)
- Seção “O que você vai aprender”:
  - `.conteudo-left` simplificado (sem card container: removidos fundo, borda e sombra)
  - itens da lista convertidos para linhas limpas com divisor inferior
  - mantidos apenas ícone + texto para leitura rápida e menos ruído visual
- Card de valor:
  - `reveal d2` recebeu classe `price-col` para tratamento dedicado
  - `.price-card` ganhou borda verde mais forte, escala (`transform: scale(1.05)` em desktop) e sombra mais profunda
  - adição de selo superior “Curso Completo” (`.price-tag`)
  - reforço de brilho interno com `::before` e `::after` para destacar o bloco sem poluir o layout
  - valor `R$97,00` ampliado para maior impacto visual
- Responsivo:
  - em `<= 900px`, `<= 768px` e `<= 480px` a escala do card volta para `none` para evitar quebra

#### Resultado esperado
- Menos sensação de interface “card sobre card”.
- Área esquerda mais limpa e editorial.
- Card de preço claramente dominante na seção, com foco de conversão.

### v2.7 — Depoimentos Reais via Pasta Local (2026-05-21)

#### Solicitação atendida
- [x] Usar a pasta local `depoimentos` como fonte dos depoimentos exibidos

#### Alterações exatas realizadas (`index.html`)
- Seção `#depoimentos`:
  - os 5 mocks de conversa em HTML foram substituídos por imagens reais:
    - `depoimentos/1.webp`
    - `depoimentos/2.webp`
    - `depoimentos/3.webp`
    - `depoimentos/4.webp`
    - `depoimentos/5.webp`
  - carrossel e setas de navegação foram mantidos
- CSS:
  - adicionada classe `.depo-shot` para renderizar as imagens no frame do celular

#### Resultado esperado
- Prova social mais autêntica (prints reais) e manutenção da mesma usabilidade do slider.

### v2.8 — Atualização de Redes e Contato Oficial (2026-05-21)

#### Solicitação atendida
- [x] Atualizar Instagram, LinkedIn e YouTube
- [x] Atualizar telefone e e-mail de contato

#### Alterações exatas realizadas (`index.html`)
- Footer (`.foot-social`):
  - Instagram: `https://www.instagram.com/metodop4/`
  - LinkedIn: `https://www.linkedin.com/company/m%C3%A9todo-p4/posts/?feedView=all`
  - YouTube: `https://www.youtube.com/@gabriel_pim`
  - WhatsApp: `https://wa.me/5519997258140`
  - todos os links sociais com `target="_blank"` e `rel="noopener"`
- Footer (`Contato`):
  - e-mail alterado para `adm@methoz.com.br`
  - telefone alterado para `(19) 99725-8140` com link `tel:+5519997258140`
- Botão flutuante de WhatsApp:
  - link atualizado para `https://wa.me/5519997258140`

#### Resultado esperado
- Canais oficiais consistentes em todo o site e contato direto funcionando corretamente.

### v2.9 — Padronização de Tamanho dos Telefones nos Depoimentos (2026-05-21)

#### Solicitação atendida
- [x] Padronizar o tamanho visual de todos os “telefones” da seção de depoimentos

#### Alterações exatas realizadas (`index.html`)
- Ajuste em `.phone-screen`:
  - adicionada proporção fixa `aspect-ratio: 9 / 19.5`
- Ajuste em `.depo-shot`:
  - `height: 100%`
  - `object-fit: cover`
  - `object-position: center top`

#### Resultado esperado
- Todos os prints de depoimento passam a ocupar o mesmo frame com altura e largura consistentes no carrossel.

### v2.10 — Seção do Instrutor sem Card Branco + Fundo com Foto do Gabriel (2026-05-21)

#### Solicitação atendida
- [x] Remover card branco da seção “Conheça seu instrutor / Quem é o Gabriel Pim”
- [x] Usar imagem `assets/1ec33064-2c8b-440d-8100-d32fc455a038.png` como fundo da seção

#### Alterações exatas realizadas (`index.html`)
- `.instrutor-sec`:
  - novo `background-image` com overlay escuro + imagem do Gabriel
  - `background-size: cover`, `background-position` ajustado e `padding` atualizado
- `.instrutor-grid`:
  - removido visual de card (`background`, `border`, `radius`, `shadow`)
  - convertido para bloco limpo com largura máxima para leitura
- Tipografia e chips:
  - textos do instrutor convertidos para paleta clara (alto contraste sobre imagem)
  - credenciais (`.inst-cred`) ajustadas para visual translúcido sobre fundo escuro
- HTML:
  - removida a coluna da foto interna (`.inst-photo-wrap`) para evitar duplicação da imagem e manter a seção mais limpa
- Responsivo:
  - ajustes de `background-position` e espaçamento em breakpoints menores

#### Resultado esperado
- Seção do instrutor integrada ao visual da landing, sem “bloco branco”.
- Maior impacto visual com a foto de fundo e leitura preservada por overlay escuro.

### v2.11 — Seção do Instrutor Maior + Imagem Mais à Direita + Texto à Esquerda (2026-05-21)

#### Solicitação atendida
- [x] Aumentar a seção do instrutor
- [x] Mover a imagem de fundo mais para a direita
- [x] Deixar o texto mais alinhado à esquerda

#### Alterações exatas realizadas (`index.html`)
- `.instrutor-sec`:
  - `background-position` desktop ajustado para `84% center`
  - `padding` aumentado para `140px 0`
  - `min-height` definido em `760px`
  - `display: flex` e `align-items: center` para melhor distribuição vertical
- `.instrutor-grid`:
  - largura máxima reduzida para `700px`
  - `margin-right: auto` para manter o bloco textual ancorado à esquerda
- Responsivo:
  - `<= 900px`: `background-position: 78% center`, `padding: 112px 0`, `min-height: 680px`
  - `<= 480px`: `background-position: 74% center`, `min-height: 620px`

#### Resultado esperado
- Seção visualmente mais ampla, com melhor enquadramento da imagem.
- Conteúdo textual mais confortável e claramente posicionado à esquerda.

### v2.12 — Aumento Extra da Seção do Instrutor + Reenquadramento (2026-05-21)

#### Solicitação atendida
- [x] Aumentar ainda mais a seção
- [x] Posicionar a imagem mais para a direita
- [x] Reforçar o texto no lado esquerdo

#### Alterações exatas realizadas (`index.html`)
- Desktop (`.instrutor-sec`):
  - `background-position` alterado para `91% center`
  - `padding` ampliado para `170px 0`
  - `min-height` ampliado para `880px`
- Bloco textual (`.instrutor-grid`):
  - `max-width` ajustado para `760px`
  - texto ancorado à esquerda com `margin-right: auto` e `margin-left: 0`
- Responsivo:
  - `<= 900px`: `background-position: 84% center`, `padding: 132px 0`, `min-height: 760px`
  - `<= 480px`: `background-position: 80% center`, `padding: 116px 0`, `min-height: 700px`

#### Resultado esperado
- Seção mais imponente visualmente, com o retrato mais deslocado para a direita e área de leitura mais confortável à esquerda.

### v2.13 — Capa Personalizada no Vídeo (2026-05-21)

#### Solicitação atendida
- [x] Definir a imagem `assets/6c9bae32-d7b5-4352-9dbc-603d1d855d01.png` como capa do vídeo

#### Alterações exatas realizadas (`index.html`)
- Seção de vídeo:
  - adicionada camada `.video-cover` com imagem de capa e botão play central
  - iframe Vimeo passa a iniciar oculto (`src="about:blank"`) e recebe URL real via `data-src`
  - autoplay ativado no clique da capa
- CSS:
  - novas classes para capa e botão (`.video-cover`, `.video-cover-play`)
  - transição de opacidade no iframe com estado `.video-wrap.playing`
- JavaScript:
  - novo bloco `VIDEO COVER` para carregar o iframe no clique e ocultar a capa

#### Resultado esperado
- O usuário vê a capa personalizada antes da reprodução e inicia o vídeo com um clique.

### v2.14 — Play Menor + Footer Alinhado (2026-05-21)

#### Solicitação atendida
- [x] Diminuir o botão de play da capa do vídeo
- [x] Melhorar e alinhar o footer

#### Alterações exatas realizadas (`index.html`)
- Vídeo (capa):
  - `.video-cover-play` reduzido para `62x62` (desktop)
  - ícone interno reduzido para `1.04rem`
  - tamanhos menores também aplicados em responsivo (`58x58` no tablet e `52x52` no mobile)
- Footer:
  - layout principal refinado para duas colunas mais equilibradas
  - bloco de contato alinhado à direita no desktop e à esquerda no mobile
  - espaçamentos e gaps ajustados para leitura mais limpa
  - linha de copyright centralizada no desktop e alinhada à esquerda no mobile

#### Resultado esperado
- Capa de vídeo com botão mais discreto e proporcional.
- Footer com alinhamento consistente em desktop e mobile.

### v2.15 — Reenquadramento Fino da Seção do Instrutor (2026-05-21)

#### Solicitação atendida
- [x] Melhorar a seção do instrutor
- [x] Alinhar mais o bloco textual à esquerda
- [x] Deslocar a imagem de fundo mais para a direita para não cobrir o rosto

#### Alterações exatas realizadas (`index.html`)
- `.instrutor-sec`:
  - overlay recalibrado para reforçar legibilidade no lado esquerdo
  - `background-position` desktop alterado para `100% center`
- `.instrutor-sec .wrap`:
  - largura máxima ampliada para `1320px`
  - paddings laterais personalizados para ancorar o conteúdo mais à esquerda
- `.instrutor-grid`:
  - largura reduzida para `max-width: 620px` (desktop) para evitar invasão da área da imagem
  - no breakpoint `<=900px`, limitado para `max-width: 580px`
- Texto e chips:
  - bio limitada para `max-width: 50ch`
  - bloco de credenciais limitado para `max-width: 560px`
- Responsivo:
  - `<=900px`: fundo em `90% center`
  - `<=480px`: fundo em `88% center`

#### Resultado esperado
- Texto mais confortável à esquerda, sem cobrir a face do instrutor.
- Melhor equilíbrio entre leitura e imagem de fundo.

### v2.16 — Novo Logo no Footer (`assets/03.png`) + Refino Visual (2026-05-21)

#### Solicitação atendida
- [x] Trocar logo do footer para `assets/03.png`
- [x] Melhorar visual e alinhamento do footer

#### Alterações exatas realizadas (`index.html`)
- Footer (estrutura visual):
  - bloco superior (`.footer-top`) com borda suave, fundo translúcido e cantos arredondados
  - espaçamentos, gaps e paddings recalibrados para leitura mais limpa
  - logo ampliado para melhor presença visual
- Footer (conteúdo):
  - logo do footer atualizado para `assets/03.png`
  - adicionado subtítulo curto: “Tributário para Marketplaces”
  - contatos com ícones (`envelope` e `telefone`) para leitura rápida
- Footer (responsivo):
  - desktop: contato alinhado à direita
  - mobile: layout em coluna com alinhamento à esquerda

#### Resultado esperado
- Rodapé com aparência mais profissional, melhor hierarquia e consistência visual com o restante da landing.

### v2.17 — Footer Sem Card (2026-05-21)

#### Solicitação atendida
- [x] Remover aparência de card no footer

#### Alterações exatas realizadas (`index.html`)
- `.footer-top`:
  - removidos `padding` interno, `border-radius`, `border` e `background`
  - mantido apenas o grid/alinhamento das colunas
- responsivo (`<=768px`):
  - removido `padding` do `.footer-top` para manter o rodapé sem caixa em mobile também

#### Resultado esperado
- Footer com visual limpo, sem bloco/cartão ao redor dos conteúdos.

### v2.18 — Instrutor Mais à Direita (Lado do WhatsApp) (2026-05-21)

#### Solicitação atendida
- [x] Deslocar a imagem de fundo da seção do instrutor mais para a direita
- [x] Manter o bloco textual livre no lado esquerdo
- [x] Registrar alteração completa na documentação

#### Alterações exatas realizadas (`index.html`)
- `.instrutor-sec` (desktop):
  - `background-position` alterado de `100% center` para `110% center`
- Responsivo `<=900px`:
  - `background-position` alterado de `90% center` para `102% center`
- Responsivo `<=480px`:
  - `background-position` alterado de `88% center` para `100% center`

#### Resultado esperado
- A figura do instrutor fica mais deslocada para a direita (próxima ao lado do botão flutuante de WhatsApp), com menor interferência sobre o conteúdo textual da esquerda.

### v2.19 — Revisão de Textos (Copy) nas Seções Principais (2026-05-21)

#### Solicitação atendida
- [x] Corrigir texto da seção **Sobre o Curso**
- [x] Corrigir texto da seção **O que você vai aprender**
- [x] Corrigir texto da seção **Quem é o Gabriel Pim**
- [x] Atualizar a documentação com o detalhamento das mudanças

#### Alterações exatas realizadas (`index.html`)
- **Sobre o Curso** (`.lojista-p`):
  - texto atualizado para:
  - *As aulas foram feitas pensando em oferecer, de forma prática e objetiva, tudo o que um EMPRESÁRIO de E-commerce deve saber sobre tributação para seu negócio. Sem "juridiquês" ou "contabilês".*
- **O que você vai aprender**:
  - título alterado de *“Garanta sua vaga e aprenda, na prática:”* para *“Garanta sua vaga e aprenda sobre:”*
  - itens da lista ajustados para:
  - Simples Nacional
  - Lucro Real e Lucro Presumido
  - ICMS ST
  - Sistemática de débito e crédito
  - DRE e Precificação
  - Etc.
- **Quem é o Gabriel Pim** (`.inst-bio`):
  - biografia substituída pela nova versão em 3 parágrafos:
  - especialização prática no Mercado Livre
  - idealização de metodologias com foco em negócios lucrativos
  - operação de loja própria + mentorias com estratégias testadas

#### Resultado esperado
- Mensagem mais clara e alinhada com o posicionamento comercial da landing, com linguagem direta e consistente entre as seções.

### v2.20 — Reenquadramento da Seção do Instrutor (Sessão 2026-05-21 cont.)

#### Solicitações atendidas
- [x] Mover imagem de fundo mais para a direita
- [x] Texto não cobrir o Gabriel
- [x] Largura da seção aumentada
- [x] Espaço entre "Conheça seu instrutor" e "Quem é o Gabriel Pim"
- [x] Seção com mesmo tamanho da Hero (100vh)
- [x] Logo não ser cortada — ajuste do eixo vertical da imagem

#### Alterações exatas realizadas (`index.html`)
- `.instrutor-sec`:
  - `background-position` ajustado progressivamente até `260% 40%` para mostrar Gabriel à direita sem cortar logo
  - `min-height: 100vh` e `padding: 124px 0 92px` — igual ao hero
  - gradiente: dark na esquerda, transparente na direita
  - `display: flex` corrigido (estava `display:center` por erro)
- `.instrutor-sec .wrap`:
  - padding personalizado: `clamp(20px, 5vw, 84px)` esquerda, `clamp(20px, 2.4vw, 34px)` direita
- `.instrutor-grid`:
  - `max-width` aumentado para `580px`
  - `margin-left: 0; margin-right: auto` — texto ancorado à esquerda
- `.inst-label`:
  - `display: block` e `margin-bottom: 14px`
- `.inst-h`:
  - `margin-top: 16px` para criar espaço visual entre label e título
- Responsivo `<=900px` e `<=480px`:
  - `background-position: 260% 40%`, `min-height: 100vh`, padding ajustado

### v2.21 — Destaque da Seção de Vídeo (2026-05-21)

#### Solicitação atendida
- [x] Adicionar destaque visual à seção "Como Funcionam as Aulas?"

#### Alterações exatas realizadas (`index.html`)
- `.video-sec`:
  - background refeito com dupla camada de radial verde e gradiente azul-claro mais pronunciado
  - resultado: fundo com maior contraste e profundidade, sem escurecer a seção
- `.video-wrap`:
  - `border` aumentada de `2px` para `2.5px` com opacidade verde mais alta (`0.72`)
  - `border-radius` aumentado de `20px` para `22px`
  - `box-shadow` refeita com 4 camadas:
    - sombra de profundidade (`0 28px 64px`)
    - primeiro anel verde difuso (`0 0 0 8px`)
    - segundo anel verde mais suave (`0 0 0 18px`)
    - glow externo verde (`0 0 80px 10px`) para efeito de destaque

#### Resultado esperado
- O player de vídeo se destaca claramente na seção, com aura verde e profundidade visual.
- Fundo da seção mais rico e consistente com a identidade da marca.

### v2.22 — Clareamento do Overlay da Seção do Instrutor (2026-05-21)

#### Solicitações atendidas
- [x] Clarear o fundo escuro da seção "Conheça seu instrutor / Quem é o Gabriel Pim" (duas rodadas de ajuste)

#### Alterações exatas realizadas (`index.html`)
- `.instrutor-sec` — `background-image` (camada de gradiente):
  - Valores anteriores: `rgba(7,14,26,.95) 0%` / `.88` 34% / `.48` 54% / `.18` 100%
  - Após 1ª rodada: `.78` / `.68` / `.30` / `.08`
  - Após 2ª rodada (versão final): `.58` / `.46` / `.16` / `.02`

#### Resultado esperado
- A foto do Gabriel aparece mais iluminada e viva, sem perder a legibilidade do texto no lado esquerdo.
- O lado direito da seção fica praticamente sem overlay, exibindo a imagem em alta fidelidade.

### v2.23 — SEO Completo + Correção de URLs de Navegação (2026-05-21)

#### Solicitações atendidas
- [x] Análise SEO e implementação das melhorias
- [x] Corrigir links de navegação para não exibir `/index.html#` na URL

#### Diagnóstico SEO (estado anterior)
| Item | Status Anterior | Status Novo |
|------|----------------|-------------|
| `<title>` | ✅ Presente | ✅ Mantido |
| `<meta description>` | ✅ Presente | ✅ Mantido |
| `lang="pt-BR"` | ✅ Presente | ✅ Mantido |
| `alt` em imagens | ✅ Presente | ✅ Mantido |
| `loading="lazy"` | ✅ Presente | ✅ Mantido |
| H1 único | ✅ Presente | ✅ Mantido |
| Canonical URL | ❌ Ausente | ✅ Adicionado |
| Open Graph (og:) | ❌ Ausente | ✅ Adicionado |
| Twitter Card | ❌ Ausente | ✅ Adicionado |
| JSON-LD Structured Data | ❌ Ausente | ✅ Adicionado |
| `meta robots` | ❌ Ausente | ✅ Adicionado |
| `meta author` | ❌ Ausente | ✅ Adicionado |
| `rel="noreferrer"` externo | ❌ Incompleto | ✅ Corrigido |
| Logo `href="#"` | ⚠️ Gerava `#` na URL | ✅ Corrigido para `#hero` |

#### Alterações exatas realizadas (`index.html`)
- `<head>`:
  - `<link rel="canonical" href="https://tributario.methoz.com.br/">`
  - Open Graph: `og:type`, `og:url`, `og:title`, `og:description`, `og:image` (1200×630), `og:locale`, `og:site_name`
  - Twitter Card: `summary_large_image`, title, description, image
  - `<meta name="robots" content="index, follow">`
  - `<meta name="author" content="Método P4">`
  - JSON-LD `@type: Course` com: name, description, provider (Organization com sameAs), instructor (Person), offers (price: 0, BRL), educationalLevel, inLanguage
- Navegação:
  - Logo: `href="#"` → `href="#hero"` (evita `/index.html#` ou URL suja)
  - Links externos: `rel="noopener"` → `rel="noopener noreferrer"` em todos

#### Observação
O domínio utilizado nas tags é `https://tributario.methoz.com.br/`. Se o domínio final for diferente, atualizar as tags `og:url`, `og:image`, `twitter:image`, `canonical` e os campos `url` do JSON-LD.

### v2.24 — Automação n8n + Captura de UTMs (2026-05-21)

#### Solicitações atendidas
- [x] Workflow n8n completo para o funil "Tributario"
- [x] Captura de UTMs e rastreamento na LP
- [x] Funil fixo em `"Tributario"` no Parse Body
- [x] Payload da LP atualizado para incluir UTMs

---

#### Arquivo gerado
`n8n-workflow-tributario.json` — colar diretamente no n8n via **Import workflow**

---

#### Fluxo da automação

```
[Webhook POST /Tributario] → [Parse Body (JS)] → [Google Sheets — append]
```

| Nó | Tipo | Função |
|----|------|--------|
| Webhook Tributario | `n8n-nodes-base.webhook` | Recebe POST da LP no path `/Tributario` |
| Parse Body | `n8n-nodes-base.code` | Normaliza campos da LP + injeta data BR e Funil fixo |
| Append row in sheet | `n8n-nodes-base.googleSheets` | Grava na aba **Tributário** da planilha |

---

#### Planilha de destino
- **URL:** `https://docs.google.com/spreadsheets/d/17uXnW7B3OoyGRgnJUtlV59S_JvaxyZe7A9R8PAJ900E`
- **Aba:** `Tributário` (gid: `1289248792`)
- **Credencial Google Sheets:** `Cockpit` (id: `jFHr4eg26MHLFHHa`) — confirmar após importar

---

#### Colunas gravadas na planilha

| Coluna | Origem | Valor padrão |
|--------|--------|-------------|
| Entrada leads | data do servidor (pt-BR, São Paulo) | — |
| Funil | **fixo** no Parse Body | `"Tributario"` |
| Nome completo | campo `nome` do form | — |
| Qual seu e-mail? | campo `email` do form | — |
| Qual o seu WhatsApp? | campo `whatsapp` do form | — |
| Confirme seu WhatsApp | campo `whatsapp_confirmacao` do form | — |
| Qual é a sua medalha no Mercado Livre? | campo `nivel_ml` do form | — |
| Qual é o seu regime tributário atual? | campo `regime_tributario` do form | — |
| channel | URL param `?channel=` | `"direto"` |
| utm_source | URL param `?utm_source=` | `"direto"` |
| utm_medium | URL param `?utm_medium=` | `"none"` |
| utm_campaign | URL param `?utm_campaign=` | `"none"` |
| utm_content | URL param `?utm_content=` | `"none"` |
| timestamp | ISO 8601 gerado no browser | — |

---

#### Captura de UTMs na LP (`index.html`)

Adicionado bloco `_utms` no JavaScript da LP:
```js
const _utms = (() => {
  const p = new URLSearchParams(window.location.search);
  return {
    channel:      p.get('channel')      || 'direto',
    utm_source:   p.get('utm_source')   || 'direto',
    utm_medium:   p.get('utm_medium')   || 'none',
    utm_campaign: p.get('utm_campaign') || 'none',
    utm_content:  p.get('utm_content')  || 'none',
    timestamp:    new Date().toISOString(),
  };
})();
```
Os UTMs são capturados no carregamento da página e incluídos no payload via spread (`..._utms`).

---

#### Como ativar

1. No n8n, clicar em **Import workflow** e colar o conteúdo de `n8n-workflow-tributario.json`
2. Ativar o workflow (toggle no topo)
3. Copiar a **Production URL** do nó Webhook (ex: `https://SEU-N8N.com/webhook/Tributario`)
4. No `index.html`, localizar `const WEBHOOK_URL = '';` e substituir pela URL copiada:
   ```js
   const WEBHOOK_URL = 'https://SEU-N8N.com/webhook/Tributario';
   ```
5. Confirmar a credencial Google Sheets `Cockpit` após importar

---

#### Exemplo de URL com rastreamento
```
https://tributario.metodop4.com.br/?utm_source=instagram&utm_medium=stories&utm_campaign=lancamento-maio&utm_content=cta-verde&channel=social
```

### v2.25 — Organização de Arquivos + Repositório GitHub (2026-05-21)

#### Solicitações atendidas
- [x] Organizar arquivos do projeto
- [x] Criar `.gitignore`
- [x] Inicializar repositório Git
- [x] Subir tudo para o GitHub

#### Repositório
**URL:** https://github.com/taysouzaa/captura-tributario  
**Branch principal:** `main`  
**Commit inicial:** `feat: landing page completa — Tributário para Marketplaces (Método P4)`

#### Estrutura final do projeto

```
captura-tributario/
│
├── index.html                        → LP principal (HTML + CSS + JS — arquivo único)
├── n8n-workflow-tributario.json      → Workflow n8n (Webhook → Parse → Google Sheets)
├── DOCUMENTACAO.md                   → Documentação técnica completa
├── .gitignore                        → Exclui Sora.zip, .DS_Store, editores
├── LICENSE                           → Licença (do repositório remoto)
├── README.md                         → README (do repositório remoto)
│
├── assets/
│   ├── 01 (1).png                    → Logo Método P4 (navbar + favicon)
│   ├── 03.png                        → Logo Método P4 (footer)
│   ├── lp-tributario.png             → Foto do hero (background)
│   ├── lojista-tributario.png        → Foto da seção "Feito de Lojista para Lojista"
│   ├── 1ec33064-...png               → Foto de fundo da seção do instrutor
│   ├── 6c9bae32-...png               → Capa do vídeo (thumbnail)
│   ├── 3.svg                         → Paleta de cores da marca
│   ├── meli.png                      → Logo Mercado Livre
│   ├── shopee.png                    → Logo Shopee
│   ├── amazon.png                    → Logo Amazon
│   ├── magalu.png                    → Logo Magalu
│   ├── shein.png                     → Logo Shein
│   └── tiktok.png                    → Logo TikTok
│
├── depoimentos/
│   ├── 1.webp … 5.webp               → Prints reais de depoimentos (carrossel)
│
└── fonts/
    ├── OFL.txt                       → Licença da fonte Sora
    ├── README.txt                    → Info da fonte
    ├── Sora-VariableFont_wght.ttf    → Fonte variável (referência)
    └── static/
        ├── Sora-Light.ttf            (weight 300)
        ├── Sora-Regular.ttf          (weight 400)
        ├── Sora-Medium.ttf           (weight 500)
        ├── Sora-SemiBold.ttf         (weight 600)
        ├── Sora-Bold.ttf             (weight 700)
        ├── Sora-ExtraBold.ttf        (weight 800)
        ├── Sora-ExtraLight.ttf       (weight 200)
        └── Sora-Thin.ttf             (weight 100)
```

#### O que foi excluído do repositório (`.gitignore`)
- `Sora.zip` — arquivo zip redundante (fontes já extraídas em `fonts/static/`)
- `.DS_Store`, `Thumbs.db`, `desktop.ini` — arquivos de sistema
- `.vscode/`, `.idea/`, `*.swp` — arquivos de editores

#### Como clonar e rodar localmente
```bash
git clone https://github.com/taysouzaa/captura-tributario.git
cd captura-tributario
# Abrir index.html no browser ou servir com live-server
```

#### Como atualizar o repositório após mudanças
```bash
git add .
git commit -m "descrição da mudança"
git push
```

### v2.26 — Correção Mobile: Selos de Confiança (2026-05-21)

#### Solicitação atendida
- [x] Corrigir visual dos selos "Compra Segura / Satisfação Garantida / Privacidade Protegida" no mobile

#### Problema
No breakpoint `≤ 480px`, o `.trust-row` estava com `grid-template-columns: 1fr`, empilhando os 3 selos verticalmente em uma única coluna — resultado feio e desequilibrado visualmente.

#### Alterações exatas realizadas (`index.html`)
```css
/* ANTES */
.trust-row { grid-template-columns: 1fr; gap: 8px; }
.trust-item { justify-content: flex-start; }
.trust-item span { font-size: .78rem; }

/* DEPOIS */
.trust-row { grid-template-columns: repeat(3, 1fr); gap: 6px; }
.trust-item { flex-direction: column; align-items: center; justify-content: center; text-align: center; gap: 4px; }
.trust-item i { font-size: 1.1rem; }
.trust-item span { font-size: .68rem; line-height: 1.2; }
```

#### Resultado
- Os 3 selos ficam lado a lado em colunas iguais
- Ícone centralizado acima do texto
- Texto menor e compacto para caber em telas pequenas

#### GitHub
- Commit: `fix(mobile): selos de confiança em 3 colunas no mobile`
- Branch: `main` — [github.com/taysouzaa/captura-tributario](https://github.com/taysouzaa/captura-tributario)

### v2.27 — Correção da Máscara e Validação de Telefone (2026-05-21)

#### Solicitação atendida
- [x] Corrigir validação que rejeitava números válidos de 11 dígitos (sem código do país)

#### Problema
A máscara e validação esperavam 13 dígitos no formato `55 11 987654321` (com prefixo do país). Usuários digitando apenas `19 987654321` (11 dígitos, sem `55`) recebiam o erro "Por favor, informe um WhatsApp válido."

#### Alterações exatas realizadas (`index.html`)

| Item | Antes | Depois |
|------|-------|--------|
| `slice` da máscara | `slice(0, 13)` | `slice(0, 11)` |
| Formato da máscara | `DD DD DDDDDDDDD` (3 grupos) | `DD DDDDDDDDD` (2 grupos) |
| Validação mínima | `wpp.length < 13` | `wpp.length < 10` |
| Placeholder | `Ex: 55 11 987654321` | `Ex: 19 987654321` |

#### Resultado
- Números no formato `DD DDDDDDDDD` (celular, 11 dígitos) aceitos normalmente
- Números fixos (`DD DDDDDDDD`, 10 dígitos) também aceitos
- Placeholder atualizado para refletir o formato esperado

---

### v2.28 — Substituição do Vídeo Vimeo por Arquivo MP4 Local (2026-05-21)

#### Solicitação atendida
- [x] Substituir embed Vimeo por vídeo local `assets/video-aula.mp4`

#### Problema
O embed Vimeo (`video/800389026`) não carregava — o vídeo estava inacessível ou com restrição de domínio no Vimeo.

#### Arquivo adicionado
- `assets/video-aula.mp4` (32,4 MB) — copiado de `aula_8_-_comparativo_lucro_real_e_presumido_v1 (1080p).mp4`

#### Evolução das tentativas

| Tentativa | Abordagem | Resultado |
|-----------|-----------|-----------|
| 1ª | Adicionado `allowfullscreen` ao iframe Vimeo | Sem efeito — problema no Vimeo |
| 2ª | `<video>` com `display:none` + JS toggling via opacity | Som funcionava, vídeo invisível |
| 3ª | `<video>` com `display:none` → `display:block` via classe | Som funcionava, vídeo invisível |
| 4ª | `<video>` sempre visível com cover overlay via z-index | Som funcionava, vídeo invisível |
| **Final** | Player HTML5 nativo simples com `poster` e `controls` | **Funcionando** |

#### Alterações finais realizadas (`index.html`)
- Removido: `<iframe>` Vimeo + `<button class="video-cover">` + JS do `VIDEO COVER`
- Adicionado: `<video src="assets/video-aula.mp4" controls playsinline preload="metadata" poster="assets/6c9bae32-...png">`
- CSS: simplificado — `<video>` ocupa `position: absolute; inset: 0; width/height: 100%` sem lógica de visibilidade
- JS: bloco `VIDEO COVER` removido por completo

#### Estrutura de arquivos atualizada
```
assets/
└── video-aula.mp4    → aula demonstrativa (Lucro Real vs Lucro Presumido, 1080p, 32,4 MB)
```

---

### v2.29 — Registro Separado da Sessão (2026-05-22)

#### Solicitações atendidas hoje
- [x] Remover da landing e da documentação menções a "grátis/gratuito/free"
- [x] Ajustar card de preço para "De: R$197,00" e "por R$97,00"
- [x] Redirecionar para checkout após envio do formulário
- [x] Orientar automação n8n para registrar o lead em duas planilhas
- [x] Gerar pacote isolado para HostGator com nomes prefixados em `tributario-tay`

#### Alterações realizadas (`index.html`)
- Copy revisada para remover referência de gratuidade em:
  - `<title>`, meta descriptions (SEO/OG/Twitter)
  - texto do hero badge
  - FAQ relacionada a gratuidade
  - mensagem pré-preenchida do botão WhatsApp
  - textos do modal de inscrição
- JSON-LD ajustado:
  - removido bloco `offers` com `price: "0"` e `category: "Free"`
  - descrição do curso ajustada para versão sem "gratuito"
- Card de preço ajustado para:
  - `De: R$197,00` (riscado)
  - `por R$97,00`
- Pós-formulário:
  - adicionado `CHECKOUT_URL` com o link de compra
  - adicionado `window.location.assign(CHECKOUT_URL)` após exibir sucesso

#### Alterações realizadas (`DOCUMENTACAO.md`)
- Conteúdo técnico alinhado com a remoção de linguagem de gratuidade
- Campos de SEO e exemplos de JSON-LD atualizados para refletir o estado atual da LP
- Registros de preço ajustados para foco em `R$97,00` quando aplicável

#### Entregáveis para deploy (HostGator)
- Pacote padrão:
  - pasta `hostgator_upload_20260522/`
  - arquivo `hostgator_upload_20260522.zip`
- Pacote isolado com prefixo anti-conflito:
  - pasta `hostgator_upload_tributario_tay/`
  - arquivo `hostgator_upload_tributario_tay.zip`
  - página principal: `tributario-tay.html`
  - assets: `tributario-tay-assets/`, `tributario-tay-depoimentos/`, `tributario-tay-fonts/`

#### Observação de automação (n8n)
- Fluxo mantido em `Webhook Tributario -> Parse Body`
- Para gravar em 2 planilhas: duplicar node Google Sheets `append` e conectar ambos na saída do `Parse Body` (execução em paralelo)

---

*Documentação atualizada em 22/05/2026 — Método P4*
