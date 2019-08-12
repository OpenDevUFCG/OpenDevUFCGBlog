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

A malha é definida por linhas e colunas que são medidas em `fr`. A medida `fr` foi criada exclusivamente para o CSS Grid e pode ser definida como "uma fração do espaço disponível no container do grid" (fonte: [MDN](https://developer.mozilla.org/pt-BR/docs/Web/CSS/CSS_Grid_Layout/Basic_Concepts_of_Grid_Layout)). No caso da imagem acima, as colunas têm a medida de `5fr` e as linhas de `3fr`. E sabe o melhor do `fr`? Ele se adequa ao tamanho da sua tela. 

Além disso, cada caixa pintada da imagem é uma área do grid onde ficará um elemento HTML.

# Colocando em prática

## Preparando o terreno

O primeiro passo para utilizar o grid é definir quais são os elementos que estamos utilizando e o que cada um contém. Isso é definido no HTML e, para facilitar o entendimento, usarei tags HTML personalizadas. Segue o exemplo:

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

Nesse caso, temos uma caixa grande que tem seis caixas pequenas dentro dela. Para começar a utilização do grid, você precisa apenas definir a propriedade `display: grid` no elemento que desejar: 

```css
caixa-grande {
  display: grid;
}

```

Facil, né? Mas calma, ainda não temos nada pronto. O que foi definido nesse CSS foi apenas o tipo de organização (`display`) que a caixa utilizará, que é o grid. Agora precisamos organizá-la de fato.

# Entendendo como organizar


Para isso, você precisa definir o grid — ou seja, a malha — no elemento HTML de sua escolha. Nessa malha você vai definir três coisas: as colunas, as linhas e as áreas para colocar o conteúdo da caixa que você escolher.

Essa organização em código se dá pela definição do `grid-template`. Essa propriedade pode ser dividida em outras três para facilitar a sua declaração. São elas:

1. `grid-template-columns`: Define as colunas;
2. `grid-template-rows`: Define as linhas;
3. `grid-template-areas`: Define as áreas.

Você não precisa definir todas essas propriedades ao mesmo tempo; o grid é inteligente o bastante para deduzir algumas coisas.

## Codificando

Vamos para um exemplo concreto. Imagine que você queira todas as caixas uma do lado da outra na horizontal. Muito simples:

```css
caixa-grande {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr 1fr 1fr;
}
```

Ou, para facilitar:

```css
caixa-grande {
  display: grid;
  grid-template-columns: repeat(6, 1fr);
}
```

O `repeat` é utilizado para repetir várias vezes a quantidade de medida passada. Nesse caso, repetimos seis vezes a medida de `1fr`. O repeat é uma função muito poderosa e será abordada em outro post.


E se quisermos um abaixo do outro? Simples também:

```css
caixa-grande {
  display: grid;
  grid-template-rows: repeat(6, 1fr);
}
```

Agora se quisermos uma malha 3x2? Continua muito simples:

```css
caixa-grande {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: repeat(2, 1fr);
}
```

Em todos esses casos, tanto as colunas como as linhas estão com tamanhos iguais, mas podemos definir com tamanhos diferentes também. Por exemplo:

```css
caixa-grande {
  display: grid;
  grid-template-columns: 2fr repeat(2, 1fr);
  grid-template-rows: repeat(2, 1fr);
}
```

Agora a primeira coluna tem o dobro do tamanho das outras colunas. Simples, né?

## Usando áreas no grid

Em nenhum desses exemplos utilizamos o `grid-template-areas`, no entanto, todos funcionaram bem. Funcionam justamente por causa da dedução do CSS Grid que mencionei anteriormente, pois ele atribui sequencialmente as propriedades da malha a cada filho.

No entanto, caso queiramos alterar a ordem das caixas, precisamos alterar o HTML. Num exemplo simples como esse, basta trocar a ordem das linhas das caixas e cada conteúdo será alterado. No entanto, em projetos complexos, se torna inviável alterar o HTML. Para a nossa sorte existe o `grid-template-areas`. Nele você define, literalmente, áreas que a malha terá. Como exemplo:


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
  grid-area: "c-pequena1";
}

caixa-pequena2 {
  grid-area: "c-pequena2";
}

caixa-pequena3 {
  grid-area: "c-pequena3";
}

caixa-pequena4 {
  grid-area: "c-pequena4";
}

caixa-pequena5 {
  grid-area: "c-pequena5";
}

caixa-pequena6 {
  grid-area: "c-pequena6";
}
```

Tudo pronto! Essa definição ficou igual ao exemplo anterior onde não tinhamos o `grid-template-areas`, mas imagine que queiramos trocar a posição da `caixa-pequena2` pela `caixa-pequena4`. Você precisa apenas alterar a ordem no `grid-template-areas`, modificando assim apenas o CSS sem mexer no HTML, desse modo:

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

# Hora de experimentar

Agora que você já sabe um pouco de CSS Grid, brinque um pouco com essa propriedade e teste coisas que eu não expliquei nesse miniguia. Tá sem ideia? Então bora lá: Se eu tiver uma área não utilizada no grid, o que acontece? E se eu quiser colocar uma área vazia entre cada coluna e cada linha? E se eu não souber a quantidade de elementos filhos, como eu faço? Tudo isso é possivel com o Grid, garanto a você.

# É isto!

Meu nome é Pedro e muito obrigado pela leitura! Sou contribuidor do OpenDevUFCG e desenvolvedor front-end. Se desejar entrar em contato comigo, é só me mandar uma mensagem no [Twitter](https://twitter.com/pedro_espindula) e se quiser ver um pouco do meu trabalho, dá uma olhada no meu [GitHub](https://github.com/pedro_espindula).

Fique atento: em breve, teremos novos artigos de contribuidores do OpenDevUFCG aqui no **dev.to** (Inclusive meus). Acompanhe o OpenDevUFCG no [Twitter](https://twitter.com/OpenDevUFCG), no [Instagram](https://instagram.com/OpenDevUFCG) e, claro, no [GitHub](https://github.com/OpenDevUFCG).
 
