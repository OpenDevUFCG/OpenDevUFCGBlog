---
title: Guia de postagens no dev.to
published: false
description: Oi, lindx. Vem sempre aqui?
tags: opensource
---
 
# Olá, queridx colega de curso! 

Obrigado por querer contribuir com o blog do OpenDevUFCG. Nós decidimos adotar a plataforma **dev.to** para hospedar o nosso conteúdo. Caso você ainda não tenha tido contato com esse ambiente, vale a pena dar uma olhada nas postagens de outras pessoas. Em geral, o dev.to é uma comunidade focada em promover a divulgação de ideias, dúvidas e conhecimento entre desenvolvedores. Dessa forma, o propósito desse portal está bem alinhado com o do OpenDevUFCG!
Se quiser visualizar esse guia no dev.to, acesse [o seguinte link](https://dev.to/opendevufcg/guia-de-postagens-no-dev-to-3c1b-temp-slug-6645055?preview=425fc609e9fa6bbd74139294f6c7220e91b6e75c9f38c54d0549d7886b8537a20d36d21bb00ccb2de68aaaa789a28f37e13a48542d6fab81ef7ddb0c).

# Primeiras orientações

Se essa é a primeira vez que você está contribuindo conosco, alguns poucos passos são necessários para que você obtenha permissão para fazer publicações.

1. Crie uma conta no [dev.to](https://dev.to);
2. Acesse **Settings** > **Organization**;
3. Em **New Organization**, insira a chave secreta que iremos te passar para que você entre na organização. Clique em **Join Organization** e agora estará tudo certo!
4. Para começar a escrever o seu texto, clique em **Write a Post** e então no campo **Publish under an organization** selecione **OpenDevUFCG**. 

# Planejamento

Teremos publicações semanais e, se você está lendo isso, provavelmente a sua está vindo em breve! Iremos determinar no planejamento a melhor data para a sua publicação de acordo com as que já estão agendadas. Quando chegar a semana da sua, o processo funciona da seguinte forma:

- até **quinta-feira** esperamos receber o seu texto para que possamos revisá-lo;
- até **sexta-feira** iremos dar um feedback em relação ao texto, sugerir ajustes (se for necessário);
- e o **sábado** é o dia da publicação e divulgação nas nossas redes sociais.

Por enquanto, **ainda** não prevemos estar fazendo publicações fora das datas planejadas. Isso não te impede de fazer sugestões ou já ir pensando em textos previamente!

# Escrevendo no dev.to

## O editor do dev.to

É muito provável que você já tenha trabalhado com ou ao menos visualizado Markdown, uma linguagem de marcação reconhecida por sua simplicidade de leitura e escrita. O README.md, por exemplo, é um arquivo presente na maioria dos repositórios de sistemas de controle de versão como o Git, explicando um pouco de como tudo funciona na comunidade relacionada a um projeto.

O editor de postagens do dev.to dispõe de várias dicas de Markdown que podem ser acessadas clicando no botão **?**. Tudo deve ser simples de entender, mas se houver dúvida para inserir alguma coisa, não hesitem em nos procurar!

O editor do dev.to também tem suporte a várias liquid tags, que facilitam bastante a adição de conteúdo incorporado na publicação. Dê uma olhada no [link para esse guia no dev.to](https://dev.to/opendevufcg/guia-de-postagens-no-dev-to-3c1b-temp-slug-6645055?preview=425fc609e9fa6bbd74139294f6c7220e91b6e75c9f38c54d0549d7886b8537a20d36d21bb00ccb2de68aaaa789a28f37e13a48542d6fab81ef7ddb0c) para ver como `{% github OpenDevUFCG/OpenDevUFCGBlog %}` é renderizado.

Liquid tags semelhantes podem ser usadas para incorporar mídia de diversas outras plataformas, como YouTube, Twitter e Spotify. Não deixe de consultar a lista e como utilizá-las (também no botão **?** do editor de postagens).

## Variáveis e valores

No topo de uma postagem do dev.to existe um campo dedicado à determinação dos valores atribuídos a algumas variáveis do artigo a ser publicado. A seguir, uma breve descrição de como preencher esses valores:

**title:** Título do seu artigo (por exemplo, "Uma breve introdução a Redux").
**published:** Valor booleano que determina se o texto estará como publicado (ou seja, visível a todos). É bem útil para que você só modifique o valor para "true" quando for hora de realmente publicar a postagem. Até lá, o artigo fica como rascunho e pode ser ajustado à vontade.
**description:** Descrição da sua postagem. É como uma expansão do título; explique como o seu texto leva ao objetivo apresentado pelo título ou faça algum comentário que chame a atenção do leitor.
**tags:** Sempre coloque a tag "ptbr". As demais devem ser decididas de acordo com o seu conteúdo; dê uma olhada em tags populares para determinar quais usar (o máximo é 4). Como o dev.to ainda é predominantemente em inglês, sempre use as tags em inglês. Um exemplo: "react, redux, javascript, ptbr". 
**canonical_url (opcional):** Se o seu texto foi publicado originalmente em algum outro blog pessoal, você pode colocar a URL do conteúdo original aqui.
**cover_image (opcional):** Uma capa para a publicação. Em geral, não é necessário colocar uma capa na sua postagem, mas se você tiver uma imagem interessante e relacionada, sempre use algo de tamanho 1000 x 420.
**series (opcional):** Sua publicação vai ser parte de uma série? Se sim, basta usar sempre o mesmo texto nesse campo que será adicionado um navegador às demais postagens da série ([um exemplo de como fica na prática aqui](https://dev.to/azure/learning-python-from-scratch-getting-started-windows-40e9)).

# Hora de produzir!

Algumas últimas orientações: quando for escrever sua postagem, tente manter o tempo de leitura (que pode ser visto clicando no botão "Preview") em uma faixa de 5 a 10 minutos. Queremos ter leituras leves e que introduzam e apeteçam a curiosidade dos leitores quanto aos conteúdos apresentados, então não há necessidade de ir muito a fundo e se estender demais. Teremos espaço para séries de postagens no futuro, o que vai permitir dividir o que seria uma postagem muito longa em um conjunto de postagens que façam sentido por conta própria.

## Terminando a sua postagem

Para que ninguém tenha que se preocupar com fazer o encerramento da postagem (e para não perder a oportunidade de divulgar as redes sociais do OpenDev), encerre a postagem com o seguinte:

```
Muito obrigado pela leitura! Fique atento: em breve, teremos novos artigos de contribuidores do OpenDevUFCG aqui no **dev.to**. Acompanhe o OpenDevUFCG no [Twitter](https://twitter.com/OpenDevUFCG), no [Instagram](https://instagram.com/OpenDevUFCG) e, claro, no [GitHub](https://github.com/OpenDevUFCG).
```

## <3

Se houver quaisquer dúvidas ao longo do processo, também estamos sempre aqui para ajudar. <3

Agradecemos pela sua disponibilidade e desejamos uma ótima experiência compartilhando conhecimento com os demais colegas de curso!
