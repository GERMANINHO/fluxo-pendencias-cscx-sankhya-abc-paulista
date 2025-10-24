# Fluxo de Pendências — CS/CX Sankhya ABC Paulista

Fluxograma interativo (HTML + SVG + CSS + JS puro) que documenta e **opera** o processo de atendimento de pendências do time de CS/CX da **Sankhya ABC Paulista**.
O arquivo é **autocontido** (um único `index.html`) e pode ser publicado diretamente no **GitHub Pages**.

---

## Visão geral

* **Objetivo:** padronizar a triagem, planejamento e acompanhamento das demandas de clientes, com links diretos para as ferramentas corporativas.
* **Tecnologia:** HTML5 (SVG), CSS (estilos e animações) e JavaScript vanilla (modais e interações).
* **Design:** caixas verdes (etapas), losango roxo (decisão/estimativa), “Start/End” circulares, setas e linhas pontilhadas.
  Todas as caixas têm **efeito pulse** no hover/focus.
* **Acessibilidade:**

  * Caixas são focáveis via teclado (Tab).
  * Enter/Espaço abre o modal; **Esc** fecha.
  * Foco retorna ao elemento de origem após fechar o modal.

---

## Como usar (fluxo operacional)

O fluxograma aparece em três linhas: **Início → Linha 1 → Linha 2 → Linha 3 → Fim**.
As etapas que **abrem pop-up (modal)** explicativo são: **Triagem e Registro** e **Escopo/Plano**.

### 1) Triagem e registro das demandas recebidas

* **O que faz:** orienta a abrir a **Carteira ABC Paulista (Google Sheets)**, escolher a empresa e, na aba **“Pendencias”**, cadastrar/atualizar as demandas.
* **Como abrir:** clique na caixa “Triagem e registro…”. O modal traz instruções e um botão **“Planilha Carteira ABC Paulista”**.
* **Link padrão:** `https://docs.google.com/spreadsheets/d/1Q5yMDzWXTsXY8uEi80FEA_UssI9wyC9q/edit?gid=292230500#gid=292230500`

### 2) Consulta/atualização da carteira ABC Paulista no Google Sheets

* **O que faz:** acesso rápido à carteira para consultas/edições.
* **Ação:** caixa com link direto (abre em nova aba).

### 3) Agendamento/coordenação de agenda com o Rogério Furian

* **O que faz:** aponta para a agenda corporativa.
* **Ação:** caixa com link direto para o Google Calendar (abre em nova aba).

### 4) Registro do status atual e próximos marcos

* **O que faz:** consolidação de status/milestones; use para reportes e sincronismo com o time.

### 5) Monitoramento contínuo pelo time de Customer Success

* **O que faz:** acompanhamento sistemático dos pontos em aberto, combinando rotinas de follow-up e revisão.

### 6) Estimativa de esforço, custos e disponibilidade de recursos

* **O que faz:** etapa de decisão/planejamento (losango roxo). Use para dimensionar recursos, dependências e riscos.

### 7) Detalhamento do escopo, entregáveis e plano de execução

* **O que faz:** abre **modal** com orientações de **Escopo/Plano** (objetivo, entregáveis/aceite, 5W2H, prazos, RACI, riscos e métricas).
  O modal também oferece o botão **“Planilha Carteira ABC Paulista”** para registrar/alinhar com a empresa/aba Pendencias.
* **Link padrão do botão do modal:** mesmo da carteira (acima).

### 8) Follow-up e comunicação com cliente e gestores

* **O que faz:** lembretes, envios e registros — mantendo cliente e gestão informados.

### 9) Homologação e aceite formal da entrega

* **O que faz:** fechamento por aceite formal.
* **Ação:** link direto para SenseData (abre em nova aba).
  **Link padrão:** `https://sankhya.sensedata.io/portfolio`

### Fim

* Conclusão do processo.

---

## Estrutura do projeto

```
/ (raiz do repositório)
└── index.html   # arquivo único com: SVG do fluxo, estilos, scripts e modais
```

* **SVG:** desenha nós (retângulos, círculo de início/fim, losango) e conexões.
* **CSS:** estilos das caixas, tipografia, responsividade e **animação “pulse”**.
* **JS:** abertura/fechamento de modais, foco acessível, links dos botões.

---

## Publicação (GitHub Pages)

1. Faça commit do `index.html` na branch `main` (ou `docs`).
2. Em **Settings → Pages**:

   * **Source:** `Deploy from a branch`
   * **Branch:** `main` (folder `/root`)
3. Aguarde o deploy e acesse a URL exibida pelo GitHub Pages.

> Dica: para uma URL dedicada (ex.: `https://seu-usuario.github.io/fluxo-pendencias-cscx-sankhya-abc-paulista/`), mantenha o nome do repositório igual ao que deseja na rota.

---

## Personalização

### Editar textos das caixas

No `index.html`, procure cada `<text class="node-label ...">` e ajuste os `<tspan>`.
As classes `xs`, `xxs` e `xxxs` controlam o **tamanho da fonte** (use `xxxs` para textos longos).

### Ajustar links

* **Carteira (Triagem/Modal):**

  * IDs dos botões: `link-triagem-carteira` e `link-escopo-carteira`.
  * Altere o atributo `href` para o destino desejado.
* **Agenda Rogério:** edite o `href` do `<a>` dentro da caixa de “Agenda”.
* **Homologação:** edite o `href` do `<a>` em “Homologação e aceite…”.

### Efeito pulse (animação)

Cada caixa tem um elemento `.pulse`. A cor pode ser trocada por caixa via CSS custom property:

```html
<g class="node" style="--pulse-color:#66cc00"> … </g>
```

Ajustes de intensidade/duração:

```css
@keyframes pulseAnim {
  0%   { transform: scale(1);    opacity: .55; }
  70%  { transform: scale(1.18); opacity: 0; }
  100% { transform: scale(1.18); opacity: 0; }
}
/* mude scale/tempos para personalizar */
```

### Acessibilidade (modais)

* **Abrir:** Enter/Espaço na caixa.
* **Fechar:** clique fora do painel ou tecla **Esc**.
* **Foco:** volta para a caixa que abriu o modal.

---

## Manutenção & boas práticas

* **Versione os links corporativos** (Google Sheets, Calendar, SenseData).
  Mudanças de `gid`/URLs devem ser refletidas nos `href` do HTML.
* Evite blocos de texto muito longos nas caixas; quando necessário, **divida com `<tspan>`** ou reduza para `xxs/xxxs`.
* Sempre teste:

  * Hover/focus e pulse;
  * Abertura/fechamento dos modais com teclado e mouse;
  * Abertura dos links (nova aba) e permissões de acesso.

---

## FAQ

**1) O texto estourou a caixa. O que fazer?**
Use a classe menor (`xxs` → `xxxs`) ou reduza as linhas dos `<tspan>`.

**2) O modal não abre pelo teclado.**
Confirme se o grupo tem `tabindex="0"` e se o listener está ativo no script (Enter/Espaço).

**3) O link do Google Sheets não carrega.**
Verifique se está logado na conta com permissão e se o `gid` é o correto.

---

## Licença

Este repositório pode ser utilizado internamente pela equipe.
Caso queira abrir, recomendo **MIT License** (simples e permissiva). Adicione um arquivo `LICENSE` se necessário.

---

## Créditos

Projeto concebido para a operação de **CS/CX Sankhya ABC Paulista**.
Implementação em HTML/SVG/CSS/JS com foco em simplicidade, acessibilidade e facilidade de manutenção.

---

### Resumo rápido (TL;DR)

* Abra o site (GitHub Pages).
* Clique nas caixas para seguir o fluxo; **Triagem** e **Escopo/Plano** mostram **modais** com instruções + botão para a **Carteira ABC Paulista**.
* **Agenda** e **Homologação** abrem links diretos.
* Todo o comportamento está em **um único `index.html`** — fácil de editar e publicar.
