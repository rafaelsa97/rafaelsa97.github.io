---
layout: post
title: "Robô Autônomo para Resgate em Situações de Desastre"
date: 2018-12-27 12:00:00
author: Rafael Almeida
permalink: blog/robo_autonomo_desastre.html
description: asdfasf
image: /images/postImages/Incendie_a_Rome.jpg
comments: true
---

Dia 02 de Setembro de 2018. Às 19:30h, após o fechamento para visitação, [um incêndio é identificado no maior museu de história natural do Brasil](https://g1.globo.com/rj/rio-de-janeiro/noticia/2018/09/04/o-que-se-sabe-sobre-o-incendio-no-museu-nacional-no-rio.ghtml): o [Museu Nacional](http://www.museunacional.ufrj.br/), a instituição científica mais antiga do país, localizado na cidade do Rio de Janeiro. A instituição contava com inúmeras peças como fósseis, múmias, exemplares de animais raros (alguns deles peças únicas no planeta) entre muitos outros itens irreparáveis, perdidos no maior desastre científico de nossa história. A situação era tão calamitosa que até mesmo as equipes de busca sentiram dificuldades no combate ao incêndio e no resgate, como relatado por [bombeiros que se feriram na tentativa de retirar o crânio de Luzia](https://g1.globo.com/rj/rio-de-janeiro/noticia/2018/09/04/bombeiro-diz-que-se-queimou-ao-tentar-resgatar-luzia-no-museu-nacional.ghtml), o fóssil humano mais antigo encontrado na América do Sul. Uma das alternativas a serem utilizadas em situações como essa é o uso de veículos autônomos desenvolvidos para resgate de objetos e vítimas em situações de acidente. Pensando nisso, desenvolvi algoritmos de navegação a serem embarcados em um robô em um cenário simulado semelhante, descritos logo a seguir.

Antes de iniciarmos, vale ressaltar que a estratégia e o cenário foi baseada na proposta da competição [Robocup Junior Rescue Competition](http://www.robocup2016.org/en/leagues/robocupjunior/rescue/). Você pode fazer o download do código a partir de [meu diretório no GitHub](https://github.com/rafaelsa97/BallPickingChallenge_RobocupJuniorRescueCompetition). A estratégia de navegação foi escrita em Python, utilizando um ambiente gerado no [Gazebo](http://gazebosim.org/) para visualizar e simular o campo de prova no qual o robô seria colocado. O simulador foi executado no [ROS Development Studio](http://www.theconstructsim.com/rds-ros-development-studio/), uma plataforma online que disponibiliza que simuladores baseados em ROS [(Robot Operating System)](http://www.ros.org/) sejam processados na nuvem. Utilizou-se o robô de modelo [P3-DX](http://www.mobilerobots.com/ResearchRobots/PioneerP3DX.aspx) para simular o algoritmo, uma vez que ele conta com diversos sensores ultrassônicos, uma câmera Kinect Sensor e um manipulador em formato de pinça na parte dianteira do veículo.
