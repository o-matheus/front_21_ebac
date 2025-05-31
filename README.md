# Módulo 21 - Projeto 4

## Menu 
[Aula 01 - Inicie o Projeto ](#aula-1--inicie-o-projeto)  
[Aula 02 - Crie o Hero ](#aula-2--crie-o-hero)  
[Aula 03 - ]()  
[Aula 04 - ]()  
[Aula 05 - ]()  
[Aula 06 - ]()  
[Aula 07 - ]()  
[Aula 08 - ]()  
[Aula 09 - ]()  
[Aula 10 - ]()  
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

Ótimo! Finalizei a revisão completa das suas anotações até aqui. Abaixo está o **texto final da Aula 2 – Crie o Hero**, seguindo exatamente o modelo da Aula 1:

---

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
