---
title: Contribuindo para projetos open source com GitHub
published: false
description: Uma porta de entrada para pessoas que planejam começar a fazer contribuições para organizações open source.
tags: opensource, comunidade, github, ptbr.
cover_image: https://thepracticaldev.s3.amazonaws.com/i/16tq00lwa6aq3wak8jsf.jpg
---

Criado em 2008 e muito aperfeiçoado desde então, o GitHub não apenas é, atualmente, o mais conhecido serviço web que oferece diversas funcionalidades aplicadas ao Git para hospedagem e versionamento de código-fonte, como também é um grande impulsionador da iniciativa open source e do trabalho em equipe. Cada vez mais, a plataforma tem adquirido uma cara de rede social, visando engajar mais pessoas em relação a contribuição em projetos de código aberto, participação em suas respectivas comunidades, além de dar mais visibilidade a tais projetos. Isso foi possível devido a inúmeras funcionalidades adotadas e implementadas pelo GitHub a fim de atingir esses objetivos, e algumas delas serão introduzidas nesse post.

![Página inicial do GitHub atualmente](https://thepracticaldev.s3.amazonaws.com/i/ca2pyzgamadnsl53fbgn.png)
*Screenshot da página inicial do GitHub atualmente*.

## Comunidade open source

Antes de prosseguir no post, é preciso deixar claro um conceito para comunidade open source. Uma comunidade é formada por **Users**, **Contributors** e **Mantainers**. O grupo de usuários (*users*) é composto por pessoas que vão utilizar um sistema que está sendo desenvolvido em código aberto, e são ativas na comunidade reportando possíveis bugs no desenvolvimento e dando sugestões de melhoria e de novas *features*. Além disso, dependendo da licença utilizada pelo projeto, usuários podem fazer modificações livremente, por exemplo, alterar partes do sistema para uso pessoal, utilização como componente em outro projeto e até comercializá-lo. Para saber o que pode e o que não pode fazer, é sempre bom checar o arquivo **LICENSE** do repositório e verificar as permissões.

O que diferencia um usuário de um contribuidor (*contributor*) é a efetividade para a organização que encabeça o projeto. O contribuidor levanta discussões mais profundas e úteis para a organização, altera diretamente o código e faz requisições de anexação do seu conteúdo ao original. Os motivos que levam alguém a contribuir com uma organização podem ser diversos: o desejo de melhorar aplicações que ele mesmo utiliza, identificação com a organização, aplicação prática de conhecimentos acerca de uma tecnologia, ganhar visibilidade com contribuições úteis e reais, entre outros. Esse é o espírito do open source.

Vale ressaltar dois pontos: primeiro, o contribuidor não está vinculado à organização. Isso significa que alguém pode aparecer repentinamente para contribuir e, da mesma forma, pode parar a qualquer momento. Segundo, é muito comum que a organização estabeleça regras para contribuição e é importantíssimo que o contribuidor esteja por dentro de todas elas antes de começar. Por isso, ele deve ler bem o que há nos arquivos **README.md**, **CONTRIBUTING.md** e **CODE_OF_CONDUCT.md**, caso existam.

Por fim, temos os mantenedores (*maintainers*). Estes são os administradores responsáveis por manter o projeto "vivo" e atualizado, pois eles são membros **efetivos** da organização, e têm privilégios de administrador no repositório do GitHub. Eles devem estar por dentro das discussões que envolvem o projeto, fazer correções de bugs, implementar features, revisar requisições de contribuidores e são os responsáveis por manter o projeto organizado: disposição dos arquivos e diretórios no repositório, alocação de *tasks*, categorização dos tópicos das discussões, entre outras obrigações.

Agora, com o conceito de comunidades open source mais claro, podemos começar a falar sobre as maneiras que o GitHub encontrou para impulsioná-las e como você pode utilizar a plataforma para fazer parte de uma.

## Stars e forks
![Stars e forks](https://thepracticaldev.s3.amazonaws.com/i/pu4yui075180v6w6k318.png)

Ao acessar um repositório público no GitHub, você pode dar uma **star**, *feature* similar às "curtidas" das redes sociais mais famosas, e também pode fazer um **fork**, que consiste na criação de uma cópia independente deste repositório, que contém todo o seu conteúdo, para a sua conta de usuário no GitHub. Na tela inicial, são mostradas as **stars** e **forks** que as pessoas que você segue dão em repositórios públicos, uma funcionalidade que tem por objetivo a ampliação do alcance dos projetos open source.

## Pull Requests (PR's) e Issues

A partir de um **fork**, quando terminar de fazer as alterações que julgar necessárias, você pode submeter uma requisição de anexação do seu conteúdo modificado ao conteúdo original do repositório escolhido. Essa requisição é a **pull request**, a forma mais característica de contribuição a um projeto na plataforma, que será revisada por um maintainer.

Também é importante ter em mente que uma contribuição para um projeto open source vai além de modificações diretas no código-fonte. Um feedback, alerta de bugs no software que está sendo desenvolvido, discussões a respeito de um novo passo que pode ser tomado em prol do projeto, troca de ideias e sugestões podem impactar profundamente no futuro do projeto e até da organização. Por isso, é extremamente importante que uma organização fique sempre atenta ao que sua comunidade tem a dizer.

Tendo em vista esse contexto, o GitHub disponibiliza as **issues**, um espaço que pode ser utilizado tanto por usuários comuns quanto por maintainers, para a finalidade de todos os pontos citados e também para priorização e organização das tarefas a serem desenvolvidas. Observe abaixo o processo para abrir uma nova issue:

![Submetendo uma nova issue](https://thepracticaldev.s3.amazonaws.com/i/c7hx2r6s9rhuauhmbsx7.png)

## Descobrindo repositórios e contribuindo

É muito comum que não se saiba muito bem por onde começar a contribuir no início da "aventura" pelo mundo open source. Para minimizar essa dificuldade, você pode visualizar a sessão **[discover repositories](https://github.com/discover)**, que traz recomendações muito interessantes de projetos, baseadas em outros repositórios que você deu star, em pessoas que você segue, em tecnologias recentes que você trabalhou e também em popularidade do projeto, os **trendings**.

Depois de localizar um projeto com o qual se identificou, você pode explorar o arquivo **README.md** a fim de obter informações a respeito do que se trata o projeto, com quais tecnologias ele trabalha, como rodar na sua máquina, link da página principal, entre outras. É importante ter em mente que a disponibilidade dessas informações nesse arquivo depende bastante da organização que criou o projeto, e como ela organiza suas atividades.

Depois de se informar sobre esse projeto e decidir que quer contribuir para ele, um ótimo ponto de partida é olhar as issues abertas e verificar as suas **labels**, que consistem em uma forma de categorização e priorização de issues e PR's para ajudar na organização do trabalho. A fim de te deixar mais confortável para começar a contribuir, é interessante que a procura inicial seja por labels de **good first issue**, **easy** e **help wanted**, por exemplo. Vale ressaltar também que essa categorização de issues e PR's por labels também depende de como a organização "se organiza".

![](https://thepracticaldev.s3.amazonaws.com/i/pjm9maf3lt611kbhjlzr.png)
*Screenshot da página de issues do [Tamburetei](https://github.com/OpenDevUFCG/Tamburetei) com suas labels.*

Você pode contribuir com novas ideias nas discussões a respeito de alguma das issues ou fazer uma PR que a resolva. Posteriormente, quando estiver mais à vontade na organização, você pode abrir novas issues ou tentar resolver algumas mais complexas.

## Explorando o GitHub

Outra forma de ter acesso a novidades de projetos open source é acessando o **[Explore](https://github.com/explore)**, uma página do GitHub que além de mostrar projetos recomendados, como mostrado no tópico anterior, também possibilita uma conexão entre as comunidades desses projetos. Você pode explorar conteúdos de curadores, buscar projetos por tópicos e labels, ter acesso a informações de posts, artigos, eventos e meetups relacionados com as tecnologias recomendadas para você, entre outras informações.

![](https://thepracticaldev.s3.amazonaws.com/i/ngl2p0mpmwzd6x8dzziq.png)

Essas são algumas das principais maneiras que o GitHub encontrou para dar visibilidade a projetos de código aberto e fomentar a colaboração dos desenvolvedores para estes. Você pode se aprofundar mais a respeito do funcionamento do Git e do GitHub e obter informações mais técnicas [neste outro post](https://medium.com/@Juliobguedes/entendendo-git-883464f379de), e começar a apoiar o open source agora mesmo!

Muito obrigado pela leitura, fique atento a mais postagens da OpenDev UFCG aqui no Dev.to! Esperamos encontrá-lo por aqui novamente. :smile:
