# Módulo 21 - Projeto 4

## Menu 
[Aula 01 - Inicie o Projeto ](#aula-1--inicie-o-projeto)  
[Aula 02 - Crie o Hero ](#aula-2--crie-o-hero)  
[Aula 03 - Importe uma fonte externa  ](#aula-3--importe-uma-fonte-externa)  
[Aula 04 - Crie a sessão de atrações ](#aula-4--crie-a-sessão-de-atrações)  
[Aula 05 - Criar listagem de planos](#aula-5--criar-listagem-de-planos)  
[Aula 06 - Crie a seção "Assista do seu jeito" ](#aula-6--crie-a-seção-assista-do-seu-jeito)  
[Aula 07 - Crie a sessão dispositivos disponíveis ](#aula-7--crie-a-sessão-dispositivos-disponíveis)  
[Aula 08 - Crie o FAQ ](#aula-8--crie-o-faq)  
[Aula 09 - Crie o Rodapé ](#aula-9--crie-o-rodapé)  
[Aula 10 - Crie o Cabeçalho ](#aula-10--crie-o-cabeçalho)  
[Aula 11 - ]()  

Claro! Com base nas suas anotações e no código final do `gulpfile.js`, aqui está o **texto unificado da Aula 1** no formato estruturado que combinamos:

---

## **Aula 1 – Inicie o projeto**

### **Objetivos da aula**

* Compreender a estrutura do projeto;
* Configurar o ambiente de desenvolvimento;
* Iniciar o controle de versão e colaboração.

---

### **Introdução ao projeto**

O projeto da aula consiste na construção de um **clone da página inicial da Disney+**. A proposta é analisar o site real da plataforma, identificar características técnicas e, com base nisso, desenvolver nossa própria versão com práticas modernas.

Durante a análise, o professor observou que o site da Disney+ (em 2022) apresentava algumas decisões incomuns, como o uso de `<br>` para espaçamento ao invés de `margin`, além de `<div>`s vazias com `margin: 40px` aplicadas. Por conta dessas inconsistências, levantou-se a hipótese de que o site pode ter sido criado com alguma **ferramenta low-code**, como Wix ou algum construtor com templates.

Outro ponto mencionado foi o uso de **duas navegações (nav)** no site da Disney+: uma visível inicialmente com o botão “Entrar”, e outra que aparece ao rolar a página, contendo o logo da marca, um fundo preto e o botão “Assine agora”. No nosso projeto, utilizaremos **apenas um único nav fixo**.

---

### **Configuração inicial do projeto**

A primeira etapa prática foi a criação de um repositório no GitHub. O professor nomeou o dele como `clone-disney+`, e no meu caso, preferi usar "Módulo 20 - EBAC" para facilitar a organização.

Em seguida, iniciamos o projeto com os comandos:

```bash
npm init
npm i --save-dev gulp gulp-sass sass
```

Esses pacotes permitem configurar um fluxo de automação com **Gulp** e compilar arquivos **SCSS com o Sass**.

---

### **Estrutura do Gulp**

O arquivo `gulpfile.js` foi criado com o seguinte conteúdo:

```js
const gulp = require('gulp');
const sass = require('gulp-sass')(require('sass'));

function styles() {
    return gulp.src('./src/styles/*.scss')
        .pipe(sass({ outputStyle: 'compressed'}))
        .pipe(gulp.dest('./dist/css'));
}

exports.default = styles;
exports.watch = function () {
    gulp.watch('./src/styles/*.scss', gulp.parallel(styles));
}
```

**Explicação:**

* `styles()` compila os arquivos `.scss` da pasta `src/styles`, comprime o CSS e envia para `dist/css`.
* `gulp.watch` observa alterações nos arquivos `.scss` e executa automaticamente a função `styles` quando necessário.
* `gulp.parallel(styles)` garante que a função `styles` seja executada corretamente no modo de observação.

---

### **Organização do projeto**

Foi criada a seguinte estrutura de pastas:

```
/src
  /styles
    main.scss
/dist
  /css
    main.css (gerado automaticamente)
```

No arquivo `main.scss`, criamos uma variável SCSS para testar a compilação:

```scss
$bgColor: red;

body {
  background-color: $bgColor;
}
```

Ao rodar `npm run build`, o CSS foi gerado corretamente.

---

### **Automatização e observação com Watch**

Para evitar a necessidade de rodar o comando manualmente a cada alteração, implementamos a função `watch`, que observa a pasta `src/styles` e recompila automaticamente o CSS ao detectar mudanças.

Durante os testes, usei o comando:

```bash
npm run build watch
```

Na primeira execução, não funcionou como esperado. Revisei o código, não alterei nada, e na segunda tentativa o comando funcionou corretamente.

---

### **Versionamento com Git**

O professor demonstrou como usar os comandos do Git no terminal:

1. `git status` – ver quais arquivos estão fora do controle de versão;
2. `git add .` – adicionar todos os arquivos;
3. `git commit -m "setup do projeto"` – criar um commit com mensagem;
4. `git push` – enviar para o GitHub.

No meu caso, utilizei a **interface gráfica do VS Code**, adicionando os arquivos com o botão "+", e usando o **Copilot** para sugerir a mensagem do commit.

Também adicionei a pasta `dist` ao `.gitignore` para evitar que os arquivos gerados sejam enviados ao repositório.

---

### **HTML inicial**

Criamos o arquivo `index.html`, com título definido como:

```html
<title>CloneDisneyPlus</title>
```

E fizemos o link do CSS gerado na pasta `dist`:

```html
<link rel="stylesheet" href="./dist/css/main.css">
```

Adicionei um `<h1>` de teste e verifiquei no navegador com o Live Server — tudo funcionou corretamente.

---

### **Resumo da Aula 1**

* Analisamos o site da Disney+ para entender peculiaridades e definir o escopo do projeto;
* Iniciamos um projeto com Gulp, Sass e automação de tarefas;
* Criamos a função `styles` para compilar e minificar SCSS em CSS;
* Usamos `gulp.watch` para monitorar alterações no SCSS e compilar automaticamente;
* Configuramos o `.gitignore` para ignorar a pasta `dist`;
* Criamos o `index.html` e testamos a integração com o CSS;
* Realizamos o controle de versão com Git usando comandos ou interface gráfica.


## **Aula 2 – Crie o Hero**

### **Objetivos da aula**

* Compreender a estrutura de um componente HTML;
* Dominar o uso de classes e estilos CSS;
* Praticar a criação de seletores CSS avançados.

---

### **Análise inicial e estruturação do Hero**

Iniciamos a aula analisando o site original da Disney+ utilizando o **painel de desenvolvedor do navegador**, observando margens, espaçamentos e outros padrões visuais. A ideia era identificar elementos reutilizáveis e montar um componente Hero com base nessas referências.

O professor reforçou a importância do uso de **Sass com metodologia BEM**, separando a estilização em arquivos específicos e reaproveitando estilos com variáveis e mixins.

---

### **Estilização modular com Sass**

Criamos o arquivo `/_hero.scss` e o importamos no `main.scss` com:

```scss
@use 'hero';
```

Também criamos o arquivo `/_variaveis.scss`, para isolar as variáveis de cor e layout. Para utilizá-las nos outros arquivos Sass, é necessário importar `variaveis` com `@use 'variaveis';` em cada arquivo.

**Variáveis principais:**

```scss
$corDeFundo: #040714;
$corTextoPrincipal: silver;
$corTextoSecundario: #f9f9f9;
$corBotaoPrimario: #6421FF;
```

---

### **Mixins personalizados**

Com base na análise do site original, o professor percebeu que o `line-height` era sempre **10px maior que a `font-size`**. Para facilitar esse padrão, criamos mixins reutilizáveis:

```scss
@mixin text($font-size: 16px) {
  font-size: $font-size;
  line-height: calc($font-size + 10px);
}

@mixin title($font-size: 40px) {
  font-size: $font-size;
  line-height: calc($font-size + 10px);
  color: $corTextoSecundario;
}
```

---

### **Estrutura HTML da seção Hero**

No `index.html`, criamos a estrutura da seção Hero:

```html
<section class="hero">
  <div class="hero__content">
    <h1 class="hero__content-branding">
      <img src="images/disneyplus.svg" alt="Disney+">
    </h1>
    <h3 class="title title--small">As melhores histórias em um só lugar</h3>
    <p class="link-text">
      <a href="#">Assinar o Disney+</a>
      <span class="text">R$ 27,90/mês ou R$ 279,90/ano</span>
    </p>
    <ul class="hero__content-combos">
      <li class="hero__content-combo">
        <!-- Combo 1 -->
      </li>
      <li class="hero__content-combo">
        <!-- Combo 2 -->
      </li>
    </ul>
    <p class="text text--small">
      *O preço pode variar caso a assinatura seja feita através de outras plataformas.
    </p>
  </div>
</section>
```

---

### **Estilização dos combos**

O professor explicou que a lista de combos usa a tag `<ul>` com a classe `.hero__content-combos`, e cada item (`<li>`) representa um plano:

```scss
.hero__content-combos {
  display: flex;
  justify-content: space-between;
  gap: 24px;
}

.hero__content-combo {
  width: calc(50% - 12px);
  text-align: center;

  img {
    max-height: 60px;
  }

  button {
    width: 100%;
    font-size: 20px;
    background-color: $corBotaoPrimario;
    text-transform: uppercase;
    border-radius: 4px;
    margin-top: 2vw;
  }
}
```

O botão de chamada para ação recebeu cor, raio de borda, texto em caixa alta e largura total.

---

### **Importante: comportamento de margin no CSS**

**Resumo da propriedade `margin`:**

* 1 valor → todos os lados;
* 2 valores → topo/base, esquerda/direita;
* 3 valores → topo, lados, base.

---

### **Sobre pseudo-seletores e organização**

Ao aplicar estilos no último parágrafo (`p`) com a observação do preço, utilizamos o pseudo-seletor `:last-child`, mas como estamos em Sass, foi necessário escrever corretamente com `&`:

```scss
.hero__content p:last-child {
  margin-top: 20px;

  img {
    width: 60px;
  }
}
```

---

### **Correções e problemas encontrados**

* Algumas **imagens não foram minificadas corretamente** com `gulp-imagemin`, então usamos diretamente os arquivos originais;
* A imagem de fundo do Hero estava cortada. A correção foi feita com:

```scss
background-size: cover;
```

---

### **Resumo da Aula 2**

* Estruturamos a seção Hero do site Disney+ com imagem, título, preços e combos;
* Usamos Sass modular com variáveis e mixins reutilizáveis;
* Organizamos o CSS com metodologia BEM;
* Estilizamos botões e listas de combos com `flex`, `calc` e espaçamentos proporcionais;
* Aprendemos boas práticas com pseudo-seletores e a importância do `background-size`;
* Criamos o script `npm run dev` no `package.json` para agilizar o desenvolvimento;
* Resolvemos pequenos bugs e alinhamos a estrutura visual ao site original.

Perfeito! Aqui está a **anotação revisada da Aula 3**, com foco em ortografia e fluidez, conforme você orientou:

---

## **Aula 3 – Importe uma fonte externa**

### **Objetivos da aula**

* Compreender como importar fontes externas em um projeto web;
* Praticar o uso de controle de versão com o Git.

---

### **Por que importar fontes neste projeto?**

Neste projeto, as fontes utilizadas **não estão disponíveis no Google Fonts**. Por isso, precisamos importar manualmente os arquivos `.woff` ou `.ttf`, utilizando a funcionalidade do `@font-face` no Sass.

---

### **Importação com Sass**

Fizemos o processo de importação no arquivo principal de estilos (`main.scss` ou `variaveis.scss`). A fonte utilizada foi a **Avenue**.

#### Fonte regular (peso normal):

```scss
@font-face {
  font-family: 'Avenue';
  src: url('../../assets/fonts/avenue-regular.woff') format('woff');
  font-weight: 400;
}
```

#### Fonte bold:

```scss
@font-face {
  font-family: 'Avenue';
  src: url('../../assets/fonts/avenue-bold.woff') format('woff');
  font-weight: 700;
}
```

Com os `@font-face` definidos, podemos utilizar a fonte normalmente no restante do projeto:

```scss
body {
  font-family: 'Avenue', sans-serif;
}
```

---

### **Uso com fallback**

Para garantir que o texto continue legível mesmo que a fonte personalizada não carregue, aplicamos um **fallback**, como `sans-serif`:

```scss
font-family: 'Avenue', sans-serif;
```

---

### **Controle de versão com Git**

Ao final da aula, o professor reforça o uso do **Git** para controle de versão, como já feito em aulas anteriores:

1. `git status` – verificar alterações;
2. `git add .` – adicionar arquivos;
3. `git commit -m "feat: adiciona fonte externa Avenue"` – criar um commit;
4. `git push` – enviar ao repositório remoto.

Ele também explicou que o Git mostra, no terminal, **comparações entre o repositório local e o remoto**, informando se os arquivos estão atualizados, pendentes ou em conflito.

Claro! Aqui está o texto unificado da **Aula 4 – Crie a sessão de atrações**, seguindo o modelo estruturado da **Aula 1 - Configuração Grunt**:

---

## Aula 4 – Crie a sessão de atrações

### Objetivos da aula

* Criar a seção de **atrações** com base no layout original da Disney+;
* Implementar uma **navegação por abas** usando JavaScript puro;
* Aplicar manipulação de classes e atributos personalizados para controlar o conteúdo visível.

---

### Etapas da construção

#### 1. Criar a estrutura HTML

Foi criada uma nova seção `<section class="shows">`, que representa os shows e atrações. Dentro dela, adicionamos:

* Um `<nav class="shows__tabs">` com **três botões**;
* Três listas `<ul>` com a classe `shows__list`, contendo os itens de cada categoria.

Cada botão possui o atributo personalizado `data-tab-button`, enquanto cada lista usa `data-tab-id` para permitir a **ligação entre botões e conteúdo**:

```html
<button data-tab-button="em_breve" class="shows__tabs__button shows__tabs__button--active">Em breve</button>
<ul data-tab-id="em_breve" class="shows__list shows__list--active">...</ul>
```

**A ideia é que ao clicar em um botão, a lista correspondente seja exibida, e as demais, ocultadas.**

---

#### 2. Implementar a lógica com JavaScript

Toda a lógica de interação foi implementada com JavaScript puro, começando com um `addEventListener` escutando o carregamento do DOM:

```js
document.addEventListener('DOMContentLoaded', function () {
    const buttons = document.querySelectorAll('[data-tab-button]');
    ...
});
```

Esse trecho garante que o código só será executado após o carregamento completo do HTML.

#### 3. Associar os botões às listas

Dentro do loop `for`, associamos cada botão à lista correspondente por meio do `dataset`:

```js
buttons[i].addEventListener('click', function(botao) {
    const abaAlvo = botao.target.dataset.tabButton;
    const aba = document.querySelector(`[data-tab-id="${abaAlvo}"]`);
    ...
});
```

A lógica completa realiza quatro ações:

1. **Esconder todas as abas** com `escondeTodasAbas()`;
2. **Mostrar a aba selecionada** com `classList.add('shows__list--active')`;
3. **Remover destaque dos outros botões** com `removeBotaoAtivo()`;
4. **Adicionar destaque ao botão atual** com `classList.add('shows__tabs__button--active')`.

---

### Funções auxiliares

As funções `escondeTodasAbas` e `removeBotaoAtivo` são responsáveis por limpar os estilos anteriores antes de aplicar os novos:

```js
function escondeTodasAbas() {
    const tabsContainer = document.querySelectorAll('[data-tab-id]');
    for (let i = 0; i < tabsContainer.length; i++) {
        tabsContainer[i].classList.remove('shows__list--active');
    }
}

function removeBotaoAtivo() {
    const buttons = document.querySelectorAll('[data-tab-button]');
    for (let i = 0; i < buttons.length; i++) {
        buttons[i].classList.remove('shows__tabs__button--active');
    }
}
```

Essas funções garantem que apenas uma aba e um botão fiquem ativos por vez.

---

### Estilização extra

Para reforçar visualmente a aba ativa, foi adicionada uma **borda inferior nos botões**. Essa borda:

* Fica **transparente** quando a aba não está selecionada;
* Torna-se **colorida** quando o botão recebe a classe `shows__tabs__button--active`.

Esse pequeno detalhe melhora a experiência do usuário ao indicar claramente qual aba está aberta.

---

### Comentário final

Durante os testes, foi observado que a interpolação de atributos com crase (\`\`) funcionou mesmo **sem aspas ao redor do seletor**, como:

```js
document.querySelector([data-tab-id=${abaAlvo}]);
```

Apesar disso, é recomendado utilizar aspas para manter a clareza e evitar possíveis problemas de interpretação por parte do navegador.

Perfeito! Aqui está o **texto unificado da Aula 5 – Criar Listagem de Planos**, seguindo exatamente o padrão que você indicou anteriormente:

---

## **Aula 5 – Criar Listagem de Planos**

### **Objetivos da aula**

* Criar uma seção de listagem de planos em um site;
* Estilizar elementos de acordo com as diretrizes de design;
* Utilizar seletores CSS e classes para organização visual.

---

### **Criação da nova seção**

A estrutura da aula começou com a criação de uma nova `section` com a classe `plans`, responsável por exibir os planos de assinatura. Essa seção possui um **título centralizado**, com a frase:

```html
<h2 class="title">Escolha o seu plano</h2>
```

Logo abaixo, foi criado um elemento `<ul class="plans__list">`, que representa a lista de planos disponíveis.

Cada item da lista (`<li class="plans__list__item">`) possui a seguinte estrutura:

* Uma imagem (`<img>`) com o logo dos serviços;
* Um preço dentro de uma tag `<strong class="title--small">`;
* Uma breve descrição em um `<p>`;
* Um botão com a chamada para ação.

Essa estrutura foi repetida três vezes, representando três opções de planos: Disney+, Disney+ + Star+, e Disney+ + Star+ + Starzplay. O plano do meio possui um **destaque especial**, com o selo de "Mais popular" em formato de imagem.

---

### **Estilização da seção**

Para organizar os estilos, foi criado um novo arquivo SASS:

```
/src/styles/plans.scss
```

Esse arquivo foi importado no `main.scss` com a linha:

```scss
@use 'plans';
```

#### Estilos principais aplicados

* `.plans` recebeu `padding: 5.6vw`;
* O `h2` foi centralizado com `text-align: center`;
* `.plans__list` recebeu `display: flex` e `justify-content: space-around` para distribuir igualmente os itens;
* Cada `.plans__list__item` recebeu:

  * `text-align: center` para alinhar os conteúdos;
  * `padding` interno;
  * `max-width: 380px` e `width: 100%` para limitar a largura;
  * `img { height: 83px; }` para manter as proporções da logo;
  * `strong { display: block; }` para separar o preço dos demais conteúdos.

Além disso, foi utilizado o seletor `strong + button` com `margin-top: 24px` para criar um espaçamento mais agradável entre o valor e o botão.

---

### **Botão reutilizável**

Inicialmente, o botão tinha estilos definidos apenas dentro do bloco `hero`, mas ao perceber que ele seria reutilizado em outras partes do projeto, foi criada uma **classe genérica** chamada `.button`.

O estilo foi movido para o `main.scss`, e a classe `.button` foi aplicada em todos os botões do projeto para manter a consistência visual.

---

### **Destaque do plano central**

Para aplicar o fundo personalizado no segundo item da lista (o plano mais popular), foi utilizado o seletor CSS:

```scss
.plans__list__item:nth-child(2) {
  background-image: url('../../images/fundo-combo-plus.png');
  background-repeat: no-repeat;
  background-size: contain;
}
```

Com isso, o segundo plano ganhou o selo de "Mais popular", com um fundo roxo que simula o destaque da plataforma oficial.

---

### **Ajustes finais e espaçamentos**

* Foi adicionado `margin-top: 50px` em `.plans__list` para afastar os planos do título;
* Em `.shows`, foi aplicado `min-height: 50vw` para garantir espaçamento entre as seções;
* A estrutura textual também foi revisada para simular a disposição original do site da Disney+, com quebras de linha que ajudam na leitura.

---

### **Resumo da Aula 5**

* Criamos a seção de listagem dos planos com título e três opções de assinatura;
* Organizamos os itens com `flexbox` e definimos limites de largura;
* Centralizamos textos e imagens, e aplicamos destaque no plano central com `nth-child`;
* Padronizamos o estilo dos botões usando a classe `.button`;
* Corrigimos espaçamentos verticais entre as seções e os elementos internos;
* Finalizamos com uma seção visualmente próxima da versão real do site da Disney+.

Perfeito! Aqui está o **texto unificado da Aula 6 – Crie a seção "Assista do seu jeito"**, seguindo o mesmo padrão das aulas anteriores:

---

## **Aula 6 – Crie a seção "Assista do seu jeito"**

### **Objetivos da aula**

* Criar uma seção de conteúdo com imagem e texto;
* Aplicar estilo CSS para formatar a seção de forma responsiva e organizada.

---

### **Criação da estrutura**

Nesta aula, criamos uma nova `section` com a classe `watch-anywhere`, responsável por apresentar a flexibilidade de acesso ao conteúdo da Disney+ em diversos dispositivos.

A estrutura interna da `section` foi composta por dois elementos principais:

1. **Imagem ilustrativa**: mostra uma composição de vários dispositivos (TV, computador, tablet, celular), todos exibindo a interface da Disney+;
2. **Texto promocional**: posicionado ao lado da imagem, com um título de destaque e uma descrição explicativa sobre a versatilidade do serviço.

O conteúdo textual inclui:

* Um título com a frase: `Assista do seu jeito`;
* Um parágrafo descrevendo a possibilidade de assistir à Disney+ em diversos aparelhos;
* Uma ênfase na qualidade e diversidade de conteúdos disponíveis.

---

### **Estilização com SASS**

Para estilizar essa nova seção, foi criado um novo arquivo SASS:

```
/src/styles/watch-anywhere.scss
```

Esse arquivo foi importado no `main.scss` da seguinte forma:

```scss
@use 'watch-anywhere';
```

#### Estilos aplicados

* `.watch-anywhere` recebeu `padding: 5.6vw` para manter a consistência com as outras seções;
* Foi aplicado `display: grid` com `grid-template-columns: 1fr 1fr`, dividindo a seção em duas colunas de tamanho igual;
* `align-items: center` centralizou verticalmente os elementos;
* A imagem foi limitada com `max-width: 100%` para evitar que ultrapasse os limites da coluna e manter a estética do layout.

#### Container de texto

Seguindo a metodologia BEM, o bloco de texto foi encapsulado na classe:

```html
<div class="watch-anywhere__text-container">
```

E recebeu os seguintes estilos:

* `padding: 0 2vw`;
* `margin: 24px 0` em títulos e parágrafos internos.

---

### **Dificuldades enfrentadas**

Durante a construção da aula, algumas dificuldades surgiram:

1. **Erros de digitação nas classes**: por conta da atenção reduzida, vários nomes de classes foram escritos incorretamente no HTML ou SASS, causando falhas de renderização;
2. **Esquecimento de salvar ou ativar automação**: em certo momento, as alterações feitas no SASS não estavam sendo refletidas no CSS final. Isso ocorreu porque o comando de observação do Gulp (`npm run dev`) não havia sido ativado, impedindo a compilação automática;
3. **Problemas ao salvar o arquivo**: houve um momento em que o CSS foi fechado antes de concluir uma edição, o que interrompeu a aplicação de estilos.

Esses erros, somados à baixa concentração, fizeram com que uma aula curta de aproximadamente 7 minutos levasse cerca de 1 hora para ser finalizada. Apesar das dificuldades, a aula foi concluída com sucesso, e os erros serviram como reforço da importância da atenção aos detalhes e da prática constante.

---

### **Resumo da Aula 6**

* Criamos a seção "Assista do seu jeito" com imagem e texto promocional;
* Utilizamos `grid` para organizar os elementos lado a lado;
* Estilizamos com SASS, mantendo padrões de espaçamento e responsividade;
* Aplicamos a metodologia BEM na nomeação das classes;
* Corrigimos erros comuns de digitação e configuração do Gulp;
* Concluímos a seção, reforçando o aprendizado prático mesmo diante das dificuldades.

Claro! Aqui está o **texto unificado da Aula 7 – Crie a sessão Dispositivos Disponíveis**, com as correções feitas com base no código que você compartilhou:

---

## Aula 7 – Crie a sessão Dispositivos Disponíveis

### Objetivos

* Reestruturar uma sessão de conteúdo com imagem e lista.
* Aplicar estilos CSS/Sass para formatar a seção.
* Resolver problemas de alinhamento e vazamento de conteúdo.

---

### Introdução

Nesta aula, o objetivo foi criar e estilizar a seção "Disponível nos seus dispositivos favoritos", explorando diferentes estratégias de layout e posicionamento. Também aprendemos a refatorar o CSS para torná-lo mais genérico e reutilizável no projeto.

---

### Estrutura HTML

Criamos uma nova `<section>` com a classe `.availabes-devices`, contendo:

* Um título `<h2 class="title">Disponível nos seus dispositivos favoritos</h2>`;
* Uma lista `<ul class="availabes-devices__list">` com quatro `<li>`s, representando:

  * Computador
  * TV
  * Videogame
  * Celular

Cada `<li>` contém:

* Uma imagem ilustrativa do dispositivo;
* Um título com classe `.title--small`;
* Uma lista `<ul class="text">` com os sistemas/plataformas suportados.

---

### Estilização com Sass

O estilo da seção foi escrito dentro de um bloco `.availabes-devices`, usando a estrutura aninhada do Sass:

```scss
.availabes-devices {
  padding: 5.6vw;

  h2 {
    text-align: center;
    margin-bottom: 24px;
  }

  &__list {
    display: grid;
    gap: 24px;
    grid-template-columns: repeat(4, 1fr);

    &__item {
      text-align: center;

      img {
        max-width: 100%;
      }

      h4 {
        margin-top: 20px;
        margin-bottom: 24px;
      }

      li {
        list-style-type: none;
      }
    }
  }
}
```

**Destaques da estilização:**

* Uso de `grid-template-columns: repeat(4, 1fr)` com `gap: 24px` para organizar os dispositivos em 4 colunas com espaçamento;
* Centralização de texto e imagens;
* Remoção dos marcadores de lista (`list-style-type: none`);
* Margens para o título `<h4>` ajustadas para melhor espaçamento vertical.

---

### Dificuldades e Aprendizados

Durante a aula, surgiram alguns erros comuns, como esquecer o `s` em `devices`, aplicar a classe `.text` diretamente nos `<li>` ao invés da `<ul>`, ou esquecer de iniciar o processo de automação do Sass com o Gulp, o que causou confusão ao não ver as alterações sendo aplicadas no CSS.

Além disso, o professor reforçou a importância de utilizar nomes de classes genéricos como `.image-text-section`, permitindo o reaproveitamento em outras seções, ao invés de nomes específicos como `.watch-anywhere`. Também aprendemos a aplicar modificadores com a metodologia BEM, como em `.image-text-section--image-full-width`.

Por fim, o professor explicou a diferença entre usar `grid-template-columns: repeat(4, 1fr)` e porcentagens fixas como `25%`. Ao utilizar `1fr`, o `gap` entre as colunas é automaticamente levado em consideração, evitando vazamentos de conteúdo.

---

### Conclusão

Apesar da aula ter um vídeo curto, o tempo de execução foi maior por conta dos ajustes finos de CSS, práticas com Sass, metodologia BEM e entendimento do grid layout. No fim, conseguimos montar uma seção visualmente bonita, responsiva e funcional para mostrar os dispositivos compatíveis com o serviço da Disney+.

Claro! A seguir está uma versão mais **detalhada e narrativa** do texto unificado da **Aula 8 – Crie o FAQ**, com foco no **processo prático, dificuldades enfrentadas**, e os **ajustes feitos ao longo do caminho**, como você tem preferido nas aulas anteriores:

---

## Aula 8 – Crie o FAQ

### Objetivos da Aula

Nesta aula, nosso principal objetivo foi desenvolver uma seção de **Perguntas Frequentes (FAQ)** para o site, semelhante à que aparece no Disney+. A proposta envolvia três frentes principais:

1. Compreender a **estrutura HTML** necessária para um componente do tipo *accordion*;
2. Estilizar esse conteúdo com **Sass/CSS**, usando boas práticas e transições suaves;
3. Implementar com **JavaScript** a lógica de abrir e fechar as respostas dinamicamente.

Além disso, exploramos conceitos como **componentes reutilizáveis**, **nomenclatura BEM**, e ajustes visuais inspirados diretamente no site original.

---

### Estruturação HTML

Iniciamos criando uma `section` com a classe `faq`. Dentro dela, colocamos um `h2` com a classe `title` e o conteúdo "Perguntas Frequentes". Abaixo desse título, montamos uma lista `ul` com a classe `faq__questions`, e dentro dela adicionamos múltiplos `li` com a classe `faq__questions__item`.

Cada item (`faq__questions__item`) é dividido em duas partes:

* `faq__questions__item__question`: é o título da pergunta clicável;
* `faq__questions__item__answer`: é o parágrafo que contém a resposta, inicialmente escondido.

Essa estrutura nos permitiu controlar cada par de pergunta e resposta de forma separada, facilitando tanto a estilização quanto a interação via JS.

---

### Estilização com Sass/CSS

#### Estilo básico e organização

Usamos a metodologia **BEM** para manter os nomes das classes consistentes e compreensíveis. A `section.faq` recebeu um espaçamento padrão de `padding: 5.6vw`, mantendo a harmonia com outras seções da página. O título `.title` foi centralizado com `text-align: center`.

Cada item da lista (`faq__questions__item`) foi posicionado com `margin`, `padding` e cores adequadas. A cor de fundo foi definida como `#13151D`, e depois convertida em uma variável para reutilização futura.

#### Esconder e revelar a resposta

No CSS, configuramos a resposta (`faq__questions__item__answer`) para ter `height: 0` e `overflow: hidden`, garantindo que ficasse oculta por padrão. Quando quisermos mostrar essa resposta, utilizamos um **modificador**: `faq__questions__item--is-open`, que altera a `height` para `auto`.

Para melhorar a experiência visual, adicionamos uma **transição CSS**:

```css
transition: height 0.5s ease;
```

Ela foi aplicada tanto no estado base quanto no modificador, permitindo uma animação suave na abertura e no fechamento.

#### Sinais de + e –

Para simular o comportamento de abrir e fechar, como no site da Disney, utilizamos o seletor `::after` na `__question`. Com isso, adicionamos dinamicamente os sinais:

* `+` quando o conteúdo está recolhido;
* `–` quando o conteúdo está visível.

Essa alternância foi feita combinando o estado da classe com as regras de CSS, usando propriedades como `content`, `position: absolute`, `top`, `right`, `font-size`, `display: flex` e `align-items: center`. Também aplicamos `position: relative` no contêiner pai para posicionar corretamente o ícone.

---

### Implementação com JavaScript

#### Seleção de elementos

Para iniciar a lógica do *accordion*, criamos uma constante:

```js
const questions = document.querySelectorAll('[data-faq-question]');
```

Esse seletor pegou todos os elementos com o atributo `data-faq-question`, que adicionamos manualmente em cada `div` de pergunta no HTML. Essa foi uma etapa importante, pois inicialmente esquecemos de adicionar o `data-faq-question`, o que gerou dúvidas até percebermos o erro.

#### Manipulação de eventos

A seguir, criamos um `for` tradicional para percorrer os elementos e adicionamos um **event listener**:

```js
for (let i = 0; i < questions.length; i++) {
  questions[i].addEventListener('click', abreOuFechaResposta);
}
```

Criamos então a função `abreOuFechaResposta`, que:

1. Captura o `target` do clique;
2. Acessa o `parentNode` (elemento pai da pergunta), pois é ele que precisa da classe `--is-open`;
3. Usa o método `classList.toggle()` para alternar essa classe.

Isso ativa a transição, muda o sinal de + para –, e revela ou oculta a resposta.

#### Lidando com erros

Durante esse processo, enfrentamos algumas dificuldades:

* Inicialmente, usamos o nome errado para classes e seletores, como esquecer o `__` da nomenclatura BEM;
* A transição CSS não funcionava porque colocamos uma vírgula indevida entre os valores de `transition` — corrigimos para:

  ```css
  transition: height 0.5s ease;
  ```
* Tivemos dúvidas sobre como a função `toggle` atuava, até entendermos que ela adiciona ou remove dinamicamente a classe indicada.

Esses pequenos tropeços fizeram parte do aprendizado e nos mostraram a importância de prestar atenção aos detalhes, principalmente quando se trabalha com múltiplas camadas (HTML, CSS e JS).

---

### Inserção de conteúdo real

Ao final da aula, começamos a preencher o conteúdo real do FAQ com perguntas e respostas reais do Disney+, como:

* O que está incluído no Disney+?
* Como faço para assistir offline?

Para cada pergunta, criamos um novo `li` e seguimos o mesmo padrão de estrutura HTML. As respostas foram inseridas em parágrafos `p`, com a classe `.text--big` aplicada para harmonizar visualmente.

---

### Conclusão

A construção do componente FAQ nos ajudou a exercitar várias habilidades de frontend ao mesmo tempo:

* Estruturação semântica e reutilizável com HTML e BEM;
* Estilização consistente e animada com Sass;
* Lógica de interação com JavaScript puro, manipulando classes e eventos.

Apesar dos desafios — como erros de digitação, problemas de nomenclatura e dificuldades com transições — conseguimos entregar um componente funcional, esteticamente agradável e de comportamento fluido.

Esse tipo de seção é bastante comum em sites e portais, e agora dominamos sua criação tanto do ponto de vista visual quanto funcional.

Ótimo! Com base nesse código SCSS fornecido, fiz uma atualização no **texto unificado da Aula 9** para alinhar completamente com a implementação real do `footer.scss`. Abaixo está a versão **revisada e completa**, já incorporando o uso correto das variáveis, seletores e estrutura BEM conforme o seu código:

---

## Aula 9 – Crie o rodapé

### Objetivos

* Compreender a estrutura e os elementos de um rodapé web;
* Aprender a estilizar elementos de um rodapé com CSS/SASS;
* Praticar o uso de variáveis CSS para gerenciar estilos com consistência.

---

### Introdução

Nesta aula, criamos o rodapé (footer) do projeto, inspirado na estrutura do site da Disney+. O foco foi consolidar a prática com componentes reutilizáveis, estrutura semântica e estilização utilizando SASS com organização BEM e variáveis SCSS para manter padronização visual.

---

### Estrutura HTML

O rodapé foi construído com a seguinte estrutura semântica:

```html
<footer class="footer">
  <div class="footer__container">
    <img class="footer__logo" src="..." alt="Logo Disney+" />

    <ul class="footer__links">
      <li class="footer__links__item"><a href="#">Termos e Condições de Uso</a></li>
      <li class="footer__links__item"><a href="#">Política de Privacidade</a></li>
      <li class="footer__links__item"><a href="#">Proteção de Dados no Brasil</a></li>
      <li class="footer__links__item"><a href="#">Anúncios Personalizados</a></li>
      <li class="footer__links__item"><a href="#">Dispositivos Compatíveis</a></li>
      <li class="footer__links__item"><a href="#">Ajuda</a></li>
      <li class="footer__links__item"><a href="#">Sobre o Disney+</a></li>

      <li class="footer__links__item language-selector">
        <img src="globo.png" alt="Ícone de globo" />
        <select>
          <option selected>Português</option>
          <option>English</option>
        </select>
      </li>
    </ul>

    <p>© Disney. Todos os direitos reservados.</p>
    <p>Serviço de assinatura pago. Conteúdo sujeito à disponibilidade.</p>
  </div>
</footer>
```

---

### Estilização com SCSS

A estilização foi dividida entre os blocos de BEM para o footer e um seletor especial `.language-selector`.

#### Estrutura geral

```scss
.footer {
  padding: 26px 8px;

  &__container {
    max-width: 1024px;
    width: 100%;
    margin: 0 auto;
    text-align: center;
    padding-bottom: 20px;
  }

  &__logo {
    width: 80px;
    margin: 0 auto 10px;
  }

  &__links {
    display: flex;
    align-items: center;
    flex-wrap: wrap;
    justify-content: center;
    margin-bottom: 16px;

    &__item {
      display: inline-block;

      a {
        text-decoration: none;
        padding: 8px 16px;
        display: block;

        &:hover {
          color: variaveis.$corTextoSecundario;
        }
      }
    }
  }

  p {
    margin-bottom: 16px;
  }
}
```

#### Seletor de idioma (`.language-selector`)

Esse seletor, responsável por exibir o ícone de idioma e o dropdown `<select>`, foi construído de forma separada para garantir sua responsividade e alinhamento:

```scss
.language-selector {
  display: flex;
  align-items: center;

  img {
    width: 18px;
    height: 18px;
  }

  select {
    background-color: transparent;
    border: none;
    color: variaveis.$corTextoPrincipal;
    height: 38px;

    &:hover {
      color: variaveis.$corTextoSecundario;
    }

    option {
      color: variaveis.$corDeFundo;
    }
  }
}
```

---

### Considerações finais

Durante o processo, alguns ajustes importantes foram feitos para alinhar os elementos visualmente:

* O `<select>` inicialmente ficou desproporcional, e foi necessário definir uma altura fixa de `38px`;
* Para garantir o alinhamento vertical entre os links e o seletor, aplicamos `display: flex`, `align-items: center` e `flex-wrap: wrap` na lista principal;
* Foi reforçado o uso da metodologia BEM para nomear classes e manter organização;
* As cores e estilos em `hover` foram definidos utilizando variáveis SCSS já criadas, mantendo consistência com o restante do projeto.

Aqui está o **Texto Final Unificado da Aula 10 – Crie o Cabeçalho**, seguindo o padrão das aulas anteriores:

---

# Aula 10 – Crie o Cabeçalho

## Objetivos da Aula

* Compreender a estrutura HTML e CSS do cabeçalho.
* Aplicar técnicas de responsividade.
* Implementar comportamentos interativos com JavaScript.

---

## Introdução

Nesta aula, o objetivo principal foi construir o cabeçalho (header) do nosso site, seguindo o layout de referência do Disney+ em sua versão de 2022. Além da estrutura HTML e das estilizações com CSS/SASS, também aprendemos a adicionar interatividade por meio de JavaScript, com foco em exibir e ocultar elementos com base na rolagem da página.

---

## Estrutura HTML do Cabeçalho

A estrutura básica do header começou com a criação da tag `<header>`, com a classe `.header`. Dentro dela, criamos um container `.header__container` que abriga os principais elementos:

* **Logo Disney+**: Agora posicionado dentro de um `<h1>`, com a classe `.header__logo`, para refletir a importância semântica na hierarquia HTML e melhorar o SEO.
* **Lista de Links (`<ul class="header__links">`)**: Contendo dois botões:

  * Um botão com a classe `.button .button--secondary` para o "Assine Agora".
  * Outro com a classe `.button .button--dark` para o "Entrar".

A lista de links utilizou a classe `.header__links__item` para estilização de cada item.

---

## Estilização com CSS/SASS

Criamos o arquivo `header.scss` e o importamos no `main.scss`.

### Principais Estilizações:

* **Container**: Usamos `display: flex` com `justify-content: space-between` para posicionar a logo à esquerda e os botões à direita.
* **Botões**: Criamos dois modificadores:

  * `.button--secondary`: Fundo azul.
  * `.button--dark`: Fundo preto com opacidade de 80%.

Além disso:

* Todos os botões receberam `border: 1px solid` para garantir tamanho uniforme entre eles.
* Adicionamos um `letter-spacing: 1px` para dar o espaçamento entre as letras, como no site da Disney.
* Removemos margens e paddings padrões usando o reset CSS.

### Hover nos Botões:

* Nos botões `.button--primary` e `.button--secondary`, usamos a função SASS `lighten()` para clarear a cor em 3% no hover.
* No botão `.button--dark`, ao passar o mouse, a cor de fundo se torna branca, e o texto assume a cor do fundo escuro original, criando contraste.

---

## Implementação da Responsividade e Efeito Sticky

Para fazer o menu fixo durante o scroll:

* Adicionamos `position: sticky; top: 0; left: 0; z-index: 1;` à classe `.header`.

Assim, o cabeçalho permanece visível no topo da tela enquanto o usuário navega.

---

## Comportamento Interativo com JavaScript

O professor nos guiou na criação de um comportamento onde, ao rolar a página, o logo da Disney e o botão "Assine Agora" desaparecem quando estamos na Hero Section.

### Lógica Implementada:

1. Criamos uma constante `heroSection` para capturar a altura da Hero usando `clientHeight`.
2. Monitoramos o scroll da janela usando:

```javascript
window.addEventListener('scroll', function() {
    const posicaoAtual = window.scrollY;
});
```

3. Criamos duas funções para controle de visibilidade:

```javascript
function ocultaElementos() {
    header.classList.add('header__is-hidden');
}

function exibeElementos() {
    header.classList.remove('header__is-hidden');
}
```

4. No evento de scroll, usamos uma condicional eficiente:

```javascript
if (posicaoAtual < alturaHero) {
    ocultaElementos();
} else {
    exibeElementos();
}
```

> Essa abordagem (condição apenas para ocultar dentro da Hero) reduziu o número de execuções da função e melhorou a performance.

---

## Ajustes Finais de CSS para a Classe `.header__is-hidden`

Quando o header recebia a classe `.header__is-hidden`, aplicamos as seguintes alterações:

* No logo e no botão "Assine Agora":

  * `opacity: 0`
  * `visibility: hidden`
  * Efeito de transição suave com `transition: opacity 0.5s ease;`

Além disso, usamos o seletor `:first-child` para garantir que somente o primeiro botão (o secundário) fosse afetado.

---

## Solução de Problemas Durante a Aula

Durante a implementação, enfrentamos alguns problemas comuns:

* Esqueci de colocar o ponto nas classes no SASS, o que fez com que o CSS não aplicasse corretamente.
* Tive erro de digitação em `window.scrollY` (escrevi como `windown`).
* Também deixei de colocar unidades como o `s` em `0.5s` na transição.
* Precisei lembrar de adicionar o `z-index: 1` ao header para evitar sobreposição com o conteúdo da Hero.

Esses ajustes foram essenciais para que o efeito final de esconder/mostrar o cabeçalho funcionasse como esperado.

---

## Conclusão

Com isso, finalizamos a construção completa do cabeçalho do site, com estruturação semântica, estilização consistente com o layout de referência, e comportamentos interativos controlados via JavaScript.

Agora o menu responde ao scroll, os elementos aparecem e desaparecem de forma suave, e a experiência de navegação fica mais próxima do padrão profissional usado no site da Disney+.

Aqui está o **texto unificado da Aula 11 – Realize o Deploy**, seguindo o estilo das aulas anteriores:

---

# Aula 11 – Realize o Deploy

## Objetivos da Aula

* Compreender a importância da responsividade no design web.
* Aprender a aplicar estilos responsivos usando CSS e media queries.
* Praticar o processo de deploy de um projeto web utilizando a Vercel.

---

## Introdução

Nesta aula, o foco principal foi finalizar o projeto do clone da Disney+, ajustando a responsividade do site para dispositivos móveis e tablets, otimizando o JavaScript através de minificação, e por fim realizando o deploy do projeto utilizando a Vercel.

---

## Ajustes de Responsividade

O primeiro passo foi realizar uma revisão completa do layout em diferentes dispositivos usando a aba de desenvolvedor do navegador. Inicialmente enfrentei dificuldades ao testar o site na visão mobile, pois a aba de desenvolvedor não estava exibindo a visualização corretamente. Após resolver o problema com o navegador, comecei os ajustes.

### Principais ajustes feitos:

* **Seção de Planos (Plans):**

  * No desktop, os planos estavam lado a lado (grid de 3 colunas).
  * No mobile, aplicamos um `display: block` para empilhar os itens verticalmente.
  * No tablet, ajustamos o alinhamento central usando `margin: 0 auto`.

* **Hero – Ajuste da imagem de fundo:**

  * Aumentei manualmente o `padding-top`, mas depois o professor apresentou uma solução mais precisa:

  ```css
  padding-top: calc(50vw + 75px);
  ```

  * Isso garantiu que o espaçamento do conteúdo ficasse proporcional ao tamanho da tela.

* **Seção de Filmes e Séries (Responsividade da Imagem):**

  * Implementamos a tag `<picture>` com `<source>` e media queries para carregar imagens diferentes conforme o tamanho da tela.
  * Estrutura utilizada:

  ```html
  <picture>
    <source media="(max-width: 768px)" srcset="src/images/mobile/filmes_series_mobile.png">
    <img src="src/images/fundo_rei_leao.png" alt="Imagem Filme Rei Leão">
  </picture>
  ```

* **Font Sizes responsivos:**

  * Criamos media queries para ajustar o tamanho das fontes:

    * `.title--big`: 15px
    * `.title`: 14px
    * `.title--small`: 12px
    * Textos normais: 11px ou 14px, dependendo do contexto.

* **Seção de Shows (Tabs):**

  * Para evitar que as abas quebrassem linha no mobile, adicionamos:

  ```scss
  white-space: nowrap;
  overflow-x: auto;
  ```

* **Seção de Combos:**

  * No mobile, cada combo passou a ter `display: block`, empilhando verticalmente os planos.

* **Header e Botões:**

  * Para os botões no mobile, ajustamos:

  ```scss
  font-size: 13px;
  padding: 8px 15px;
  height: 40px;
  letter-spacing: 1px;
  ```

* **Geral:**

  * Ajustamos margens, espaçamentos e aplicamos `media queries` adicionais nas seções de título e containers.

---

## Minificação de JavaScript com UglifyJS

O professor mostrou como minificar o JavaScript para reduzir o tamanho do arquivo e melhorar o desempenho.

### Passos seguidos:

1. Instalamos o **UglifyJS** como dependência de desenvolvimento:

```bash
npm i --save-dev uglify-js
```

2. Criamos uma task específica no Grunt para minificar o arquivo `main.js`:

```js
uglify: {
  dist: {
    files: {
      'dist/js/main.min.js': ['src/main.js']
    }
  }
}
```

3. Corrigimos um erro de sintaxe (havia escrito `scripts` em vez de `script` no caminho do Grunt).

4. Atualizamos o HTML para apontar para o novo arquivo minificado:

```html
<script src="dist/js/main.min.js"></script>
```

---

## Configuração de Deploy na Vercel

Por fim, o professor nos orientou sobre como fazer o deploy do projeto utilizando a **Vercel**, com atenção especial por ser um projeto que utiliza o Grunt como pré-processador.

### Configuração feita na Vercel:

* **Build Command:**

```
npm run build
```

* **Output Directory:**

```
dist/
```

* Criamos o script de build no `package.json`:

```json
"scripts": {
  "build": "grunt"
}
```

### Problemas enfrentados:

* No primeiro deploy, houve erro por conta de um comando digitado errado. Corrigi rapidamente e o segundo deploy funcionou perfeitamente.

* O site foi publicado com sucesso. A versão final já está online e responsiva.

---

## Considerações Finais

A experiência foi bem completa: além de trabalhar com HTML, CSS (SASS), JavaScript e Grunt, também passamos por tópicos de **responsividade**, **minificação de código** e **deploy em ambiente real**.

Apesar de alguns pequenos detalhes de alinhamento ou espaçamento ainda poderem ser refinados, o resultado geral foi muito positivo. O site está disponível tanto para desktop quanto para dispositivos móveis.

> Próximo passo: começar o exercício final do módulo.
