# README — Fluxograma de Processos CS/CX (SVG em arquivo único)

Este documento descreve **tudo** o que foi feito até agora no nosso fluxograma: decisões de design, estrutura do código, ajustes solicitados, animações e links. O objetivo é deixar claro **como o arquivo funciona**, **o que já foi alterado** e **como manter/editar** no futuro.

---

## 1) Visão geral

* Construímos um **fluxograma responsivo em SVG** dentro de um **único arquivo HTML** (HTML5 + CSS3 + JS embutidos).
* Todo o desenho é vetorial (SVG) para manter nitidez em qualquer zoom/tela.
* Mantemos **Início → 9 etapas nomeadas → Fim** em **3 linhas**, com conectores pontilhados entre as caixas.
* Implementamos um **efeito “pulse”** (inspirado no *button hover pulse* via `box-shadow`) **adaptado para SVG**.
* Adicionamos **dois links clicáveis** diretamente em nós do diagrama:

  * **Planilha Google (Sheets) Carteira ABC Paulista** → abre a planilha no Google Sheets.
  * **Agenda Rogerio Furian** → abre o Google Calendar (agenda do Rogério).

---

## 2) Estrutura e responsividade

* O SVG usa `viewBox="0 0 500 500"` e `preserveAspectRatio="xMinYMin meet"` para escalar proporcionalmente.
* A responsividade da área visível é garantida por um *wrapper* com o truque do **`padding-bottom: 100%`**:

  ```css
  .svg-container { position: relative; width: 100%; padding-bottom: 100%; }
  .svg-content   { position: absolute; inset: 0; }
  ```
* Cores base do layout:

  * Fundo da página: `#f5f3e7`.
  * Caixas verdes: `#66cc00`.
  * Losango (decoração no nó “Custo/Recurso”): `#72508d`.
  * Conectores: linhas pontilhadas `#443c3d`.

---

## 3) Evolução (passo a passo do que foi feito)

1. **Unificação**: juntamos **HTML, CSS e JS** em um **único arquivo** (sem dependências locais).
2. **Limpeza de rótulos**: removemos primeiro só “Step #10” e depois **todos os “Step #X”**, deixando apenas “WORKFLOW”.
3. **Tradução das extremidades**: trocamos “Start” → **Inicio** e “End” → **Fim**.
4. **Nomeação das etapas** (na ordem do fluxo), substituindo “WORKFLOW” por:

   1. **Pendencia**
   2. **Planilha Google (Sheets) Carteira ABC Paulista**
   3. **Agenda Rogerio Furian**
   4. **Status**
   5. **CS Acompanhamento**
   6. **Custo/Recurso**
   7. **Escopo/Plano de Ação**
   8. **Spams Cliente / Rogerio Furian**
   9. **Validar Entrega**
5. **Ajuste de layout final**: mantivemos **nove** etapas antes do **Fim**. Na última linha, **removemos o terceiro bloco** e ligamos o segundo bloco diretamente ao **Fim** para respeitar a contagem de 9 etapas.
6. **Tamanhos de fonte e quebras**:

   * Reduzimos os tamanhos para caber dentro das formas e criamos classes utilitárias:

     * `.node-label` (base, 10px), `.node-label.sm` (9px) e `.node-label.xs` (8px).
   * Inserimos quebras com `<tspan>` para rótulos longos (ex.: Planilha, Agenda, Spams, Validar Entrega).
   * Centralizamos textos com `text-anchor="middle"` e `dominant-baseline="middle"` quando adequado.
7. **Correção de ordem** (2ª linha): **Status** ficou à esquerda e **CS Acompanhamento** ficou no segundo bloco (como solicitado).
8. **Efeito “Pulse” aplicado a todas as caixas**:

   * Como `box-shadow` **não** funciona em formas SVG, **adaptamos** o efeito usando um **“anel de pulso”** (`.pulse-ring`) que é um **stroke animado** (cresce e desaparece).
   * O efeito é disparado no `:hover` e `:focus` do grupo `.node` (acessível via teclado).
9. **Correção do “Pulse” no “Custo/Recurso”**:

   * O anel estava desalinhado por usar inicialmente um `polygon` maior/offset.
   * **Fix**: passamos a usar um **retângulo de anel** alinhado **exatamente** ao retângulo principal do nó (x=2, y=25, w=90, h=50).
10. **Links clicáveis**:

    * **Planilha Google (Sheets) Carteira ABC Paulista**: adicionamos `<a>` dentro do grupo do nó com `target="_blank"` e `rel="noopener noreferrer"`.
    * **Agenda Rogerio Furian**: mesmo padrão, abrindo o Google Calendar.

---

## 4) Como o código está organizado no SVG

Cada etapa (nó) é um `<g class="node" transform="translate(...)" tabindex="0">` contendo:

* **Anel de pulso**: um `<rect class="pulse-ring">` (ou poderia ser `polygon` se quisermos um anel no formato do losango).
* **Forma principal**:

  * Retângulo verde (`<rect class="main" fill="#66cc00">`) para a maioria dos nós.
  * Em **Custo/Recurso**, mantivemos um **losango decorativo** (`<polygon class="main" ...>`) atrás do retângulo principal roxo.
* **Texto**: `<text class="node-label ...">` com eventuais `<tspan>` para quebrar linhas.
* **(Quando há link)**: todo o bloco de desenho/texte é envolvido por um `<a ...>`.

Exemplo de nó com link (Planilha):

```html
<g class="node" tabindex="0" transform="translate(268)">
  <a href="https://docs.google.com/..." target="_blank" rel="noopener noreferrer">
    <title>Abrir Planilha Google (Sheets) — Carteira ABC Paulista</title>
    <rect class="pulse-ring" x="2" y="25" rx="6" ry="6" width="90" height="50" />
    <rect class="main" fill="#66cc00" x="2" y="25" rx="3" ry="3" width="90" height="50" />
    <text class="node-label xs" x="47" y="40" text-anchor="middle">
      <tspan x="47" dy="0">Planilha Google</tspan>
      <tspan x="47" dy="11">(Sheets)</tspan>
      <tspan x="47" dy="11">Carteira ABC Paulista</tspan>
    </text>
  </a>
</g>
```

---

## 5) O efeito “Pulse” (como funciona e como ajustar)

* Em CSS criamos `.pulse-ring` e uma animação `@keyframes pulseRing`:

  ```css
  .pulse-ring {
    fill: none;
    stroke: #f699d1;     /* cor do pulso */
    stroke-width: 0;
    opacity: 0;
    transform-box: fill-box;
    transform-origin: center;
    pointer-events: none;
  }
  .node:hover .pulse-ring,
  .node:focus .pulse-ring { animation: pulseRing 1s ease-out; }

  @keyframes pulseRing {
    0%   { opacity:.75; stroke-width:6;  transform: scale(1);    }
    70%  { opacity:.35; stroke-width:14; transform: scale(1.25); }
    100% { opacity:0;   stroke-width:18; transform: scale(1.35); }
  }
  ```
* O brilho leve na caixa em hover/focus:

  ```css
  .node rect.main, .node polygon.main { transition: filter .25s ease; }
  .node:hover rect.main, .node:focus rect.main,
  .node:hover polygon.main, .node:focus polygon.main {
    filter: brightness(1.06) saturate(1.02);
  }
  ```
* **Dica**: se quiser que cada nó pulse na **mesma cor da sua caixa**, basta:

  * Definir `stroke: currentColor` em `.pulse-ring`.
  * Atribuir `color: #66cc00` (ou outra) no `<g class="node">` correspondente.

---

## 6) Acessibilidade e usabilidade

* O SVG tem `<title>` e `<desc>` para leitores de tela.
* Cada nó interativo possui `tabindex="0"` para receber foco via teclado (e disparar o efeito `:focus`).
* O texto dentro das caixas não intercepta o mouse (`pointer-events: none;`) para o clique acontecer no grupo inteiro.

---

## 7) Como editar textos e disposições

* **Textos curtos**: edite diretamente o conteúdo do `<text class="node-label">...</text>`.
* **Textos em 2 ou 3 linhas**: use `<tspan>` com `x` fixo e `dy="11"` (ou ajuste) para o espaçamento entre linhas.
* **Fontes**:

  * Use `.node-label` (base), `.node-label.sm` (médio) ou `.node-label.xs` (pequeno) para se ajustar ao espaço.
* **Posição dos nós**: mude o `transform="translate(x,y)"` no `<g class="node">`.
* **Adicionar um novo nó**:

  * Copie um `<g class="node">...</g>`, cole, mude o `translate(...)`, edite texto e conectores (`<line>`).
* **Links**:

  * Envolva o conteúdo do nó com `<a href="..." target="_blank" rel="noopener noreferrer"> ... </a>`.
  * Mantivemos `xmlns:xlink` e também `xlink:href` para compatibilidade ampla (SVG 1.1); `href` puro já funciona em navegadores modernos.

---

## 8) Conectores, seta e ordem do fluxo

* Conectores são `<line>` com `stroke-dasharray="2,1"`.
* A seta é um `<marker id="arrow">` acoplado via `marker-start="url(#arrow)"` (usado no conector que aponta para “Escopo/Plano de Ação”).
* Ordem final das etapas (esquerda → direita; de cima para baixo):

  1. **Pendencia**
  2. **Planilha Google (Sheets) Carteira ABC Paulista**
  3. **Agenda Rogerio Furian**
  4. **Status**
  5. **CS Acompanhamento**
  6. **Custo/Recurso**
  7. **Escopo/Plano de Ação**
  8. **Spams Cliente / Rogerio Furian**
  9. **Validar Entrega**

  * **Fim**

---

## 9) Decisões importantes e correções

* **Remoção dos “Step #X”** para manter o diagrama limpo e 100% em PT-BR.
* **Troca de Start/End** para **Inicio/Fim** (grafia sem acento conforme pedido).
* **Ajuste tipográfico**: redução de fontes + uso de `<tspan>` para caber nos retângulos.
* **Inversão de Status/CS Acompanhamento** (2ª linha) conforme fluxo desejado.
* **Pulse no “Custo/Recurso”**: bug corrigido (anel reposicionado para o retângulo principal).
* **Links adicionados**:

  * Planilha (Google Sheets).
  * Agenda (Google Calendar).
* **Acessibilidade**: foco por teclado, `<title>` e `<desc>`, e `pointer-events: none` no texto.

---

## 10) Como executar

* Basta **abrir o arquivo HTML** em qualquer navegador moderno.
* Não há dependências de build; a única chamada externa é a fonte do Google Fonts.

---

## 11) Personalizações futuras (opcional)

* **Pulso por cor de nó**: fazer o pulso herdar a cor da caixa (`stroke: currentColor` + `color` no grupo).
* **Pulso no formato do losango**: trocar o `<rect class="pulse-ring">` por um `<polygon class="pulse-ring">` com os mesmos pontos do losango e sem `transform`.
* **Tooltip por nó**: adicionar `<title>` específico dentro de cada grupo para mostrar dica ao passar o mouse.
* **Estados (feito/em progresso/bloqueado)**: variar `fill` e/ou borda do retângulo/losango por classe (ex.: `.done`, `.wip`, `.blocked`).
* **Exportar imagem**: criar um botão que usa `SVG` → `PNG` via canvas para baixar prints do fluxograma.

---

## 12) Referência rápida de classes

* **.node**: grupo interativo do nó (recebe `hover`/`focus`).
* **.main**: forma visual do nó (retângulo ou losango).
* **.pulse-ring**: anel animado do pulso (stroke animado).
* **.node-label / .sm / .xs**: tamanhos de texto (10px, 9px, 8px).

---

Se quiser, eu posso gerar uma versão **README.md** pronta em Markdown (com trechos de código já formatados) para colocar direto no repositório.
