---
layout: post
title: "NeRF e Gaussian Splatting: cenas 3D a partir de fotos"
date: 2026-09-04
numero: 4
categories: [3d, ia]
tags: [nerf, gaussian splatting, reconstrução 3d, redes neurais]
---

Com um punhado de fotos de um objeto ou de um ambiente, dá para gerar uma cena 3D
em que você move a câmera livremente, inclusive para ângulos que nunca foram
fotografados. Duas técnicas recentes fazem isso muito bem, e de formas bem
diferentes: o NeRF e o Gaussian Splatting.

As duas partem do mesmo ponto. Antes de qualquer coisa, o computador precisa
descobrir de onde cada foto foi tirada, ou seja, a posição e o ângulo da câmera em
cada imagem. Isso é feito procurando pontos em comum entre as fotos, os mesmos
cantos e detalhes que aparecem em imagens diferentes, e, a partir de como esses
pontos se deslocam de uma foto para outra, calculando a posição de cada câmera. Com
as fotos e as posições em mãos, cada técnica constrói a cena de um jeito.

O NeRF (sigla para Neural Radiance Fields) representa a cena inteira dentro de uma
rede neural. A ideia é treinar a rede para responder a uma pergunta simples, feita
ponto a ponto no espaço:

```
rede(x, y, z, direção de visão)  ->  cor e densidade naquele ponto
```

Ou seja, para qualquer ponto do espaço e qualquer direção de onde você olha, a rede
diz qual é a cor ali e o quão sólido ou vazio aquele ponto é. Para gerar a imagem
de um novo ângulo, o processo lembra o traçado de raios:

```
para renderizar um pixel:
    lança um raio da câmera passando pelo pixel
    escolhe vários pontos ao longo desse raio
    pergunta à rede a cor e a densidade de cada ponto
    combina tudo, dando mais peso aos pontos mais sólidos, para achar a cor final
```

O treinamento é o que faz a coisa funcionar. A rede começa chutando, o computador
compara as imagens que ela gera com as fotos reais e vai ajustando a rede até que
os resultados fiquem parecidos com as fotos. Quando isso acontece, a rede virou uma
cópia 3D da cena. O ponto fraco do NeRF é o custo, porque treinar e gerar as
imagens costuma ser lento e pesado.

O Gaussian Splatting resolve a cena de outro jeito, sem esconder tudo dentro de uma
rede. Ele representa o ambiente como uma nuvem enorme de pequenas manchas
tridimensionais, chamadas de gaussianas. Cada mancha tem uma posição no espaço, um
tamanho, uma cor e um grau de transparência. Para gerar a imagem, o computador
projeta essas manchas na tela e as mistura, uma sobre a outra, formando a cena.
Esse processo é muito mais rápido, a ponto de rodar em tempo real, e costuma gerar
imagens bem nítidas. O treinamento segue a mesma lógica do NeRF, com o computador
comparando o resultado com as fotos e ajustando a posição, a cor e a transparência
de cada mancha até tudo bater.

A diferença central entre os dois está em onde a cena fica guardada. No NeRF, ela
vive dentro dos números de uma rede neural, o que é compacto, mas custa caro para
desenhar. No Gaussian Splatting, ela vive como milhões de manchas espalhadas pelo
espaço, o que ocupa mais, mas desenha muito mais rápido.

O resultado, nos dois casos, é o mesmo tipo de coisa que era difícil de conseguir
antes: pegar fotos comuns, tiradas com uma câmera qualquer, e transformar em uma
cena 3D que você explora de qualquer ponto de vista. Isso tem uso em realidade
virtual, videogames, mapeamento de lugares e efeitos visuais, e é hoje uma das
áreas que mais avançam dentro da computação visual.
