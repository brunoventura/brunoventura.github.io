---
title: Emulador CHIP-8 em JavaScript do zero | Parte 1
date: 2016-07-13 10:03:19
author: "Bruno Ventura"
header-img: "maxresdefault.jpg"
tags:
    - JavaScript
    - js
    - Emulador
    - CHIP-8
---

## Introdução

Essa primeira parte tratará principalmente da parte conceitual de um emulador e de arquitetura de computadores.

**[TL;DR Pular para a parte interessante](#O-que-e-um-emulador)**

Desde que tive meu primeiro computador (um 486) um dos meus maiores entretenimentos foi a capacidade de poder baixar e instalar emuladores e jogos para os mesmos. Era um universo muito amplo, ter acesso a qualquer jogo de qualquer plataforma (apenas a uma internet discada de distância rsrs), algo que pelo menos para minha realidade era algo intangivel com os poucos consoles físicos que tive e o alto custo dos jogos.

O tempo passou e eventualmente ingressei no mundo da tecnologia como desenvolvedor de softwares, juntamente com esse advento a curiosidade sobre os emuladores aumentou, e um questionamento começou a me instigar:

> Como diabos um emulador funciona…

Curso sistemas de informação, e tive uma matéria de arquitetura de computadores, logo, eu tinha entendido (muito pouco) como era o funcionamento de um computador, mas mesmo assim, era dificil compreender o que de fato consistia em um emulador. Nesse momento eu já havia trabalhado programando 2 anos em Java e estava começando a trabalhar com Javascript/Node.js, sabia que existiam emuladores no que rodavam no browser e estava *[mind blown](https://media.giphy.com/media/TlK63EWmJDBa5MjIr6g/giphy.gif)* com pequenas possibilidades como criar um servidor REST em 10 linhas de código e que javascript ia muito além do JQuery, pensei:

> Já que roda no browser e é em JavaScript não deve ser dificil entender, tá pra mim! Vou começar por um fácil, NES...

Doce ilusão… lembro de ter acessado um repositório do Github (não tenho mais certeza de exatamente qual, mas acredito que seja esse: [jsnes](https://github.com/fcambus/jsemu)) e foi como se tivesse levado uma paulada na cabeça

```javascript

// Find address mode:
var addrMode = (opinf >> 8) & 0xFF;

// Increment PC by number of op bytes:
var opaddr = this.REG_PC;
this.REG_PC += ((opinf >> 16) & 0xFF);
```

Eu conhecia os operadores Bitwise (que até então eram conhecidos só como operadores lógicos) AND, OR, XOR, NOT que são os básicos que se aprende na faculdade, porém nunca são utilizados no mundo real das pessoas que amam a família. Porém >>, >>>, << fora o fato que o uso de hexadecimal só fazia parecer ainda mais complexo o código. Obviamente como toda pessoa com muita motivação faria, eu larguei tudo de lado e achei que nunca mais olharia pro código de um emulador de novo.

Até que um dia algum tempo depois eu esbarrei com essa pergunta do [stackoverflow](http://stackoverflow.com/questions/448673/how-do-emulators-work-and-how-are-they-written):

![alt text](stackoverflow.jpg)

Nas respostas é possivel ter um amplo panorâma sobre o que de fato é a programação de um emulador, com vários links de [referência](#summary) muito interessantes. Nesse ponto, estava bem claro para mim que apesar de eu ter algum conhecimento em javascript, havia um déficit em como realmente um computador funciona, seja ele emulado ou não. Vendo isso percebi que seria uma ótima forma de ter vários ganhos:

1. Eu aprenderia low-level e arquitetura de computadores na prática
2. Eu estaria mechendo com emuladores
3. Consolidaria conhecimentos em JavaScript

## Para quem é essa série

Para acompanhar a série é importante um background mesmo que básico em programação, durante todo o processo usaremos JavaScript, porém tirando a parte de renderização todo o resto é facilmente portado para qualquer linguagem, conceitos como operações bitwise serão explicadas no decorrer do tutorial. Não é necessário ter conhecimento em emuladores ou em baixo nível (como assembly), alguns conceitos serão explicados durante o tutorial, e alguns serão linkados para outros artigos. E principalmente ter curiosidade para tirar dúvidas e ir atrás de outras fontes de informação.

## O que é um emulador

Segundo o dicionário a palavra emulador significa:

> Sentimento nobre que nos leva a igualar ou exceder outra pessoa em virtudes e merecimentos.


Segundo a definição do [Wikipedia](https://en.wikipedia.org/wiki/Emulator) para emuladores computacionáis:

> Em computação, um **emulador** é um hardware ou software que possibilita um sistema computacional (chamado de *host*) se comportar como outro sistema computacional (chamado de *guest*).

Diz-se que o primeiro emulador foi criado quando o primeiro computador precisou ser trocado por um novo que fosse compatível com o anterior. Logo, os softwares do computador anterior precisavam ser portados para o novo. Para esse fim existem várias soluções, como recompilar o software para o novo CPU (caso você tenha o source code), reescrever toda a aplicação (nhé...), traduzir todas as syscalls juntamente com binário, ou também criar um emulador da máquina antiga na nova.

O primeiro emulador conhecido foi da IBM na década de 60. A cena da emulação de jogos inicio apenas na década de 90 quando os computadores pessoais começaram a ficar mais acessiveis e potentes.

Abaixo segue uma pequena listagem de benefícios e dificuldades que são encontrados na emulação:

#### Vantagens

- Melhorar qualidade de imagem e audio (remasterização)
- Podem ser adicionadas features (speed++)
- Save state (feature de destaque)
- Utiliza mesmas mídias do console original
- Facilidade de debug
- Forte cena open-source
- Garante que uma plataforma não seja esquecida!!!

#### Dificuldades

- Nem sempre é simples
- Copyright (Processinho)
- Propriedade intelectual (dificuldade de encontrar documentação)
- Demanda um hardware muito mais potente que o emulado
- Burlar alguns sistemas anti pirataria
- Necessidade de engenharia reversa

## Arquitetura de computadores

Antes de começar com a mão na massa é importante relembrar (ou conhecer) alguns conceitos sobre como um computador funciona, isso ajudará bastante na hora de visualizar o contexto geral e como se dá o funcionamento do nosso emulador

### Continua...
