---
title: Administrando Sistemas com Docker
published: false
description: Conceitos básicos dessa tecnologia partindo de uma perspectiva da administração de sistemas.
tags: docker, sysadmin, gitlab.
cover_image: https://thepracticaldev.s3.amazonaws.com/i/b16v56drk48gkq5092ve.png
---

O uso de containers na virtualização e isolamento de aplicações está cada vez mais em alta nos projetos de código aberto e o Docker se destaca como a principal tecnologia do mercado, tendo sido recentemente classificada como a plataforma mais buscada no Stack Overflow (e a segunda mais amada, perdendo apenas para o Linux). A seguir, falaremos de conceitos básicos dessa tecnologia partindo de uma perspectiva da administração de sistemas.

## Administrar Sistemas

Algumas funções que um administrador de sistemas é encarregado de exercer são de instalar, suportar e fazer manutenção de servidores e, como o nome já sugere, sistemas em geral. Isso significa que, em uma equipe de desenvolvimento, as plataformas que são utilizadas pelos desenvolvedores devem ser fornecidas por profissionais dessa área, como sistemas de controle de versão, de revisão de código, controle de processo e diversos outros sistemas criados especialmente para o ambiente de desenvolvimento.

Dependendo da necessidade da equipe de desenvolvimento, as escolhas das tecnologias podem ocasionar conflitos entre si, como sistemas que usam a mesma linguagem de programação em versões distintas ou sistemas legados que utilizam tecnologias que não recebem mais suporte. Esses problemas crescem exponencialmente quando todos eles estão rodando em uma única máquina física.

Conseguir separar recursos computacionais (memória, espaço em disco e processamento), além de ter um isolamento completo de cada sistema, é o ideal para o administrador de sistemas e é por isso que o Docker, aliado aos seus containers, se torna o mais novo parceiro para essa área.

## Hello Docker World!

![Banner Docker](https://raw.githubusercontent.com/victorhundo/docker-guide/master/assets/docker-banner.jpg)

Agora que entendemos a importância do Docker para a instalação e manutenção de sistemas, vamos falar de alguns conceitos básicos dessa tecnologia e recomendamos que você tenha o Docker instalado na sua máquina para darmos prosseguimento com esse texto. Caso não tenha instalado, recomendamos que leia a [documentação de como fazer isso](https://docs.docker.com/install/).



Para verificar que seu ambiente está configurado de forma correta, basta executar o seguinte comando abaixo. Se aparecer a mensagem “Hello from Docker!”, tudo ocorreu bem:

```
sudo docker run hello-world
```

![Hello-World-Gif](https://raw.githubusercontent.com/victorhundo/docker-guide/master/assets/git-hello.gif)

Ao executar esse comando, nós chamamos o binário do Docker, criamos um container a partir da opção `run` e dizemos que o container que será criado é da imagem `hello-world:latest`. Uma imagem Docker é um container pré-configurado em que será utilizado como referência para **criar containers**. As imagens são baixadas do Docker Hub; no nosso caso, utilizamos a imagem [hello-world](https://hub.docker.com/_/hello-world)

## Subindo um sistema com Docker

Agora que temos o Docker instalado e tivemos uma breve ideia de como criar um container, vamos criar algo mais útil para um ambiente de desenvolvimento: um container do GitLab!

Para isso, execute:
```
sudo docker run \
  -p 8080:80 \
  -p 222:22 \
  --name gitlab \
  -v /srv/gitlab/config:/etc/gitlab \
  -v /srv/gitlab/logs:/var/log/gitlab \
  -v /srv/gitlab/data:/var/opt/gitlab \
  gitlab/gitlab-ce:latest
```
Utilizamos o parâmetro **name** para que possamos identificar com mais facilidade o container quando ele for listado através do comando `docker ps`. A saída desse comando será algo como:

```
CONTAINER ID        IMAGE                          COMMAND                  CREATED             NAMES                          NAMES                                               

80e2ce68a5bf        gitlab/gitlab-ce:latest   "/assets/wrapper"        5 weeks ago            0.0.0.0:222->22/tcp, 0.0.0.0:8080->80/tcp, 443/tcp   gitlab
```
Além do nome do container e a partir de qual imagem ele será criado, também especificamos parâmetros de **volume** (parâmetro -v) e **porta** (parâmetro -p), que são fundamentais para a administração de qualquer sistema e que explicaremos com mais detalhes a seguir.

### Volumes

Volumes são a forma com que o Docker gerencia o sistema de arquivos, fazendo ligação da virtualização dentro do container com os diretórios da máquina real. Os containers foram projetados para ser efêmeros: caso existam dados que precisem ser salvos ou lidos/escritos é importante utilizar volumes; caso contrário, os dados serão perdidos com a destruição do container.

Existem dois tipos de volumes: "com bind" e "sem bind". Volumes com bind são utilizados quando você quer especificar um caminho na sua máquina que será compartilhado com o container, enquanto nos volumes sem bind a especificação do endereço é feita pelo próprio Docker.

A criação do volume pode ser feito na criação do container utilizando o parâmetro `-v`:

```
# Exemplo de criação de volume com bind
-v /home/user/container_data:/opt/app
```

```
# Exemplo de criação de volume sem bind
-v /opt/app (volume sem bind)
```

Normalmente, volumes com bind são utilizados quando é **preciso fazer modificações** nos dados frequentemente (ao alterar os dados na pasta da sua máquina real em que está sendo feito o bind, os dados dentro do container também serão alterados). **Arquivos de configuração ou códigos de desenvolvimento são exemplos de bons usos desse tipo de volume.**

Já os volumes sem bind geralmente são utilizados quando queremos **apenas salvar as informações** e queremos apenas ler esses dados. **Banco de dados** é um exemplo que pode utilizar esse tipo de volume.

No nosso caso, utilizamos o parâmetro de volume três vezes, para mapear e separar arquivos de configuração, arquivos de log e os dados do GitLab:

```
-v /srv/gitlab/config:/etc/gitlab \
-v /srv/gitlab/logs:/var/log/gitlab \
-v /srv/gitlab/data:/var/opt/gitlab \
```

O diretório da esquerda, por exemplo, `/src/gitlab/config`, se refere ao diretório da máquina real, enquanto o da direita é o do container. Os arquivos alterados nesse diretório da máquina real também serão alterados dentro do container (e vice-versa).

Além de facilitar a configuração e personalização dos sistemas, utilizar volumes também facilita na criação de backups ou na disponibilidade, já que ao subir um container com os dados que estão nos diretórios mapeados em outra máquina fará com que você tenha uma redundância do sistema.

### Portas

Uma das coisas mais poderosas do Docker é a sua gerência de rede. É possível [rotear todo o tráfego de rede do container através do Tor](https://github.com/jessfraz/onion), por exemplo, mas vamos nos conter em apenas abordar o redirecionamento de portas que é feito utilizando o **parâmetro -p**.

Serviços em redes são acessados através de portas, como páginas http (80), servidores de email (25) e login remoto (22) e precisam especificar, além do endereço IP, a porta de comunicação. O GitLab utiliza tanto uma interface web na porta 80, quanto pode-se acessá-lo remotamente com ssh na porta 22, porém é muito provável que outras aplicações já utilizem essas mesmas portas para outras aplicações na máquina real. Por isso, foram feitos os seguintes mapeamentos:

```
-p 8080:80 \
-p 222:22 \

```

Ou seja, ao acessar a porta 8080 na máquina real, o Docker irá redirecionar para a porta 80 dentro do container, na qual está rodando o serviço web do GitLab. Da mesma forma é feito o mapeamento da porta 222 para a 22.

Acessando http://localhost:8080, você terá acesso ao seu GitLab.

Usando esse parâmetro do Docker com [proxy Apache](https://httpd.apache.org/docs/2.4/howto/reverse_proxy.html), por exemplo, é possível servir diversos serviços com redirecionamento para aplicações completamente isoladas através de containers.

![apache+docker](https://i.imgur.com/1qb8icC.png)



## Considerações Finais


Resolver problemas de conflitos, fazer cópia do sistema para realizar testes, diminuir tempo de upgrade/manutenção e até melhorar a organização dos arquivos de configurações são vantagens que o uso do Docker pode trazer para o administrador de sistemas.

Com uma breve explicação foi possível subir uma poderosa aplicação como o GitLab. Encorajamos que você se aprofunde nesse mundo de containers e, para isso, recomendamos:

1. [Documentação Docker](https://docs.docker.com/);
2. [Documentação da Imagem Docker do GitLab](https://docs.gitlab.com/omnibus/docker/);
3. [Jessie Frazelle (referência no mundo Docker)](https://github.com/jessfraz/dockerfiles);
4. [Canal do YouTube LINUXtips](https://www.youtube.com/watch?v=0cDj7citEjE);
5. [Oficina de Docker no GitHub](https://github.com/victorhundo/docker-guide).

Muito obrigado pela leitura! Fique atento: em breve, teremos novos artigos de contribuidores do OpenDevUFCG aqui no **dev.to**. Acompanhe o OpenDevUFCG no [Twitter](https://twitter.com/OpenDevUFCG), no [Instagram](https://instagram.com/OpenDevUFCG) e, claro, no [GitHub](https://github.com/OpenDevUFCG).
