---
title: Usando o spotify-web-sdk para gerenciar requisições à API Web do Spotify
description: Não nos chame, nós te chamamos!
tags: react, webdev, showdev, ptbr
canonical_url: https://dev.to/calluswhatyouwant/using-spotify-web-sdk-to-handle-requests-to-the-spotify-web-api-2oj6
cover_image: https://thepracticaldev.s3.amazonaws.com/i/6a3wfu20y6db2gojyuzi.png
---

Eu e [Renan](https://dev.to/joserenan) temos uma organização open source chamada [Call Us What You Want](https://github.com/calluswhatyouwant), na qual desenvolvemos aplicações com foco em música. Um de nossos projetos é um SDK para a API Web do Spotify, que criamos para servir de apoio para outras aplicações que fazem uso desses dados.

Nessa postagem, vou mostrar como o [spotify-web-sdk](https://github.com/calluswhatyouwant/spotify-web-sdk) pode facilitar a recuperação e a gerência de dados de usuários do Spotify. Farei isso descrevendo o processo de desenvolvimento de uma aplicação inspirada por um problema *muito sério* que eu enfrentava como usuário dessa plataforma de streaming.

{% github https://github.com/calluswhatyouwant/spotify-web-sdk %}

# Um problema do mundo real (*A little motivation*)

A seção de álbuns da minha biblioteca do Spotify é bem desorganizada, mas não exatamente por culpa minha — na implementação do aplicativo, sempre que um single é adicionado às músicas, o mesmo também acaba aparecendo como álbum. Isso acaba sendo um problema na hora de localizar algo para ouvir, já que eu prefiro consumir álbuns do início ao fim no lugar de entrar no modo aleatório das playlists.

![Screenshot de álbuns no Spotify](https://thepracticaldev.s3.amazonaws.com/i/9m6nxnmakkfwp13kisr3.png)
<figcaption><b>Comfort Crowd</b> and <b>People</b> são singles, mas também aparecem na minha biblioteca como álbuns.</figcaption>

Pensei em uma solução simples: criei uma [playlist](https://open.spotify.com/user/jrobsonjr/playlist/7lWSJamQptWauCMIEDXgYC?si=HZZTKAY8RWObSGEdYm-4uA) contendo a primeira faixa de cada álbum que eu já ouvi. Ter essa playlist é muito útil para que eu também mantenha o registro do dia em que ouvi os trabalhos inteiros pela primeira vez (pelo menos quando eu lembro de adicioná-los assim que termino de escutá-los). Problema resolvido... ao menos parcialmente. Bem, para manter as coisas ainda mais organizadas, eu ainda queria poder ordenar as referências aos álbuns pelas suas datas de lançamento, algo que o Spotify ainda não dá suporte (mesmo com usuários pedindo por essa feature [há mais de cinco anos](https://community.spotify.com/t5/Live-Ideas/Your-Music-Sort-Music-in-quot-Your-Music-quot-by-Year/idi-p/744893)).

Procurei por ferramentas que pudessem gerenciar esse método de ordenação para mim e descobri uma aplicação web chamada [Sort Your Music](http://sortyourmusic.playlistmachinery.com/). Apesar de ser super simples e eficiente de usar, a abordagem de ordenação deles sobrescreve a data de adição das faixas. Sei que por agora você já deve estar revirando os olhos com a minha insatisfação, mas, sabendo que é possível conseguir o resultado que eu desejo, decidi implementar a minha própria ferramenta. Nada como um pequeno DIY, certo?

![Taylor Swift com ódio](https://thepracticaldev.s3.amazonaws.com/i/f1co9752rtmcdbm5nqfr.gif)
<figcaption>Perdoe-me caso você não enfrente esse problema no seu dia a dia.</figcaption>

# Devagar e sempre

A API Web do Spotify disponibiliza dois endpoints para gerência de playlists: [um](https://developer.spotify.com/documentation/web-api/reference/playlists/replace-playlists-tracks) que permite sobrescrever todas as faixas e [outro](https://developer.spotify.com/documentation/web-api/reference/playlists/reorder-playlists-tracks/) que pode ser usado para mover uma faixa ou um bloco de faixas. Ao contrário do primeiro, este não modifica o *timestamp* que indica quando as faixas foram adicionadas à playlist e o identificador de quem as adicionou (para playlists colaborativas).  

![Visualização da reordenação de faixas](https://thepracticaldev.s3.amazonaws.com/i/lfcnbuvlnmvemungeuyu.png)
<figcaption>Visualização de como funciona o endpoint "reordenação de faixas de uma playlist". <br /> Fonte: <a href="https://developer.spotify.com/documentation/web-api/reference/playlists/reorder-playlists-tracks/">Spotify for Developers</a></figcaption>

Como visto acima, esse endpoint funciona de forma que ordenar uma playlist requer várias requisições consecutivas (basicamente uma requisição por faixa na playlist), o que também significa que leva muito mais tempo do que simplesmente sobrescrever tudo. Sacrifícios tiveram que ser feitos, mas o resultado é exatamente o esperado: aqui está o [Spotify Playlist Editor](https://github.com/calluswhatyouwant/spotify-playlist-editor)!

![Spotify Playlist Editor](https://thepracticaldev.s3.amazonaws.com/i/7ookuks66s0203i3s9n3.png)
<figcaption>Quando você tiver feito login, algo assim deve aparecer na tela inicial da aplicação.</figcaption>

# Detalhando o processo (mas não demais)

Eu queria prototipar uma aplicação React o mais rápido possível, então usei o [create-react-app](https://facebook.github.io/create-react-app/), que me poupou muito tempo de configurações iniciais. Em geral, encorajo que você tente entender como criar uma aplicação React do zero, mas isso é com certeza é uma mão na roda quando se tem pressa. Para manter tudo simples, o **Spotify Playlist Editor** é *serverless*, mas permite que todos acessem seus dados do Spotify através de uma implementação do [fluxo de concessão implícita](https://developer.spotify.com/documentation/general/guides/authorization-guide/), uma abordagem simples para conseguir token de acesso à API.

Incluí alguns pacotes e ferramentas para simplificar o processo de codificação (por exemplo, [Bootstrap](https://getbootstrap.com) para que eu me preocupasse menos com estilos e [prettier](https://prettier.io/) para ajudar a manter o código padronizadinho). Também considero interessante mencionar que uso [Flow](https://flow.org) para checagem estática de tipos. Eu amo que JavaScript tem tipagem dinâmica, mas como o SDK lida com muitos modelos que têm, por sua vez, muitos atributos, Flow se torna um ótimo aliado.

![IntelliSense](https://thepracticaldev.s3.amazonaws.com/i/jjhcgbimvjazm6wcvcb8.png)
<figcaption>Você pode até odiar o Flow, mas o fato é que React + Flow + <a href="https://code.visualstudio.com/docs/editor/intellisense">IntelliSense</a> é um verdadeiro <i>dream team</i>.</figcaption>

## Conheça o melhor amigo ~~autoproclamado~~ da API Web do Spotify, o spotify-web-sdk!

Aqui está um trecho de código tirado do componente *UserPage*. Você pode ver que alguns imports são feitos diretamente ao SDK:

```javascript
/* @flow */

import React, { Component } from 'react';
import {
    /* Functions that wrap Spotify endpoints */
    init,
    getCurrentUserPlaylists,
    getCurrentUserProfile,
    /* Models */
    Page,
    PlaylistSimplified,
    PrivateUser,
} from 'spotify-web-sdk';

type Props = {
    history: any,
};

type State = {
    page: Page<PlaylistSimplified>,
    playlists: PlaylistSimplified[],
    user: PrivateUser,
};

class UserPage extends Component<Props, State> {
    // ...
}
```

Uma vez que o usuário esteja conectado ao Spotify, *UserPage* é a página principal da aplicação. Seu principal propósito é exibir uma lista de playlists do usuário, com um botão que permite selecionar uma playlist de interesse. Inicialmente, cinco playlists são recuperadas:

```javascript
componentDidMount = async () => {
    const page = await getCurrentUserPlaylists({ limit: 5 });
    this.setState({
        page,
        playlists: page.items,
    });
}
```

Ao manter o objeto *page* no state do componente, requisitar mais playlists é tão simples quanto poderia ser. Isso é graças à lógica de recuperar os próximos dados já estar implementada lá na declaração da classe *Page* no *spotify-web-sdk*:

```javascript
class Page<T> {
    // ...
    async getNextPage() {
        if (!this.hasNext()) throw new Error('There are no more pages');
        const params = {
            ...this.queryParams,
            limit: this.limit,
            offset: this.offset + this.limit,
        };
        const response = await this.getAxiosPageInstance().get('/', { params });
            return new Page<T>(response.data, this.t, this.wrapper);
    }
}
```

Dessa forma, lidar com toda essa lógica no editor de playlists se resume a uma simples chamada de função, dispensando a necessidade de manter-se a par de valores como limite e offset:

```javascript
loadMorePlaylists = async () => {
    const { page } = this.state;
    const nextPage = await page.getNextPage(); 
    // Relaxa e deixa o spotify-web-sdk lidar com o trabalho duro!
    this.setState(prevState => {
        const playlists = prevState.playlists.concat(nextPage.items);
        return { playlists, page: nextPage };
    });
};
```

O propósito principal da aplicação é permitir que os usuários ordenem suas playlists, então vamos focar nisso agora. Do componente *PlaylistPage*, o usuário pode selecionar um método de ordenação (incluindo por data de lançamento!). Por baixo, minha implementação determina primeiro a ordem final esperada das faixas e então faz uso de chamadas sequenciais para reordená-las. É assim que funciona mais ou menos em código:

```javascript
import { reorderPlaylistTracks } from 'spotify-web-sdk';

export const sortPlaylistTracksByAttribute = async (
    playlistId: string,
    attribute: string
) => {
    let insertionOrder = await getInsertionOrder(playlistId, attribute);
    return insertionOrder.reduce(async (previousPromise, current) => {
        await previousPromise;
        return moveTrackToTop(playlistId, current);
    }, Promise.resolve());
};

const getInsertionOrder = async (
    playlistId: string,
    attribute: string
) => { /* Determina a ordem de inserção baseada no atributo escolhido. */ };

const moveTrackToTop = (id: string, oldIndex: number) =>
    reorderPlaylistTracks(id, oldIndex, { rangeLength: 1, insertBefore: 0 });
```

A propósito, se você não está familizarizado com essa abordagem de resolver Promises sequencialmente usando *Array.prototype.reduce()*, tem um excelente artigo explicando como e, mais importantemente, a **razão** disso funcionar. Dá uma olhada lá no [CSS Tricks](https://css-tricks.com/why-using-reduce-to-sequentially-resolve-promises-works/)!

Como mostrado acima, o objetivo principal do spotify-web-sdk é poupar tempo dos desenvolvedores que desejam fazer acesso à API Web do Spotify. No lugar de fazer chamadas diretas à API e se preocupar com detalhes, a maior parte do seu trabalho deve ser importar uma função e invocá-la no código. 

Caso você tenha interesse em construir sua própria aplicação usando dados do Spotify, a versão atual do *spotify-web-sdk* está [hospedada no NPM](https://npmjs.com/package/spotify-web-sdk) e pode ser adicionada rapidamente em um projeto com package.json rodando `npm add spotify-web-sdk`.

O **Spotify Playlist Editor** também está no ar [aqui](https://calluswhatyouwant.github.io/spotify-playlist-editor/) se você quiser brincar um pouco. Caso você experiencie qualquer coisa inesperada ao usar a aplicação, é apenas um treino para estimular o espírito open source: crie uma issue no repositório do GitHub! PRs também são sempre bem-vindas (tem umas issues abertas e o Hacktoberfest está logo aí...). 

{% github https://github.com/calluswhatyouwant/spotify-playlist-editor %}

Se quiser ficar por dentro das novidades dos projetos do Call Us What You Want, acompanhe o nosso [perfil no Twitter](https://twitter.com/CallUsWhat)!

---

Muito obrigado pela leitura! Fique atento: em breve, teremos novos artigos de contribuidores do OpenDevUFCG aqui no **dev.to**. Acompanhe o OpenDevUFCG no [Twitter](https://twitter.com/OpenDevUFCG), no [Instagram](https://instagram.com/OpenDevUFCG) e, claro, no [GitHub](https://github.com/OpenDevUFCG).