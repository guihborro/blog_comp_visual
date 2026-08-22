---
layout: post
title: "Como um scanner transforma um quarto num modelo 3D"
date: 2026-08-22
categories: [3d, geometria]
tags: [scanner 3d, nuvem de pontos, mesh, profundidade]
---

Um scanner 3D consegue capturar um ambiente inteiro e transformar tudo em um
modelo que você pode girar na tela. Esse processo acontece em dois passos
principais. Primeiro o aparelho mede a distância de cada ponto do ambiente.
Depois o computador usa essas distâncias para montar uma superfície em três
dimensões.

O scanner não capta cor, e sim distância. Ele mede o quão longe cada parte do
ambiente está dele. Isso pode ser feito de algumas formas. O LiDAR dispara um
feixe de luz e mede o tempo que ele leva para bater no objeto e voltar. Outros
aparelhos projetam um padrão de luz sobre o ambiente e observam como esse padrão
se deforma ao cair nas superfícies. Existem também sistemas que usam duas câmeras
e comparam as duas imagens para calcular a distância, de forma parecida com o que
os olhos humanos fazem. Em todos os casos, o resultado é uma medida de distância
para cada direção que o aparelho observa.

Saber a distância ainda não é o suficiente para localizar um ponto no espaço. Por
isso o computador combina cada distância medida com a direção para onde o scanner
estava apontando naquele momento. Com essas duas informações, ele calcula a
posição exata de cada ponto, ou seja, a que altura, largura e profundidade ele
está. Repetindo esse cálculo para todos os pontos medidos, o computador gera uma
nuvem de pontos, que é um conjunto de milhões de pontos posicionados no espaço.
Quando o scanner também capta cor, cada ponto guarda a sua cor junto com a
posição.

Um scanner sozinho só enxerga o que está à sua frente. Por isso o ambiente
costuma ser escaneado de vários ângulos. O computador então junta todas essas
capturas em uma nuvem única, alinhando os pontos de uma captura com os da outra
até que as partes em comum coincidam.

A nuvem de pontos ainda é apenas um conjunto de pontos separados. Existe espaço
vazio entre eles e não há uma superfície contínua, apenas o contorno sugerido
pelos pontos. Para transformar isso em um modelo sólido, o computador faz a
reconstrução da superfície. Ele liga os pontos vizinhos formando uma malha de
pequenos triângulos que cobre todo o ambiente e fecha os espaços vazios. Antes
disso, ele calcula em cada ponto para que lado a superfície está voltada, o que
indica o lado de dentro e o lado de fora de cada parte. Essa informação é o que
permite montar os triângulos na orientação correta.

Ao final desse processo, medir a distância, gerar a nuvem de pontos e reconstruir
a superfície, o resultado é um modelo tridimensional do ambiente. Esse modelo pode
ser girado, visto de qualquer ângulo e usado em jogos, em realidade aumentada ou
em impressão 3D. Todo o trabalho de trazer o ambiente real para dentro do
computador está nesses dois pontos: transformar cada distância em uma posição no
espaço e transformar o conjunto de posições em uma superfície.
