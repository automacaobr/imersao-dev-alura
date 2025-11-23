# Base de Conhecimento

## Visão Geral

Este projeto é uma "Base de Conhecimento" — uma ferramenta web simples desenvolvida para fins educacionais. O objetivo é fornecer uma interface para pesquisar e exibir informações sobre diversas linguagens de programação e tecnologias de desenvolvimento de software. A aplicação é totalmente frontend, utilizando HTML, CSS e JavaScript para criar uma experiência de usuário interativa e responsiva, com os dados sendo carregados a partir de um arquivo JSON local.

## Como Funciona

A aplicação consiste em uma única página web com os seguintes elementos:

1.  **Cabeçalho:** Contém o título da aplicação e um campo de busca.
2.  **Campo de Busca:** Permite que o usuário digite um termo (como o nome de uma linguagem ou uma palavra-chave) para filtrar as tecnologias.
3.  **Botão "Buscar":** Ao ser clicado, aciona a lógica de pesquisa.
4.  **Container de Cards:** Uma seção principal onde os resultados da busca são exibidos em formato de "cards".

O fluxo de interação do usuário é o seguinte:

- O usuário digita um termo no campo de busca (por exemplo, "JavaScript", "banco de dados", "2014").
- Ao clicar no botão "Buscar", o código JavaScript é executado.
- O JavaScript lê os dados do arquivo `data.json`.
- Ele filtra a lista de tecnologias, mantendo apenas aquelas cujo nome ou descrição contenham o termo pesquisado (a busca não diferencia maiúsculas de minúsculas).
- Para cada resultado correspondente, um card é gerado dinamicamente e inserido na página.
- Cada card exibe o nome da tecnologia, o ano de sua criação, uma breve descrição e um link "Saiba mais" que redireciona para a documentação oficial ou página principal.
- Se uma nova busca for realizada, os cards antigos são removidos e a nova lista de resultados é exibida.

---

## Detalhamento Técnico e Comandos

O projeto não requer um servidor backend para funcionar e pode ser executado diretamente em um navegador. A seguir, um detalhamento de cada arquivo.

### `index.html`

Este é o arquivo principal que estrutura a página web.

- **`<header>`:** Contém o título e a barra de busca, e é definido como `position: sticky` para permanecer no topo da tela durante a rolagem.
- **`<input type="text" id="campo-busca">`:** O campo onde o usuário insere o texto para a pesquisa.
- **`<button onclick="iniciarBusca()">`:** O botão que chama a função `iniciarBusca()` do `script.js` quando clicado.
- **`<main>` e `<section class="card-container">`:** A área onde os cards com os resultados da busca serão inseridos dinamicamente pelo JavaScript.
- **`<script src="script.js"></script>`:** Inclui o arquivo JavaScript no final do `<body>` para garantir que o DOM (a estrutura HTML) seja totalmente carregado antes que o script tente manipulá-lo.

### `script.js`

Este arquivo contém toda a lógica da aplicação.

- **`async function iniciarBusca()`:**
    - É uma função assíncrona, pois utiliza o `await` para esperar a resposta da leitura do arquivo.
    - **`fetch("data.json")`:** Faz uma requisição para carregar o conteúdo do arquivo `data.json`.
    - **`await resposta.json()`:** Converte a resposta da requisição (que é texto) para um objeto JavaScript.
    - **`const termoBusca = campoBusca.value.toLowerCase()`:** Pega o valor digitado pelo usuário, o converte para letras minúsculas para uma busca "case-insensitive".
    - **`dados.filter(...)`:** Filtra o array de dados. Para cada objeto, verifica se o `nome` ou a `descricao` (convertidos para minúsculas) incluem o `termoBusca`.
    - **`rederizarCards(resultados)`:** Chama a função que exibirá os resultados filtrados na tela.

- **`function rederizarCards(dados)`:**
    - **`cardContainer.innerHTML = ""`:** Limpa a área de resultados antes de exibir os novos, evitando que buscas antigas se acumulem.
    - **`for (let dado of dados)`:** Itera sobre cada um dos resultados filtrados.
    - **`document.createElement("article")`:** Cria um novo elemento `<article>` para cada card.
    - **`article.innerHTML`:** Preenche o card com as informações do objeto (`nome`, `ano`, `descricao`, `link`).
    - **`cardContainer.appendChild(article)`:** Adiciona o novo card à página.

### `style.css`

Este arquivo é responsável pela aparência e responsividade da aplicação.

- **`@import url(...)`:** Importa a fonte "Quicksand" do Google Fonts.
- **`:root` e Variáveis CSS (`--primary-color`, etc.):** Define um esquema de cores centralizado. Se for necessário alterar uma cor em todo o site, basta mudar o valor da variável.
- **`* { box-sizing: border-box; }`:** Uma prática recomendada que facilita o cálculo de larguras e alturas dos elementos.
- **Layout com Flexbox:** O `header` utiliza `display: flex` para alinhar seus itens de forma organizada.
- **Estilo dos Cards e Efeitos `hover`:** Os cards possuem um efeito sutil de zoom (`transform: scale(1.01)`) quando o mouse passa sobre eles, melhorando a interatividade.
- **Media Queries (`@media (max-width: ...)`):** Contêm regras de CSS que são aplicadas apenas em telas menores (como tablets e celulares), ajustando o layout para garantir que a aplicação seja agradável de usar em qualquer dispositivo. Por exemplo, em telas pequenas, o cabeçalho e o campo de busca se reorganizam para um layout vertical.

### `data.json`

É um arquivo no formato JSON (JavaScript Object Notation) que atua como um banco de dados simples. Ele contém um array de objetos, onde cada objeto representa uma tecnologia e possui as seguintes propriedades: `nome`, `descricao`, `ano`, `link` e `tags`.

---

## Boas Práticas Implementadas

1.  **Separação de Responsabilidades:**
    - **HTML (Estrutura):** `index.html` cuida apenas da marcação e do conteúdo estático.
    - **CSS (Apresentação):** `style.css` lida com toda a parte visual, sem misturar estilos no HTML.
    - **JavaScript (Comportamento):** `script.js` gerencia a interatividade e a manipulação de dados.

2.  **JavaScript Moderno:**
    - Uso de `let` e `const` em vez de `var`, o que melhora o escopo de variáveis.
    - Uso de `async/await` com `fetch` para lidar com código assíncrono de forma mais limpa e legível.

3.  **CSS Organizado e Reutilizável:**
    - Utilização de **variáveis CSS (`:root`)** para o tema de cores, facilitando a manutenção.
    - Design **responsivo** com media queries para adaptação a diferentes tamanhos de tela.
    - Comentários explicativos para regras CSS mais complexas.

4.  **Acessibilidade e UX (Experiência do Usuário):**
    - O campo de busca possui um `placeholder` para instruir o usuário.
    - O cursor muda para "ponteiro" em elementos clicáveis (`cursor: pointer`).
    - O feedback visual nos botões e cards (`:hover`, `:active`) informa ao usuário que a interface está respondendo.

5.  **Performance:**
    - O script `script.js` é carregado no final do `<body>`, permitindo que a página seja renderizada visualmente antes da execução do JavaScript, melhorando a percepção de velocidade de carregamento.

---

## Sugestões de Melhorias

Embora o código atual seja funcional e bem-estruturado para fins educacionais, ele poderia ser aprimorado com as seguintes funcionalidades:

1.  **Carregamento Inicial dos Dados:** Atualmente, os dados só são carregados após a primeira busca. Seria interessante exibir todos os cards assim que a página carrega, chamando a função `iniciarBusca()` (ou uma similar) sem um termo de busca logo no início.

2.  **Busca em Tempo Real:** Em vez de esperar o clique no botão "Buscar", a busca poderia ser acionada automaticamente enquanto o usuário digita, utilizando o evento `onkeyup` ou `oninput` no campo de busca.

3.  **Filtragem por Tags:** O `data.json` contém um campo `"tags"`. Uma funcionalidade avançada seria permitir que o usuário clicasse em tags para filtrar as tecnologias (por exemplo, clicar em "frontend" para ver apenas ferramentas dessa categoria).

4.  **Feedback de "Nenhum Resultado":** Se a busca do usuário não encontrar nenhum resultado, a tela simplesmente fica em branco. Seria mais amigável exibir uma mensagem como "Nenhum resultado encontrado para a sua busca."

5.  **Tratamento de Erros:** O código não possui tratamento para o caso de o arquivo `data.json` não ser encontrado ou estiver mal formatado. O uso de um bloco `try...catch` em volta do `fetch` poderia capturar esses erros e exibir uma mensagem amigável.

6.  **Modularização do JavaScript:** Para projetos maiores, o `script.js` poderia ser dividido em módulos (ESM - ECMAScript Modules), separando a lógica de busca da lógica de renderização, por exemplo.

7.  **Melhorias de Acessibilidade (a11y):**
    - Adicionar atributos ARIA para indicar o status da busca (por exemplo, `aria-live` na região dos resultados).
    - Garantir que a navegação via teclado seja fluida, especialmente o foco nos resultados da busca.
