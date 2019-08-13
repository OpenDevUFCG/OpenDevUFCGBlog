---
title: Criando malhas simples com CSS Grid
published: false
description: Introdução a CSS grid
tags: css, grid, ptbr, tips
---


# Grid para ser feliz

Você conhece o CSS Grid? Se não, você está perdendo uma ótima ferramenta do CSS que facilita tanto o desenvolvimento como também a manutenção de código. Nesse post, irei tratar um pouco de seu funcionamento e de como ele pode ser poderoso.

# Um pouco sobre caixas

Pra começar, você tem que abstrair que todo elemento HTML está envolvido por uma caixa que tanto o delimita como também o seu conteúdo (para saber mais, acesse [esse link](https://tableless.github.io/iniciantes/manual/css/box-model.html) e [esse link](https://developer.mozilla.org/pt-BR/docs/Web/CSS/box_model)). Na maioria das vezes, vão existir elementos que têm dentro do seu conteúdo outros elementos HTML, ou seja, caixas dentro de caixas, e o que o Grid faz é simplesmente organizar essas caixas de acordo com sua vontade. Como ele faz isso? Ele define uma malha quadriculada e atribui espaços dessa malha a cada elemento HTML.

![](https://thepracticaldev.s3.amazonaws.com/i/58gv9hkeys3de94e0amo.png)
*Fonte: [SparkBox](https://seesparkbox.com/foundry/css_grid_layout_guide_with_flexbox_fallbacks)*

A malha é definida por trackers que são basicamente delimitadores das linhas e colunas, que são esses marcadores brancos:

![](https://thepracticaldev.s3.amazonaws.com/i/kldxli86fo9f6mcgoi4r.jpg)
*Fonte: [SparkBox](https://seesparkbox.com/foundry/css_grid_layout_guide_with_flexbox_fallbacks)*

A linha é a área entre um tracker e outro de forma vertical, ou seja, cada novo espaço na altura da malha. Já a coluna é a área entre um tracker e outro de forma horizontal, ou seja, cada novo espaço no comprimento da malha.

As linhas e colunas são geralmente medidas em `fr`. Essa medida foi criada exclusivamente para o CSS Grid e pode ser definida como "uma fração do espaço disponível no container do grid" (fonte: [MDN](https://developer.mozilla.org/pt-BR/docs/Web/CSS/CSS_Grid_Layout/Basic_Concepts_of_Grid_Layout)). No caso da imagem acima, as colunas têm a medida de `5fr` e as linhas de `3fr`. E sabe o melhor do `fr`? Ele se adequa ao tamanho da sua tela. 

Além disso, cada caixa pintada da imagem é uma área do grid onde ficará um elemento HTML. Mas calma que eu vou explicar o porquê isso é importante.

# Colocando em prática

Só pra você saber, os exemplos nesse post vão seguir uma estrutura onde mostrarei sempre um código css e uma aplicação desse código num codepen. Além disso, existirá um fluxo onde cada código será usado como base para os códigos subsequentes.

## Preparando o terreno

O primeiro passo para utilizar o grid é definir quais são os elementos que estamos utilizando e o que cada um contém. Isso é definido no HTML. Nesse caso, temos uma caixa grande que tem seis caixas pequenas dentro dela:

```html
<div class="caixa grande">
  <div class="caixa pequena-1">Caixa pequena 1</div>
  <div class="caixa pequena-2">Caixa pequena 2</div>
  <div class="caixa pequena-3">Caixa pequena 3</div>
  <div class="caixa pequena-4">Caixa pequena 4</div>
  <div class="caixa pequena-5">Caixa pequena 5</div>
  <div class="caixa pequena-6">Caixa pequena 6</div>
</div>
```

Para facilitar o entendimento, pode-se usar tags HTML personalizadas. Segue o exemplo de como o código acima poderia ficar:

```html
<caixa-grande>
  <caixa-pequena1/>
  <caixa-pequena2/>
  <caixa-pequena3/>
  <caixa-pequena4/>
  <caixa-pequena5/>
  <caixa-pequena6/>
</caixa-grande>
```

Para começar a utilização do grid, você precisa apenas definir a propriedade `display: grid` no elemento que desejar, no caso iremos utilizar essa propriedade na `caixa-grande` já que queremos organizar o que tem dentro dela: 

```css
caixa-grande {
  display: grid;
}

```

{% codepen https://codepen.io/pedroespindula/pen/mNQYGQ %}

Facil, né? Mas calma, ainda não temos nada pronto. O que foi definido nesse CSS foi apenas o tipo de organização (`display`) que a caixa utilizará, que é o grid. Nesse caso, estamos dizendo ao elemento apenas isso: "Use o grid pra organizar o que tem dentro de você. Não me importa como você vai fazer isso, apenas use o grid.".

# Entendendo como organizar

## Organização básica

Você deve ter notado que a malha foi definida de forma automática onde cada caixa menor foi colocada em uma linha da malha, ou seja, um abaixo do outro. Implicitamente, ao definir o `display: grid` você define algumas outras propriedades padrões do grid ao elemento. A propriedade responsável por organizar o grid desse modo é a [grid-auto-flow](https://developer.mozilla.org/pt-BR/docs/Web/CSS/grid-auto-flow) que é definida de forma padrão como `grid-auto-flow: row`. Ou seja, o fluxo automático de organização do grid está definido como linha. Assim, a cada novo filho adicionado, ele vai ser posto numa nova linha.

Agora imagine que queiramos organizar os elementos em colunas ao invés de linhas, ou seja, um ao lado do outro ao invés de um abaixo do outro. Basta modificar essa propriedade da seguinte forma:

```css
caixa-grande {
  display: grid;
  grid-auto-flow: column;
}
```
{% codepen https://codepen.io/pedroespindula/pen/qeLPzx %}

Nesse exemplo estamos dizendo literalmente ao elemento "Use o grid mas organize essas caixas em colunas".

## Organização com grid-template

No exemplo anterior deixamos o trabalho da definição da malha todo para o grid de forma automatica. Mas e se quisermos definir uma malha personalizada? Para isso, precisamos definir o `grid-template` no elemento HTML de sua escolha. Nessa malha você vai definir três coisas: as colunas (columns), as linhas (rows) e as áreas (areas) para colocar o conteúdo da caixa que você escolher.

Lembra da definição de linhas, colunas e áreas que eu dei la em cima? Pronto, você vai fazer seguir esses conceitos para definir sua malha.

## Codificando

Vamos para um exemplo concreto. Imagine que você não queira utilizar o `grid-auto-flow` mas queira todas as caixas uma do lado da outra numa unica linha. Já que temos seis caixas pequenas, temos que definir seis colunas e uma linha, desse modo:

```css
caixa-grande {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr 1fr 1fr;
  grid-template-rows: 1fr;
}
```

{% codepen https://codepen.io/pedroespindula/pen/MNLZEV %}

Já que temos apenas uma linha, não precisamos defini-la explicitamente por isso, para facilitar, a definição pode ser feita desse modo:

```css
caixa-grande {
  display: grid;
  grid-template-columns: repeat(6, 1fr);
}
```

{% codepen https://codepen.io/pedroespindula/pen/VogNKJ %}

Além da omissão do `grid-template-rows` utilizamos a função `repeat`. O `repeat` é utilizado para repetir várias vezes a quantidade de medida passada. Nesse caso, repetimos seis vezes a medida de `1fr`, já que todas as colunas teriam o mesmo tamanho de 1fr. O repeat é uma função muito poderosa e será abordada de forma mais aprofundada em outro post.


Agora se quisermos uma malha 3x2? Continua muito simples:

```css
caixa-grande {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: repeat(2, 1fr);
}
```

{% codepen https://codepen.io/pedroespindula/pen/YzKwNBj %}

Em todos esses casos, tanto as colunas como as linhas estão com tamanhos iguais, mas podemos definir com tamanhos diferentes também. Por exemplo:

```css
caixa-grande {
  display: grid;
  grid-template-columns: 2fr repeat(2, 1fr);
  grid-template-rows: repeat(2, 1fr);
}
```

{% codepen https://codepen.io/pedroespindula/pen/OJLMWee %}

Agora a primeira coluna tem o dobro do tamanho das outras colunas. Simples, né?

## Usando áreas no grid

Em nenhum desses exemplos utilizamos o `grid-template-areas`, no entanto, todos funcionaram bem. Funcionam justamente por causa da dedução do CSS Grid que mencionei anteriormente, pois ele atribui sequencialmente as propriedades da malha a cada filho.

No entanto, caso queiramos alterar a ordem das caixas, precisamos alterar o HTML. Num exemplo simples como esse, basta trocar a ordem das linhas das caixas e cada conteúdo será alterado. No entanto, em projetos complexos, se torna inviável alterar o HTML. Para a nossa sorte existe o `grid-template-areas`.

Nele você define, literalmente, áreas que a malha terá. Como exemplo:


```css
caixa-grande {
  display: grid;
  grid-template-columns: 2fr repeat(2, 1fr);
  grid-template-rows: repeat(2, 1fr);
  grid-template-areas: 
    "c-pequena1 c-pequena2 c-pequena3"
    "c-pequena4 c-pequena5 c-pequena6";
}
```

O container pai está pronto, mas precisamos ainda definir dentro dos filhos a qual área cada um está atribuído, o que é feito desse modo:

```css
caixa-pequena1 {
  grid-area: c-pequena1;
}

caixa-pequena2 {
  grid-area: c-pequena2;
}

caixa-pequena3 {
  grid-area: c-pequena3;
}

caixa-pequena4 {
  grid-area: c-pequena4;
}

caixa-pequena5 {
  grid-area: c-pequena5;
}

caixa-pequena6 {
  grid-area: c-pequena6;
}
```

{% codepen https://codepen.io/pedroespindula/pen/VwZePoG %}

Tudo pronto! Essa definição ficou igual ao exemplo anterior onde não tinhamos o `grid-template-areas`, mas imagine que queiramos trocar a posição da `caixa-pequena2` pela `caixa-pequena4`. Você precisa apenas alterar a ordem no `grid-template-areas`, modificando assim apenas o CSS sem mexer no HTML, desse modo:

Só uma observação, a definição da propriedade do `grid-area` é sem aspas, se você colocar a área envolta de aspas ele não vai funcionar.

```css
caixa-grande {
  display: grid;
  grid-template-columns: 2fr repeat(2, 1fr);
  grid-template-rows: repeat(2, 1fr);
  grid-template-areas: 
    "c-pequena1 c-pequena4 c-pequena3"
    "c-pequena2 c-pequena5 c-pequena6";
}
```

{% codepen https://codepen.io/pedroespindula/pen/YzKwZzR %}

As áreas do grid não possibilitam somente a mudança de forma facil de elementos HTML de posição. A forma mais poderosa que ele pode ser usada é para a definição de malhas complexas. Uma coisa que não tinhamos feito antes é usar mais de uma vez uma área na definição do `grid-template-areas`, mas esse é o motivo principal de usarmos o grid.

Para você entender melhor, imagine que temos agora que fazer uma pagina principal que tenha uma barra lateral, um conteúdo, um cabeçalho e um rodapé. O cabeçalho deve ficar sempre em cima, o rodapé sempre em baixo, a barra lateral no lado esquerdo e o conteúdo principal no lado direito. O conteúdo principal é três vezes maior que a barra lateral em largura e quatro vezes maior que o cabeçalho e o rodapé em altura. 

Parece complexo né? É não, isso fica fácil de organizar com grid e com grid-areas. Primeiro definimos todos os elementos:

```html
<pagina-principal>
  <cabecalho/>
  <barra-lateral/>
  <conteudo-principal/>
  <rodape/>
</pagina-principal>
```

Depois o grid:

```css
pagina-principal {
  display: grid;
}
```

Eu normalmente defino o `grid-template-areas` primeiro pois assim tenho uma visualização melhor do que estou tentando fazer:
```css
pagina-principal {
  display: grid;
  grid-template-areas: 
    "cabecalho cabecalho"
    "barra-lateral conteudo-principal"
    "rodape rodape";
}
```

Depois podemos atribuir cada área a um elemento filho:

```css
cabecalho {
  grid-area: cabecalho;
}

barra-lateral {
  grid-area: barra-lateral;
}

conteudo-principal {
  grid-area: conteudo-principal;
}

rodape {
  grid-area: rodape;
}

```

Mas ainda falta definir os tamanhos que foram pedidos, pra isso usamos a definição das linhas e das colunas. Como o conteúdo principal vai ser três vezes maior que a barra lateral, teremos uma coluna três vezes maior que a outra, e como o conteúdo principal vai ser quatro vezes maior que o cabeçalho e o rodapé, a linha do conteúdo principal vai ter que ser quatro vezes maior que as linhas do cabeçalho e do rodapé:

```css
pagina-principal {
  display: grid;
  grid-template-columns: 1fr 3fr;
  grid-template-rows: 1fr 4fr 1fr;
  grid-template-areas: 
    "cabecalho cabecalho"
    "barra-lateral conteudo-principal"
    "rodape rodape";
}
```

{% codepen https://codepen.io/pedroespindula/pen/gOYPmrN %}

Pronto, temos a nossa pagina principal feita!

# Hora de experimentar

Agora que você já sabe um pouco de CSS Grid, brinque um pouco com essa propriedade e teste coisas que eu não expliquei nesse miniguia. Tá sem ideia? Então bora lá: Se eu tiver uma área não utilizada no grid, o que acontece? E se eu quiser colocar uma área vazia entre cada coluna e cada linha? E se eu não souber a quantidade de elementos filhos, como eu faço? E se eu não quiser nenhum espaço vazio? Tudo isso é possivel com o Grid, garanto a você.

# É isto!

Meu nome é Pedro e muito obrigado pela leitura! Sou contribuidor do OpenDevUFCG e desenvolvedor front-end. Se desejar entrar em contato comigo, é só me mandar uma mensagem no [Twitter](https://twitter.com/pedro_espindula) e se quiser ver um pouco do meu trabalho, dá uma olhada no meu [GitHub](https://github.com/pedro_espindula).

Fique atento: em breve, teremos novos artigos de contribuidores do OpenDevUFCG aqui no **dev.to** (Inclusive meus). Acompanhe o OpenDevUFCG no [Twitter](https://twitter.com/OpenDevUFCG), no [Instagram](https://instagram.com/OpenDevUFCG) e, claro, no [GitHub](https://github.com/OpenDevUFCG).
 
