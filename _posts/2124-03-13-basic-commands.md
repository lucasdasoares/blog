---
layout: post
title:  "Ciências de Dados: A Biblioteca Pytrends"
subtitle: "Utilize o Google Trends nas suas previsões"
date:   2124-03-13 00:00:00 -0300
categories: data_science pytrends
author: Prof. Lucas Soares
---

Esse é um texto em linguagem markdown.

Essa é uma equação.

$$\begin{aligned} 2x - 4 &= 6 \\ 2x &= 10 \\ x &= 5 \end{aligned}$$

Essa é uma imagem.
{:refdef: style="text-align: center;"}
![image-description]({{site.baseurl}}/assets/perfil.webp){: width="400" }<br>*Descrição da imagem.*
{: refdef}

Esse é um código em Python.
{% highlight python %}
import cv2 as cv
import mediapipe as mp
import os
import numpy as np

# Inicializa o detector do mediapipe.
mpHands = mp.solutions.hands
hands = mpHands.Hands()   

# Cria listas para armazenar pontos e labels.
listaPontos = []
listaLabels = []

# Caminho para o diretório central.
dirCentral = '/home/lucas/Downloads/ASL_Dataset/Train/'
os.chdir(dirCentral)
listaPastas = ['A','B','C','D','E','F','G','H','I','J','K','L','M','N','O','P','Q','R','S','T','U','V','W','X','Y','Z','Space','Nothing']
# Percorre todas as pastas.
for contLabel, pasta in enumerate(listaPastas):
    dirPasta = dirCentral + pasta + '/'
    os.chdir(dirPasta)
    print('Pasta atual: ', dirPasta)
    listaArquivos = os.listdir()
    
    # Percorre todos os arquivos em uma pasta.
    for arquivo in listaArquivos:
        # Lê a imagem.
        img = cv.imread(arquivo)
        # Converte a imagem para RGB.
        img = cv.cvtColor(img, cv.COLOR_BGR2RGB)
        # Processa a imagem pelo mediapipe.
        results = hands.process(img)
        
        if results.multi_hand_landmarks:
            # Obtém as coordenadas de cada um dos pontos da mão.
            matrizPontos = np.zeros((21, 3))
            for p in range(21):
                matrizPontos[p, 0] = results.multi_hand_landmarks[0].landmark[p].x
                matrizPontos[p, 1] = results.multi_hand_landmarks[0].landmark[p].y
                matrizPontos[p, 2] = results.multi_hand_landmarks[0].landmark[p].z
                
            # Salva os pontos na lista de pontos.
            listaPontos.append(matrizPontos)
            # Salva o label na lista de labels.
            listaLabels.append(contLabel)
os.chdir('/home/lucas/Downloads/ASL_Dataset')
listaPontos = np.array(listaPontos)
listaLabels = np.array(listaLabels)
np.save('dados_test.npy', listaPontos)
np.save('labels_test.npy', listaLabels)
{% endhighlight %}
