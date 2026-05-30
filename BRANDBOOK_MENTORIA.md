# BRANDBOOK_MENTORIA.md
### Sistema visual — Mentoria Marca com Essência© · KA | Inteligência para Marcas

> Documento de continuidade para retomar este projeto no **Claude Code**.
> Leia este arquivo inteiro antes de mexer em qualquer peça. As regras da Seção 2 são **inegociáveis**.

---

## 0. Contexto rápido

A **Mentoria Marca com Essência©** é um produto dentro do ecossistema KA (Livro → Mentoria → Direção → Projeto). A identidade da Mentoria é um **tier** dentro do sistema KA — não é uma marca nova. As cores e a estrutura foram extraídas e validadas a partir da página oficial: `https://kellyalbert.com.br/mentoria/`.

Toda a entrega vive num único arquivo: **`brandbook_mentoria.html`** (fontes embutidas em base64, abre offline). Este `.md` é o manual de continuidade.

---

## 1. Arquivos do handoff

```
/handoff
├── BRANDBOOK_MENTORIA.md        ← este documento
├── brandbook_mentoria.html      ← o sistema visual completo (fonte da verdade)
└── /fontes
    ├── PlayfairDisplay.ttf        (nome e títulos)
    ├── PlayfairDisplay-Italic.ttf (itálico de "Essência")
    ├── Montserrat.ttf             (kicker "MENTORIA" e rótulos)
    └── Manrope.ttf                (texto corrido)
```

No HTML as fontes estão embutidas via `@font-face` em base64. Para um site de produção (Netlify), o recomendado é servir os `.ttf`/`.woff2` da pasta `/fontes` e remover o base64, para deixar o arquivo leve.

---

## 2. As 3 leis do wordmark — INEGOCIÁVEIS

Estas três regras já causaram retrabalho. Não quebrar em hipótese nenhuma.

1. **Estrutura fixa em 3 linhas, sempre:**
   ```
   MENTORIA          ← kicker
   Marca com         ← linha 1 do nome (SEMPRE juntas)
   Essência©         ← linha 2 do nome
   ```
2. **"Marca com" nunca quebra.** "com" fica SEMPRE colado em "Marca", na mesma linha. Garantido por `white-space:nowrap` em `.wm__l1`. Nunca remover essa propriedade.
3. **"MENTORIA" é SEMPRE menor que o nome** — proporção fixa de **0.26 da altura do nome** (`font-size:.26em` em `.wm__kicker`). Nunca igual nem maior.

Variações permitidas: **apenas o alinhamento** — `wm--center`, `wm--left`, `wm--right`. Nada mais muda.

### Proibido
- Nome em uma linha só, ou em mais de duas linhas.
- Separar "Marca" de "com", ou subir "Essência" para cima.
- "MENTORIA" em Playfair, em caixa baixa, ou do mesmo tamanho do nome.
- "KA" junto ao nome (o nome é só *Mentoria Marca com Essência©*).
- Inverter cores entre "Marca com" e "Essência".
- Qualquer cor fora de marinho / laranja / creme.

---

## 3. Tokens de cor

Extraídos pixel a pixel da página oficial. Use sempre via variável CSS.

| Token | Hex | Uso |
|---|---|---|
| `--navy` | `#1A2737` | Texto principal e fundos escuros |
| `--orange` | `#B97B40` | Acento: "Essência", "MENTORIA", botões, detalhes |
| `--cream` | `#F7F6F2` | Fundo claro e texto sobre o marinho |
| `--line` | `#D9D2C5` | Linhas, bordas, divisores |
| `--mut` | `#6F7682` | Texto secundário / legendas |

```css
:root{
  --navy:#1A2737; --orange:#B97B40; --cream:#F7F6F2;
  --line:#D9D2C5; --mut:#6F7682;
}
```

Sobre fundo marinho, "Marca com" usa `--cream` (regra `.on-navy .wm__l1`).

---

## 4. Tipografia

| Papel | Fonte | Peso / estilo | Onde |
|---|---|---|---|
| Nome e títulos | **Playfair Display** | 600 (e itálico 600 p/ "Essência") | wordmark, h2, títulos das peças |
| Kicker e rótulos | **Montserrat** | 700, CAIXA ALTA, tracking `.40em` | "MENTORIA", eyebrows, labels |
| Texto corrido | **Manrope** | 400 / 700 | parágrafos, descrições |

Regra-mãe: **"MENTORIA" usa SEMPRE Montserrat.** É o que padroniza todas as aplicações.

---

## 5. O componente wordmark (copiar e reusar)

```html
<!-- Trocar só o alinhamento: wm--center | wm--left | wm--right -->
<span class="wm wm--center" style="font-size:64px">
  <span class="wm__kicker">Mentoria</span>
  <span class="wm__l1">Marca com</span>
  <span class="wm__l2">Essência<span class="wm__c">©</span></span>
</span>
```

```css
.wm{display:inline-flex;flex-direction:column;line-height:1.04;}
.wm--center{align-items:center;text-align:center;}
.wm--left{align-items:flex-start;text-align:left;}
.wm--right{align-items:flex-end;text-align:right;}

/* MENTORIA — proporção fixa, sempre menor */
.wm__kicker{
  font-family:'Montserrat',sans-serif; font-weight:700;
  text-transform:uppercase; letter-spacing:.40em;
  color:var(--orange); font-size:.26em;   /* 26% da altura do nome — FIXO */
  margin-bottom:.45em;
}
.wm--center .wm__kicker{margin-right:-.40em;} /* compensa tracking p/ centralizar */

/* Nome — Playfair, sempre 2 linhas, "Marca com" nunca quebra */
.wm__l1,.wm__l2{font-family:'Playfair Display',serif;font-weight:600;letter-spacing:.01em;display:block;white-space:nowrap;}
.wm__l1{color:var(--navy);}                       /* Marca com */
.wm__l2{color:var(--orange);font-style:italic;}   /* Essência© */
.wm__c{font-size:.34em;vertical-align:super;}
.on-navy .wm__l1{color:var(--cream);}
```

O tamanho do wordmark é controlado **só** pelo `font-size` do `.wm` pai. Tudo dentro é proporcional (`em`), então kicker e © escalam juntos automaticamente.

---

## 6. Peças já no sistema (no HTML)

Todas renderizam ao vivo, em proporção real, e herdam o wordmark:

- **Capa de slide dos encontros** — 16:9
- **Post de feed** — 1:1
- **Story** — 9:16
- **Selo da turma** — 1:1
- **Capa do documento "Base Estratégica da Sua Marca"** — A4 (1:1.414)
- **Assinatura de e-mail**

Dados fixos da turma atual (atualizar a cada edição): **Início 16 jun 2026 · 19h30 · terças e quintas · 8 encontros · Google Meet.**

> **Texto padrão da turma (escrever SEMPRE assim):** `2º Turma | 16 de junho/2026` (com "junho" em minúscula).
> Esse é o formato canônico para qualquer peça que mostre turma + data (feed, story, selo, assinatura). No selo ele aparece quebrado em duas linhas ("2º Turma" + "16 de junho/2026"), e logo abaixo vem **"Terça-Feira às 19h30"**.

### Ajustes já aplicados nesta rodada (peças)
- **Capa de slide (16:9):** logo *Marca com Essência* ampliado e centralizado no painel marinho; removida a frase "Mentoria · Encontro".
- **Post de feed (1:1):** logo ampliado; etiqueta da turma no padrão `2º Turma | 16 de junho/2026`.
- **Story (9:16):** removido o degradê (`.glow`) do fundo; título reduzido para ficar proporcional; turma adicionada no rodapé.
- **Selo (1:1):** layout refeito — textos menores, todos dentro do círculo (anel ampliado p/ 88%).
- **Unidades:** as peças passaram a usar `cqw` (container query) em vez de `vw`. Cada peça escala pelo próprio tamanho, então dá pra exibi-las reduzidas (classes `.fr-slide`, `.fr-feed`, etc. controlam só a largura de visualização) sem distorcer proporção.

---

## 7. Roadmap — próximos passos (escolher no Claude Code)

Em aberto, em ordem sugerida. Pegar uma frente por vez.

- [ ] **A. Integrar ao site (Netlify).** Transformar o wordmark e os tokens em componentes da página `/mentoria`. Servir as fontes da pasta `/fontes` (não base64). Site canônico já existe em `kellyalbert.com.br`.
- [ ] **B. Export programático das peças.** Gerar PNG/PDF de cada peça (Playwright → screenshot por seletor), em alta resolução, para uso imediato no Instagram/impressão.
- [ ] **C. Expandir o sistema.** Novas peças: capa de YouTube, cartão de boas-vindas da turma, os 8 slides internos numerados (títulos já existem: Essência, Posicionamento, Diferenciais, Proposta de Valor, Personalidade/Tom de Voz, Narrativa, Arquitetura, Presença Digital).
- [ ] **D. Versão transparente do wordmark.** Hoje as peças saem com fundo. Gerar PNG com fundo transparente para aplicar sobre foto.
- [ ] **E. Master vetorial (com a Gabi).** `.ai`/`.svg` em CMYK para impressão (certificado). Ajustar o kerning do © (na web ele fica colado no "a").

---

## 8. Como rodar / editar no Claude Code

```bash
# Ver no navegador
open brandbook_mentoria.html        # (ou abrir o arquivo direto)

# Se for gerar PNGs das peças (frente B do roadmap):
pip install playwright --break-system-packages
playwright install chromium
# depois: page.locator(".r-slide").screenshot(...) por seletor
```

Para editar: o HTML é autocontido. Procure os comentários `/* ===== WORDMARK ===== */`, `:root{...}` (tokens) e as classes `.r-slide`, `.r-feed`, `.r-story`, `.r-selo`, `.r-base`, `.r-mail` (peças).

---

## 9. Checklist antes de publicar qualquer peça

- [ ] MENTORIA em cima, menor (Montserrat, caixa alta, laranja)?
- [ ] "Marca com" numa linha só, "Essência©" embaixo?
- [ ] Cores só marinho / laranja / creme?
- [ ] Sem "KA" junto ao nome?
- [ ] "Essência" em Playfair itálico laranja?
- [ ] Dados da turma corretos (data, horário)?
- [ ] É identidade do **produto** — não identidade visual do cliente (a Mentoria não inclui isso)?

---

*Porque essência conecta e posicionamento faz vender.*
KA | Inteligência para Marcas · 2026
