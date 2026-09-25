---
layout: post
title: "Embeddings: como o computador sabe que duas imagens são parecidas"
date: 2026-09-25
numero: 6
categories: [ia, visão computacional]
tags: [embeddings, vetores, cnn, clip, busca por imagem]
---

No [post anterior](/blog_comp_visual/posts/da-borda-ao-objeto/) a gente viu que uma
rede neural vai da borda até o objeto, empilhando convoluções. O que eu não contei
é o que sai no final desse processo: a rede transforma a imagem inteira em um
**vetor de números**, com algumas centenas de valores, que resume o conteúdo dela.
Esse vetor tem um nome: **embedding**. E ele resolve uma pergunta que parece
simples, mas é difícil: como o computador sabe que duas imagens são parecidas?

O jeito ingênuo seria comparar pixel a pixel. O problema é que isso quase nunca
funciona. Uma foto de um gato de dia e a mesma cena de noite têm valores de pixel
completamente diferentes, mesmo sendo, pra gente, "a mesma coisa". No nível do
pixel, elas estão longe; no nível do conteúdo, são quase idênticas.

O embedding resolve isso porque ele não guarda "qual a cor do pixel tal", e sim
algo mais parecido com o significado da imagem: se tem um animal, uma paisagem,
que formas e texturas aparecem. E tem uma propriedade que é a chave de tudo:
imagens com conteúdo parecido geram vetores próximos, e imagens diferentes geram
vetores distantes. Aquelas duas fotos do gato, que estavam longe no nível dos
pixels, ficam pertinho uma da outra no espaço dos embeddings.

Isso é poderoso porque transforma uma pergunta difícil ("estas duas imagens são
parecidas?") em uma pergunta de geometria simples ("estes dois vetores estão
perto?"). É essa ideia que está por trás da busca por imagem (aquele "pesquisar
com uma foto"), de encontrar fotos duplicadas ou parecidas numa galeria, e de
sistemas que recomendam imagens no mesmo estilo.

E aqui fecha um ciclo pra mim. No [meu primeiro post](/blog_comp_visual/posts/computacao-visual/)
eu contei que, antes da disciplina começar, imaginava Computação Visual bem ligada
a usar embeddings pra representar imagens numericamente. O que eu não sabia é que
os embeddings vêm no fim de uma escada inteira: primeiro a imagem vira pixels,
depois bordas, depois formas, e só então esse vetor cheio de significado. A minha
ideia inicial pulava direto pra última etapa, sem ver tudo que vem antes.

Tem ainda uma virada que eu achei genial. Dá pra treinar o modelo de um jeito que
não só as imagens virem vetores, mas o **texto também**, no mesmo espaço. É o que
faz o CLIP: uma foto de um cachorro na praia e a frase "um cachorro na praia"
acabam caindo perto uma da outra. É assim que funciona buscar imagem escrevendo
uma frase, e é uma das bases por trás dos geradores de imagem por texto.

No fim das contas, é disso que a Computação Visual trata: pegar aquilo que a gente
enxerga e transformar em números que o computador consegue comparar e entender. A
gente começou nos pixels, passou pelas bordas, e chegou num vetor que, de certa
forma, carrega o sentido da imagem.
