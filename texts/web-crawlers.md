---
title: Crawlers - Me diga aonde você vai que eu vou varrendo
published: false
description: Uso de webcrawler para extração de metadados de músicas
tags: crawler, music, ptbr, scrapy
---

Você já quis obter dados de um serviço que não disponibiliza uma API? ou se você faz computação na UFCG, provavelmente já escutou alguém falando do bot de matrícula e se perguntou como funciona? Pois bem, no post de hoje você aprenderá como é possível fazer essas coisinhas e poderá usar sua criatividade para brincar e explorar ainda mais.

## Mr robot

![](https://media.giphy.com/media/l0IyiADcZ3Ecjrn5m/giphy.gif)

Um **web crawler** pode ser definido como um script ou programa que é utilizado para acessar um website, extrair conteúdo e descobrir páginas relacionadas ao mesmo, repetindo o processo até que não existam mais páginas a serem pesquisadas.
Eles são muito importantes pois as *engines* de busca, usam os crawlers para retornar resultados eficientes, devido a isso, frequentemente algumas pessoas se referem a ele como **bot da internet**. De uma forma superficial, você pode pensar no crawler como um carinha que faz requisições para as páginas que ele encontra e a medida que ele percorre essas páginas faz o *parse* do seu html.

## Pré-Requisitos

Para conseguir reproduzir o que faremos nesse tutorial, você precisará de um ambiente com o Python 3 configurado, uma alternativa é usar o [virtualenv](https://www.digitalocean.com/community/tutorial_series/how-to-install-and-set-up-a-local-programming-environment-for-python-3), que possibilita a criação de um ambiente de desenvolvimento isolado.

Além do Python3, faremos uso do [**Scrapy**](https://scrapy.org/), um framework que possui as ferramentas necessárias para **extrair** dados de websites, **processar** os que você queira e **armazená-los** na estrutura de seu interesse. Apesar de ser possível construir um crawler usando módulos fornecidos pelo python, a medida que o seu projeto cresce, pode se tornar complicado gerenciar todos os processos da varredura de páginas da web, por isso faremos uso dele, mas caso tenha interesse em conhecer outras alternativas, deixarei links nas referências.

Para instalá-lo, utilize o índice de pacotes do Python(`PyPI`), através do seguinte comando:
`pip install Scrapy`

Feito isso, podemos dar ínicio a nossa implementação.

## Construindo o webcrawler

Se você usa o spotify, já viu que vez ou outra em algumas músicas, são exibidas as letras e algumas informações interessantes relacionadas a mesma, que são extraídas do **Genius**, motivada em obter essas informações de uma banda que eu conheci recentemente chamada **Parcels**(inclusive, fica aqui a sugestão, se você gosta de *Daft Punk*, provavelmente vai curtir eles), decidi construir esse crawler.

![](https://spotifysupport.freetls.fastly.net/article-gallery/articles2/android/android_behind_the_lyrics.png)

A primeira coisa que um crawler precisa é de um ponto inicial, também chamado de **seed**, para que ele comece a extrair os conteúdos. No nosso caso, esse ponto será a página da banda no genius, essa [aqui](https://genius.com/artists/Parcels).

Uma vez que temos isso, podemos iniciar a construção do nosso robozinho, crie um arquivo python chamado `genius.py`, que possui o seguinte conteúdo:

```python
import scrapy

class GeniusSpider(scrapy.Spider):
    name = "genius"
    start_urls = ['https://genius.com/artists/Parcels']

```

Primeiro, importamos o **scrapy** para termos acesso as funcionalidades que esse módulo fornece. Em seguida criamos um classe chamada `GeniusSpider` que é baseada na `Spider` do Scrapy, é ela que define quais métodos estamos hábeis a usar, que poderão nos auxiliar durante a execução do crawler. Por fim, definimos o nome do spider, como **genius** e o nosso *seed* como sendo a página dos parcels.

Vamos executar e ver o que acontece, diferentemente do que fazemos com scripts python, usaremos a forma que o próprio scrapy provê, por meio da sua CLI, através do seguinte comando:

`scrapy crawl genius`

// Output

Certo, temos várias letrinhas bonitinhas e outras nem tanto, mas o que tudo isso quer dizer?

1. O scrapy carregou e configurou o que precisava
2. Requisitou a url que definimos no start_urls, e baixou o seu contéudo
3. Obteve o conteúdo extraído e repassou para um método parse, que como não tinhamos criado ele, nada aconteceu e e ele finalizou a execução.

## Entendendo a estrutura das páginas do genius

O próximo passo é definir **como** o scrapy deve extrair o conteúdo, fazemos isso definindo o método **parse**. Nesse passo, é importante você entender como estão organizados os elementos da sua página, pois será essencial para transmitir para o crawler, que locais ele deve **raspar** quando extrair a página.

![](https://i.imgur.com/YJ9G6sg.png)

Analisando a imagem, você pode ver que a sua direita existe uma listagem de cards com as músicas populares, que ao serem clicados nos levam ao que queremos. Então esse deve ser o local que o nosso crawler irá extrair os links.

O scrapy extrai o conteúdo, baseado em *seletores*, os *seletores* são "padrões" ou "modelos" que casam com os elementos de uma árvore do documento e portanto podem ser usados para selecionar os nós de um documento HTML. Para conseguir fazer isso, o scrapy fornece duas formas, através do Xpath e através de seletores CSS, usaremos os seletores CSS, por simplicidade.

Usando o inspetor do meu browser para analisar quais os nós que contém esse link, e então formar o nosso seletor, você deverá visualizar que o elemento que contém essa lista de cards é uma `<div class="mini_card_grid-song"><a href="...">...</div>`.

//gif

Sendo assim, podemos informar para o **scrapy** que ele deve obter algo como:
`div.mini_clard_grid-song a::attr(href)`
Isso indica que queremos os links, que são filhos da classe, por isso o `.` como em CSS, `mini_card_grid-song`. Assim, podemos construir nosso método:

```python
 def parse(self, response):
        songs_urls = response.css('div.mini_card_grid-song a::attr(href)').getall()
        for song_url in songs_urls:
            yield scrapy.Request(url=song_url, callback=self.parse_lyrics_page)
```

O método é composto por um parâmetro **response**, que indica o conteúdo obtido após o crawler ter requisitado nossa url inicial. Feito isso, temos as urls das músicas sendo obtidas a partir do metodo do nosso seletor, é importante ressaltar que usamos o `getAll()`, porque queremos extrair **todos** os seletores que casarem com o padrão que passamos, mas as vezes estamos interessados apenas na primeira ocorrencia e podemos fazer uso do `get`. Uma vez que temos as urls, fazemos uma requisição, passando a url e uma **callback**, que é uma função que será executada após o crawler fazer download da url que passamos.

Ótimo, conseguimos alcançar a página das nossas músicas, mas e depois que chegamos nela o que faremos? Seguimos um processo muito parecido com a diferença de que agora, ao invés de tentarmos extrair links, poderemos extrair nossa informação, como visto o método que será executado após ele consultar a página da música é o `parse_lyrics_page`, então adicione esse trecho no seu arquivo:

```python

    def parse_lyrics_page(self, response):
        title = response.css('div.song_body-lyrics h2::text').get()
        song_metadata = response.css('div.rich_text_formatting p::text').getall()
        artists = set(response.css('span.metadata_unit-info a::text').getall())
        lyric = response.css('div.lyrics p::text').getall()
        annotations_ids = response.css('a::attr(annotation-fragment)').getall()

        item = {
            'title': title,
            'artists': artists,
            'lyric': lyric,
            'metadata': song_metadata,
            'snippet_lyric': [],
            'annotations': []
        }

        if annotations_ids:
            return self.get_annotations(response, annotations_ids, item)
        else:
            return item

    def get_annotations(self, response, annotations_ids, item):
        for annotation_id in annotations_ids:
            url = urljoin(response.url, annotation_id)
            yield scrapy.Request(
                url=url,
                callback=self.parse_annotation_page,
                meta={'item': item}
)
```

Muita coisa, né? mas vamos por partes, como diria Jack.

Para extração das informações básicas como os artistas, a letra o título, as curiosidades(especificadas por meio do atributo metadata), não é muito diferente do que fizemos na extração dos links, já que só precisamos fornecer o seletor e obter a informação, só temos a diferença de que adicionamos o `::text`, isso acontece, porque se não utilizarmos ele, extraíriamos a informação com as tags html, não só o conteúdo que ele envolve, então removemos esse ruído, através dele.

Você deve está se perguntando, por que as anotações tem um comportamento diferente, olhando a página, você verá que os nós que deveriam conter as anotações para as músicas, contém apenas um identificador, que redirecionam pra outra página, que possue o conteúdo delas. Então, o que estamos fazendo é, primeiro, obtendo esses ids, e para cada id obtido, concatenando a nossa url inicial:
`urljoin(response.url, annotation_id)`

E em seguida, fazemos uma requisição para cada um deles, mas espere, essa requisição tem algo que ainda não vimos antes, um parâmetro chamado `meta`.
O `meta` é a forma que o scrapy provê para que consigamos nos comunicar entre uma página e outra, então, o que estamos querendo dizer, é que toda vez que ele consultar as páginas das anotações, leve consigo as informações que já extraímos dessa, isso será útil, para merjamos os dois objetos e retornamos os resultados.

Agora que você está na página de anotações, encontre os seletores das anotações e retorne o \resultado, e pronto você terá feito o crawler. Como mostra o trecho abaixo:

```python
    def parse_annotation_page(self, response):
        snippet_lyric = response.css("meta[property='og:title']::attr(content)").getall()
        annotations = response.css("meta[property='og:description']::attr(content)").getall()
        item = response.meta['item']
        item['snippet_lyric'] = snippet_lyric
        item['annotations']= annotations

    return item
```


## Extraindo metadados


## Referências