# 🦀 MangueSound

ChatGPT
O MangueSound é um protótipo de Arduino autoral, projetado para proporcionar acessibilidade cultural a pessoas com deficiência visual. Focado no enriquecimento da cultura pernambucana, ele oferece uma experiência sensorial única através do Circuito da Poesia, permitindo que usuários explorem e absorvam o rico patrimônio poético da região de forma interativa e inclusiva.


## 📺 Demo

![GIF de utilização](https://github.com/davimqz/Projeto-1/assets/160409868/3ce1cacc-eefb-4357-ac4a-0b20974587eb
)


## 🖱️ Modo de Uso

```
1º passo - Utilizar o protótipo estilo "Shoulder Bag"
2º passo - Colocar os fones de ouvidos
3º passo - Aproximar a face transparente do protótipo ao TOTEM desejado
4º passo - Aproveitar o processo de endoculturação 
```

## 🎯 Tecnologias Utilizadas

| Features             | 🦀MangueSound |
| -------------------- | :-------: |
| Sensor RFID                 |    ✔️     |
| Micro Player  (MP3)          |    ✔️     |
| MFRC522.h [Biblioteca] |    ✔️     |
| SoftwareSerial.h [Biblioteca]  |    ✔️     |
| DFRobotDFPlayerMini.h [Biblioteca]  |    ✔️     |


#### ⚙️ Funcionalidade

**Scan e Áudio** - Possibilita o escaneamento de "TAGS" para a reprodução de áudios específicos para cada estátua, os quais são responsáveis por divulgar uma pequena bibliografia sobre cada homenageado no Circuito dos Poetas


## 📦 Tecnologias

Linguagem - Arduíno => **C++**

![made-with-Arduíno](https://img.shields.io/badge/Made%20with-Arduíno-brightgreen)

## 📄 Documentação

[📄 Google Sites](https://sites.google.com/cesar.school/projeto1g18/status-report-2)

[📄 Miro](https://miro.com/app/board/uXjVNhyEkrA=/)

[📄 Artigo Tutorial](https://docs.google.com/document/d/15gwvMzpmFrWJf3u7feJsscYYtUyy2JT0z45cMhVVo4o/edit#heading=h.3qu7i54zi4hm)

## 👥 Integrantes

1 - Amanda Montarrois

2 - Davi Marques Oliveira

3 - Felipe Asfora 

4 - Isabelle Mariano 

5 - Júlia Tavares 

6 - Maria Luiza Dantas 
 
7 - Victor Burle

8 - Vítor César

9 - Pedro Jorge

10 - Luís Guilherme

## 💻 Código do Protótipo

```
[11:26, 11/06/2024] Luis CESAR: #include <SPI.h>
#include <MFRC522.h>
#include <SoftwareSerial.h>
#include <DFRobotDFPlayerMini.h>

#define RST_PIN         9   // Define o pino de reset do módulo RFID
#define SS_PIN          10  // Define o pino de seleção do escravo (SDA) do módulo RFID
#define RX_PIN          4   // RX do módulo MP3
#define TX_PIN          5   // TX do módulo MP3

MFRC522 mfrc522(SS_PIN, RST_PIN);   // Cria uma instância do objeto MFRC522
SoftwareSerial mp3Serial(RX_PIN, TX_PIN);  // Inicializa a comunicação com o módulo MP3
DFRobotDFPlayerMini myDFPlayer;

void setup() {
  Serial.begin(9600);
  mp3Serial.begin(9600);  // Inicializa a comunicação com o módulo MP3~
   // Inicializa o DFPlayer Mini
  if (!myDFPlayer.begin(mp3Serial)) {  // Use softwareSerial para comunicar
    Serial.println("Não foi possível iniciar o DFPlayer Mini");
    while(true);
  }

  SPI.begin();            // Inicializa a comunicação SPI
  mfrc522.PCD_Init();     // Inicializa o módulo RFID

  Serial.println("Aguardando cartão RFID...");

  // Defina o volume no máximo
  Serial.println("Definindo o volume no máximo...");
  //setVolume(30);  // 30 é geralmente o volume máximo
  myDFPlayer.volume(10);  // Ajusta o volume (0-30)
}

void loop() {
  if (mfrc522.PICC_IsNewCardPresent() && mfrc522.PICC_ReadCardSerial()) { // Loop para verificar se um novo cartão RFID foi encontrado
    Serial.println("Cartão RFID detectado!");

    String cardID = ""; // String para armazenar o UID do cartão 
    for (byte i = 0; i < mfrc522.uid.size; i++) { // Loop para ler o UID do cartão
      cardID += String(mfrc522.uid.uidByte[i] < 0x10 ? "0" : "");
      cardID += String(mfrc522.uid.uidByte[i], HEX);
    }
    Serial.print("ID do cartão: ");
    Serial.println(cardID);

    if (cardID == "03ddcc0e") { // UID do primeiro cartão
      Serial.println("Cartão autorizado. Reproduzindo áudio 001.mp3...");

      // Reproduza o arquivo 001.mp3
      //playAudioFile(1);
      myDFPlayer.play(1);  // Toca o primeiro arquivo MP3 no cartão SD

    } if (cardID == "832f4fad") { // UID do segundo cartão
      Serial.println("Cartão autorizado. Reproduzindo áudio 002.mp3...");

      // Reproduza o arquivo 002.mp3
      //playAudioFile(2);
      myDFPlayer.play(2);  // Toca o primeiro arquivo MP3 no cartão SD

    } if  (cardID == "83dbd9a6") { // UID do segundo cartão
      Serial.println("Cartão autorizado. Reproduzindo áudio 003.mp3...");

      // Reproduza o arquivo 002.mp3
      //playAudioFile(2);
      myDFPlayer.play(3);  // Toca o primeiro arquivo MP3 no cartão SD
    
    } if (cardID == "2d3508cf") { // UID do segundo cartão
      Serial.println("Cartão autorizado. Reproduzindo áudio 004.mp3...");

      // Reproduza o arquivo 002.mp3
      //playAudioFile(2);
      myDFPlayer.play(4);  // Toca o primeiro arquivo MP3 no cartão SD
    
    } if (cardID == "adcb06cf") { // UID do segundo cartão
      Serial.println("Cartão autorizado. Reproduzindo áudio 005.mp3...");

      // Reproduza o arquivo 002.mp3
      //playAudioFile(2);
      myDFPlayer.play(5);  // Toca o primeiro arquivo MP3 no cartão SD
    
    } else {
      Serial.println("Cartão não autorizado.");
    }
    delay(1000);
  }
}

void playAudioFile(uint16_t fileNumber) { // Função para reproduzir o arquivo MP3
  byte highByte = (fileNumber >> 8) & 0xFF; // Calcula o byte alto do arquivo
  byte lowByte = fileNumber & 0xFF; // Calcula o byte baixo do arquivo

  sendMP3Command(0x0F, highByte, lowByte); // Envia o comando para reproduzir o arquivo de áudio
}

void setVolume(byte volume) { // Função para definir o volume do MP3
  sendMP3Command(0x06, 0x00, volume);  // Comando para definir o volume
}
