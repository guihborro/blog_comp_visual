---
layout: post
title: "Como a IA gera imagens: os modelos de difusão"
date: 2026-10-01
numero: 7
categories: [ia, visão computacional]
tags: [difusão, geração de imagens, stable diffusion, clip]
---

No [post sobre embeddings](/blog_comp_visual/posts/embeddings/) a gente viu como a
máquina transforma uma imagem num vetor de significado e, com o CLIP, chega a ligar
imagem e texto no mesmo espaço. Agora vem o caminho contrário, que é o que mais
impressiona hoje em dia: como a IA cria uma imagem do zero a partir de uma frase. A
técnica por trás disso se chama difusão.

A ideia central é meio contraintuitiva, porque ela começa pelo avesso, ensinando o
modelo a destruir uma imagem. No treino, pega-se uma foto real e vai adicionando
ruído aleatório aos poucos, em vários passos, até a imagem virar pura estática,
aquele chuvisco de TV sem sinal. O que o modelo aprende é a fazer o inverso: olhar
uma imagem com ruído e prever qual ruído foi adicionado, pra conseguir remover um
pouco dele.

Depois de treinado, gerar uma imagem é usar essa habilidade ao contrário.
Começa-se com uma tela de puro ruído aleatório e aplica-se, muitas vezes seguidas,
o passo que o modelo aprendeu: remover um pouco de ruído. A cada passo a estática
vai ficando um pouco mais organizada, até que, depois de dezenas de passos, surge
uma imagem coerente que nunca existiu antes.

Mas falta a parte mais importante: como a imagem sai parecida com o que a gente
pediu? É aqui que o post anterior se encaixa. A frase que você escreve é
transformada em um vetor (um embedding de texto, como o do CLIP) e esse vetor guia
a remoção de ruído em cada passo. Em vez de só tirar ruído, o modelo tira ruído na
direção de uma imagem que combina com o texto. Por isso "um gato astronauta" e "uma
praia ao pôr do sol" levam o mesmo chuvisco inicial a resultados completamente
diferentes: o texto está empurrando cada passo para um lado.

Um detalhe prático: os modelos mais usados hoje não fazem isso direto nos pixels, o
que seria pesado demais, e sim em uma versão comprimida da imagem, o que deixa a
geração bem mais rápida. E é esse mesmo processo que está por trás das ferramentas
que geram arte, preenchem partes que faltam em uma foto ou trocam o fundo de uma
imagem.

Pra mim, isso fecha o blog de um jeito legal. A gente começou vendo como o
computador aprende a enxergar, saindo dos pixels para as bordas e dos objetos para
os embeddings. E chegou no ponto em que ele usa essas mesmas peças, pixels e
vetores de significado, não para entender uma imagem, mas para criar uma. A
Computação Visual deixou de apenas interpretar o que existe e passou também a
inventar o que não existe.
