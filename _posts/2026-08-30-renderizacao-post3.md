---
layout: post
title: "Como um modelo 3D vira uma imagem na tela"
date: 2026-08-30
numero: 3
categories: [3d, computação gráfica]
tags: [renderização, rasterização, ray tracing, gpu]
---

Um modelo 3D dentro do computador é basicamente geometria: pontos e triângulos
posicionados no espaço, junto com informações de cor e de material. Mas a sua
tela é plana, feita de pixels. O processo que transforma essa cena tridimensional
em uma imagem 2D que aparece no monitor se chama renderização. Existem dois
caminhos principais para fazer isso: a rasterização e o traçado de raios (ray
tracing).

A rasterização parte da geometria. O primeiro passo é a projeção: cada vértice do
modelo, que vive numa posição (x, y, z) no espaço, é convertido para uma posição
na tela. A parte que cria a sensação de profundidade é a divisão pela distância,
ou seja, quanto mais longe da câmera um ponto está, mais ele é encolhido em direção
ao centro, o que faz objetos distantes parecerem menores. Com os três vértices de
um triângulo já projetados, o computador descobre quais pixels caem dentro dele e
os pinta. Para decidir o que fica na frente, ele usa uma técnica chamada z-buffer:
além da cor, ele guarda para cada pixel a profundidade do que foi desenhado ali, e
só substitui aquele pixel se o novo triângulo estiver mais perto da câmera do que o
que já estava. Isso é rápido o suficiente para rodar em tempo real, e as placas de
vídeo (GPUs) são construídas justamente para fazer essa conta para milhões de
triângulos por segundo.

O traçado de raios funciona ao contrário, partindo dos pixels. De forma
simplificada, a lógica é essa:

```
para cada pixel da tela:
    raio = parte da câmera e passa por esse pixel
    ponto = primeiro objeto que o raio atinge na cena
    se atingiu algo:
        cor do pixel = calcula a iluminação nesse ponto
    senão:
        cor do pixel = cor do fundo
```

O passo de "primeiro objeto que o raio atinge" é uma conta de geometria: o
computador testa o raio contra os objetos da cena e fica com o ponto de impacto
mais próximo. Já o cálculo da iluminação costuma começar por uma regra simples,
que é: quanto mais a superfície estiver virada de frente para a luz, mais clara
ela fica. Isso é medido pelo ângulo entre a direção da luz e a normal da
superfície, que é a direção para onde aquele pedaço da superfície aponta. A partir
do ponto de impacto, o computador ainda pode lançar novos raios, um em direção à
luz para saber se o ponto está na sombra de outro objeto, e outro na direção do
reflexo para desenhar superfícies espelhadas. É essa cadeia de raios que dá o
realismo.

A diferença entre os dois é uma troca entre velocidade e realismo. A rasterização
é rápida, mas precisa de vários truques para imitar uma iluminação convincente. O
traçado de raios entrega imagens mais realistas, porém exige muito mais
processamento, o que o tornou por muito tempo mais comum em filmes e efeitos
visuais, onde cada quadro pode levar minutos ou horas para ser gerado. Hoje as
placas de vídeo mais novas já conseguem fazer traçado de raios em tempo real, e
muitos jogos combinam os dois, usando rasterização para a base da imagem e traçado
de raios apenas para alguns efeitos, como reflexos e sombras.

Nos dois casos, além da geometria, o computador precisa saber onde está a câmera e
para onde ela aponta, onde estão as luzes da cena e de que material é feito cada
objeto, se ele é fosco, brilhante, metálico ou transparente. São esses dados que
definem como cada superfície reage à luz e, no fim, a cor de cada pixel.

No fim, renderizar é responder, pixel por pixel, a uma única pergunta: o que a
câmera enxerga naquele ponto e de que cor isso aparece. A rasterização responde
partindo dos objetos, e o traçado de raios responde partindo da luz, mas os dois
chegam ao mesmo lugar, que é a imagem final que aparece na tela.
