---
title: Crawlers - Me diga aonde você vai que eu vou varrendo
published: false
description: Uso de webcrawler para extração de metadados de músicas
tags: crawler, music, ptbr, scrapy
---

Você já quis obter informações de forma programática/automática para um uso posterior? Por exemplo, informações de produto de uma loja, de um placar esportivo, de um serviço que não tem os dados disponibilizados via API e se você faz computação na UFCG, provavelmente já escutou alguém falando do bot de matrícula e se perguntou como funciona? Pois bem, no post de hoje você aprenderá como fazer essas coisinhas e poderá usar sua criatividade para brincar e explorar ainda mais.

## Mr robot

![](https://media.giphy.com/media/l0IyiADcZ3Ecjrn5m/giphy.gif)

Um **web crawler** pode ser definido como um script ou programa que é utilizado para acessar um website, extrair conteúdo e descobrir páginas relacionadas ao mesmo, repetindo o processo até que não existam mais páginas a serem pesquisadas. De uma forma bem superficial, você pode pensar no crawler como um carinha que faz requisições para as páginas que ele encontra e a medida que ele percorre essas páginas faz o *parse* do seu html.
