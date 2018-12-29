---
layout: post
title: "Robô Autônomo para Resgate em Situações de Desastre"
date: 2018-12-27 12:00:00
author: Rafael Almeida
permalink: blog/robo_autonomo_desastre.html
description: Algoritmo de navegação para robô autônomo de resgate
image: /images/postImages/roboAutonomo/Incendie_a_Rome.jpg
imageName: Incêndio de Roma
imageAuthor: Hubert Robert
comments: true
---

<style>
#imageCenter{
    display: block;
    margin-left: auto;
    margin-right: auto;
    width: 50%;
}
</style>

Dia 02 de Setembro de 2018. Às 19:30h, após o fechamento para visitação, [um incêndio é identificado no maior museu de história natural do Brasil](https://g1.globo.com/rj/rio-de-janeiro/noticia/2018/09/04/o-que-se-sabe-sobre-o-incendio-no-museu-nacional-no-rio.ghtml): o [Museu Nacional](http://www.museunacional.ufrj.br/), localizado na cidade do Rio de Janeiro. A instituição contava com inúmeras peças como fósseis, múmias, exemplares de animais raros (alguns deles peças únicas no planeta) entre muitos outros itens irreparáveis, perdidos no maior desastre científico de nossa história. A situação era tão calamitosa que até mesmo as equipes de busca sentiram dificuldades no combate ao incêndio e no resgate, como relatado por [bombeiros que se feriram na tentativa de retirar o crânio de Luzia](https://g1.globo.com/rj/rio-de-janeiro/noticia/2018/09/04/bombeiro-diz-que-se-queimou-ao-tentar-resgatar-luzia-no-museu-nacional.ghtml), o fóssil humano mais antigo encontrado na América do Sul. A fim de evitarmos risco aos seres humanos e atuar de forma mais eficiente em situações semelhantes, uma das alternativas seria o uso de veículos autônomos desenvolvidos para resgate de objetos e vítimas. Pensando nisso, desenvolvi algoritmos de navegação a serem embarcados em um robô em um cenário simulado semelhante, descritos logo a seguir.

Antes de iniciarmos, vale ressaltar que a estratégia e o cenário foram baseados na proposta da competição [Robocup Junior Rescue Competition](http://www.robocup2016.org/en/leagues/robocupjunior/rescue/). Você pode fazer o download do código a partir de [meu diretório no GitHub](https://github.com/rafaelsa97/BallPickingChallenge_RobocupJuniorRescueCompetition). A estratégia de navegação foi escrita em Python, utilizando um ambiente gerado no [Gazebo](http://gazebosim.org/) para visualizar e simular o campo de prova no qual o robô seria colocado. O simulador foi executado no [ROS Development Studio](http://www.theconstructsim.com/rds-ros-development-studio/), uma plataforma online que permite simulações de robôs baseados em ROS [(Robot Operating System)](http://www.ros.org/) na nuvem. Utilizou-se o robô de modelo [P3-DX](http://www.mobilerobots.com/ResearchRobots/PioneerP3DX.aspx) para simular o algoritmo, uma vez que ele conta com diversos sensores ultrassônicos, uma câmera Kinect Sensor e um manipulador em formato de pinça na parte dianteira do veículo.  
<img id="imageCenter" src="/images/postImages/roboAutonomo/p3dx.png" alt="P3-DX">  
<center>O robô Pioneer P3-DX</center>  

O desafio consistia em duas partes: inicialmente, o robô deveria percorrer uma linha tracejada no chão até chegar em um ponto final. Durante o percurso, ele deveria desviar de obstáculos, tratar falhas no traçado do caminho, passar por curvas bruscas, entre outros empecilhos. Na segunda parte, o veículo era colocado em um ambiente de simulação de resgate de vítimas (modeladas como bolas azuis), cabendo ao robô identificá-las, definir uma trajetória, agarrá-las com seu gripper e deixá-las em uma área de segurança, destacada em vermelho.  
<img id="imageCenter" src="/images/postImages/roboAutonomo/robocup.png" alt="Campo de Prova">  
[<center>Campo de prova</center>](http://rcj.robocup.org/rescue.html)

Para a primeira parte do trajeto, utilizei a biblioteca [OpenCV](https://opencv.org/) para obtenção e tratamento das imagens da câmera do Kinect, a fim de localizar a linha tracejada no chão. A estratégia básica foi definir uma centróide nas imagens (identificando as linhas pela sua cor) e, a partir de sua posição no plano cartesiano, alterar a velocidade angular do robô de forma que a centróide permanecesse no centro, fechando assim uma malha de controle. A velocidade linear do robô foi mantida constante, enquanto a angular é determinada por um controlador automático de constante proporcional Kp, cujo valor foi determinada pelo método de inspeção direta.

Os obstáculos deixados no meio do percurso eram identificados por meio dos sensores ultrassônicos posicionados na parte frontal do veículo. Ao detectar sua proximidade, o robô inicia uma rotina em que percorre o bloqueio com velocidade linear constante, variando sua velocidade angular de acordo com sua distância lateral em relação ao objeto, retomando a função de seguidor de linha ao detectar as cores do trajeto a uma distância mínima previamente definida. Por fim, as curvas acentuadas eram identificadas por quadrados verdes em suas arestas. Ao detectá-las por meio de sua câmera, o robô gira em torno de seu próprio eixo até o momento que a linha contínua relativa ao trajeto encontra-se em uma posição adequada a ser seguida.

Já na segunda etapa, a estratégia de identificar as esferas azuis por meio de centróides determinadas pela cor foi mantida. Ao iniciar sua missão, o robô gira em torno de seu próprio eixo até encontrar qualquer elemento de cor azul nas imagens, seguindo em direção ao objeto assim que encontrado, corrigindo sua direção em sua caminhada. Após se aproximar a uma certa distância da esfera, o robô limita a análise apenas à metade inferior da imagem da câmera, ignorando quaisquer outras bolas no cenário. Além disso, o veículo reduz sua velocidade linear para que, de forma mais precisa, consiga se alinhar e agarrar a bola. Feito isso, o robô gira em torno do seu eixo em busca da área de segurança (identificada igualmente pela sua cor), caminhando em sua direção assim que detectada. Os passos descritos anteriormente são repetidos até que todas as “vítimas” sejam resgatadas.

Apesar de terem sido utilizados elementos ilustrativos e simplificados, os algoritmos de detecção de objetos e de navegação poderão ser usados em diversas outras situações. Caso tenha sugestões ou correções que gostaria de realizar, sinta-se à vontade para [comentar e modificar como preferir](https://github.com/rafaelsa97/BallPickingChallenge_RobocupJuniorRescueCompetition). Não se esqueça de dar uma estrela no GitHub! :)
