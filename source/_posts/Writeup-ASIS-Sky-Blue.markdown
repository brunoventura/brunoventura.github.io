---
title: Write-up | ASIS2016 Finals | Forensics - Sky Blue
date: 2016-09-12 12:03:19
author: "Bruno Ventura"
header-img: "maxresdefault.jpg"
tags:
    - ASIS
    - ctf
    - writeup
    - Forensics
    - scripting
    - nodejs
---

## Descrição

Why is the [sky blue](blue.pcap)?

## Solução

Extraindo o arquivo temos um .pcap, que nos dá a entender que é um arquivo de transmissão de rede, meu wireshark estava desatualizado e não consegui abrir o arquivo nele. Analisando o arquivo com cat ou strings é possível perceber facilmente que se trata de um log ou sniff de comunicação por bluetooth entre dois dispositivos:

```
➜  blue strings blue.pcap | head
OS X Version 10.11.5 (Build 15F34) / Model ID: MacBookPro11,3
Bluetooth Software Version: 4.4.5f3 17904
nW`v
Host Controller:  Broadcom / 0x05AC / 0x8289 / v118 c9121 / Built-In (Yes) / Location ID (0x14330000)
Support:  Deep Idle (No) / WoBT (Yes) / BTRS (No) / New Idle Policy (Yes) / Idle Time (500 ms)
nWrz
CONNECTED: 78:31:C1:2E:76:31 - Handle: 0x0040 - 0x0000 - "iPhomate"
CONNECTED: CC:44:63:98:24:46 - Handle: 0x0042 - 0x0000 - "NotePad"
[0xC000] [IOBluetoothHCIRequest][Start] --  OpCode 0x200C (LE Set Scan Enable) from: blued (92)  Synchronous  status: 0x00 (kIOReturnSuccess) state: 2 (BUSY) timeout: 4321
[IOBluetoothHostController][SendHCIRequestFormatted] -- request was put on wait queue -- mNumberOfCommandsAllowedByHardware is 0, inID = 190, opCode = 0x0c17, requestPtr = 0xffffff802479e000
```

Usando binwalk é possível encontrar um header de png dentro do pcap:

```
➜  blue binwalk blue.pcap

DECIMAL   	HEX       	DESCRIPTION
-------------------------------------------------------------------------------------------------------
40535     	0x9E57    	PNG image, 1400 x 74, 8-bit colormap, non-interlaced
```

Extraindo esse arquivo ele retorna a seguinte imagem, que visivelmente é a flag porém está corrompida:
![alt text](blue.png)

Novamente analisando o arquivo gerado, foi possível que os logs das intruções bluetooth estavam no meio do arquivo. Então chegou a hora de colocar a mão na massa, criar um script que deveria identificar e remover tudo que não fosse parte da imagem, para conseguir visualizar o conteúdo completo.

Resolvi fazer o script em node que é a linguagem que mais tenho familiaridade (indo em contramão ao python). E o que ele deveria fazer?

1. Identificar o inicio da imagem;
2. Identificar o fim da imagem;
3. Separar o conteúdo;

Esses três primeiros passos foram feitos pois usei o pcap inteiro como input e não apenas a imagem extraida com binwalk, a segunda parte deveria:

4. Identificar o inicio da operação bluetooth;
5. Identificar o fim da operação bluetooth;
6. Remover os bytes desse intervalo;
7. Repetir esses passos até o fim do arquivo.

O notepad abaixo mostra o código funcional:
[https://tonicdev.com/brunoventura/57d6ce33ff903b130003a04e](https://tonicdev.com/brunoventura/57d6ce33ff903b130003a04e)

E o resultado "final":  

![alt text](flag.png)

OBS: O ultimo chunk do png ficou quebrado pois a última instrução não seguia o padrão das anteriores, porém como ctf também depende de tempo, resolvi não resolver até o final, já que com a imagem obtida era possível extrair a flag :)
