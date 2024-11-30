# Dispositivo de Treinamento de Sensação Tátil com Feedback em Tempo Real

**Descrição:** Neste projeto, vamos aprender a construir o dispositivo acima utilizando o Arduino Mega. Tal projeto envisa auxiliar no processo de reabilitação motora e sensorial de pacientes. Além disso, o mesmo é ideal para quem deseja aprender a combinar sensores e atuadores em aplicações práticas. Ao final do tutorial, você terá adquirido as seguintes habilidades:
- Configurar sensores analógicos (por exemplo, flex sensor) e atuadores (como motores vibratórios) para monitorar e responder a estímulos;
- Integrar e programar um idsplay LCD para forncecer feedback visual ao usuário;
- Implementar controles remotos infravermelhos para criar interfaces de interação acessíveis.

Este tutorial é voltado para **estudantes de eletrônica**, **profissionais de reabilitação** e **entusiastas de tecnologia**, permitindo aplicar conhecimentos de eletrônica e programação em um contexto prático e significativo.

---

## Índice

1. [Introdução](#introdução)
2. [Requisitos](#requisitos)
3. [Configuração do Ambiente](#configuração-do-ambiente)
4. [Montagem do Circuito](#montagem-do-circuito)
5. [Programação](#programação)
6. [Teste e Validação](#teste-e-validação)
7. [Expansões e Melhorias](#expansões-e-melhorias)
8. [Referências](#referências)

---

## Introdução

O dispositivo em questão é uma solução prática e acessível para apoiar pacientes na reabilitação de habilidades motoras finas e sensibilidade tátil, especialmente em casos de lesões, cirurgias ou doenças neurológicas. Por meio de um sensor flexível, o mecanismo mede o movimento de flexão dos dedos, enquanto um motor vibratório oferece feedback tátil para reforçar o progresso do usuário. Um display LCD exibe mensagens motivacionais e informações em tempo real, promovendo engajamento e autonomia durante o processo de reabilitação. Este projeto se integra ao mundo IoT ao trazer a possibilidade de conectividade com maior eficiência ao processo terapêutico e médico, permitindo que ambos possam acompanhar o avanço da reabilitação em tempo real, reduzindo a necessidade de acompanhamento presencial e proporcionando uma experiência mais personalizada e contínua.

---

## Requisitos

### Hardware

- **Placa**: Arduino Mega ou Mega 2560.
- **Sensores**: Sensor flexível (Flex Sensor) e Receptor IR (Infravermelho).
- **Atuadores**: Motor vibratório e Display LCD 16x2.
- **Outros componentes**: Resistores (1kΩ e 220Ω), uma protoboard e um controle remoto infravermelho.

### Software

- **Linguagens**: C
- **IDE**: Arduino IDE, Tinkercad(opcional)
- **Bibliotecas**: `LiquidCrystal`(para controle do display LCD) e `IRremote`(para controle remoto IR -infravermelho).

---

## Configuração do Ambiente

### Passo 1: Instalação do Software

- **Arduino IDE**: [Baixe](https://www.arduino.cc/en/software) e instale a IDE do Arduino para programar o Arduino Uno.
- **Bibliotecas**: A biblioteca `LiquidCrystal` e `IRremote` podem ser instaladas como o exemplo mostra abaixo.

1. Procure pelo seguinte ícone, após a instalação da IDE:
![Caminho para a Biblioteca](../Sistema-Alerta/Caminho-Biblioteca.png)
2. Na área de texto escreva `LiquidCrystal` e escolha a opção "by Arduino".
3. Analogamente, escreva `IRremote` e escolha a opção "by shirriff, z3t0, ArminJo".

### Passo 2: Configuração das Placas

- Conecte o Arduino Mega ao computador usando um cabo USB.
- Selecione sua _board_ na opção a seguir:
![Caminho para a Selecionar Board](../Sistema-Alerta/Caminho-Selecionar-Port.png)
1. Clique em _Select other board and port_;
2. Selecione a _board_ **Arduino Mega or Mega 2560**;
3. Por fim, selecione a _Port_ de sua preferência.

---

## Montagem do Circuito

- Visualização do circuito completo. Para acessá-lo, [clique aqui](https://www.tinkercad.com/things/94V5gX0DVnz-tactile-sensation-trainer-disp-de-treinamento-tatil?sharecode=-OK07sNZnsPHU7p-FayuRfy9U6VkTNLpAw3AULmx_Pw)
![Circuito completo do dispositivo de treinamento de sensação tátil com feedback](circuito-de-treinamento-de-sensacao-tatil.png)
>[!NOTE]
>Se é sua primeira vez usando um LCD, é bem capaz que você tenha que soldar pinos nele para conectá-lo a _protoboard_ e, enfim, seguir para as Conexões do Circuito.

### Conexões do Circuito:
1. Sensor Flexível:
   - Um terminal no pino A0, conecte-o como o cabo verde demonstra na figura;
   - Conecte o outro terminal ao 5V(+) da _protoboard_;
   - Abaixo da conexão do terminal no pino A0, insira um resistor de 1kΩ no GND(-) da _protoboard_.
2. Motor Vibratório:
   - Pino positivo ao pino digital 6 e negativo ao GND(-) da _protoboard_;
3. Receptor IR:
   - Pino de dados(OUT) ao pino 2, Power ao 5V(+) da _protoboard_, GND ao GND(-) da _protoboard_.
5. LCD 16x2 — Começando da Esquerda para a Direita:
   - GND do LCD para GND(-) da _protoboard_;
   - VCC do LCD para 5V(+) da _protoboard_;
   - VO para o GND(-) da _protoboard_, isso fará que o contraste do LCD esteja no máximo;
   - RS do LCD para o pino 7 do Arduino;
   - RW do LCD para o GND(-) da _protoboard_;
   - E (Enable/Ligado) do LCD para o pino 8 do Arduino;
   - DB0 até DB3 não serão usados, por isso pule eles;
   - DB4, DB5, DB6 e DB7 do LCD para os pinos 9, 10, 11 e 12 do Arduino, respectivamente;
   - LED (Backlight/Luz de Fundo) do LCD para um resistor de 220Ω e conecte esse resistor ao 5V(+) da _protoboard_;
   - O último LED, você pode conectá-lo ao GND(-) da _protoboard_.
>[!WARNING]
>Certifique-se de que todas as conexões estão firmes e corretas antes de prosseguir com a programação. Em caso de dúvida, refaça esta etapa consultando diretamente a simulação, link acima.

---

## Programação

### Passo 1: Configuração dos Sensores e Atuadores

```cpp
#include <LiquidCrystal.h>
#include <IRremote.h>

const int flexPin = A0;             
const int vibrationMotorPin = 6;    
const int receiverPin = 2;          

LiquidCrystal lcd(7, 8, 9, 10, 11, 12);

int flexValue = 0;                  
int mappedValue = 0;                
int threshold = 700;                
bool trainingStarted = false;       
int button = 0;                     

int mapCodeToButton(unsigned long code) {
  if ((code & 0x0000FFFF) == 0x0000BF00) {
    code >>= 16;
    if (((code >> 8) ^ (code & 0x00FF)) == 0x00FF) {
      return code & 0xFF;
    }
  }
  return -1;
}

int readInfrared() {
  int result = -1;
  if (IrReceiver.decode()) {
    unsigned long code = IrReceiver.decodedIRData.decodedRawData;
    result = mapCodeToButton(code);
    IrReceiver.resume();
  }
  return result;
}

void setup() {
  pinMode(flexPin, INPUT);
  pinMode(vibrationMotorPin, OUTPUT);

  lcd.begin(16, 2);
  lcd.print("Treino Tatil");
  lcd.setCursor(0, 1);
  lcd.print("Aperte Comecar");
  
  IrReceiver.begin(receiverPin);
  Serial.begin(9600);
}
```

### Passo 2: Processamento e Lógica de Alerta

```cpp
void loop() {
  button = readInfrared();
  if (button >= 0) {
    handleIRInput(button);
  }

  if (trainingStarted) {
    flexValue = analogRead(flexPin);
    mappedValue = map(flexValue, 33, 6, 0, 100); 
    mappedValue = constrain(mappedValue, 0, 100);         
	
    lcd.setCursor(0, 0);
    lcd.print("Progresso:       ");
    lcd.setCursor(0, 0);
    lcd.print("Progresso: ");
    lcd.print(mappedValue);
    lcd.print("%");

    lcd.setCursor(0, 1);
    if (mappedValue < 50) {
      lcd.print("Continue!     ");
    } else if (mappedValue < 90) {
      lcd.print("Quase la!");
    } else {
      lcd.print("Bom Trabalho!       ");
    }

    if (mappedValue >= 90) {
      digitalWrite(vibrationMotorPin, HIGH);
    } else {
      digitalWrite(vibrationMotorPin, LOW);
    }
  }
}

void handleIRInput(int button) {
  switch (button) {
    case 0: 
      trainingStarted = true;
      lcd.clear();
      lcd.print("Comecando Treino");
      delay(1000);
      lcd.clear();
      break;
    case 5: 
      trainingStarted = false;
      digitalWrite(vibrationMotorPin, LOW);
      lcd.clear();
      lcd.print("Treino parado");
      lcd.setCursor(0, 1);
      lcd.print("Aperte Comecar");
      break;
    default:
      break;
  }
}
```

---

## Teste e Validação

Descreva os testes para validar cada parte do projeto:

1. **Testar o Sensor Flexível**: Abra o monitor serial e verifique os valores do sensor quando dobrado e relaxado..
2. **Testar o Motor Vibratório**: Confirme que o motor vibra quando o progresso atinge 90%.
3. **Validar o Controle Remoto**: Teste os botões para iniciar e parar o treinamento.
4. **Monitoramento Contínuo**: Verifique se as messagens "Comecando Treino" e "Treino parado" estão aparecendo no display. Dobre o sensor e veja as mensagens "Continue!", "Quase la!" e "Bom Trabalho!  aparecerem. Continue testando para garantir que o sistema responda de acordo com o limite configurado, logo confirme se a mensagem "Bom Trabalho!" aparece somente quando for 90% ou maior.

---

## Expansões e Melhorias

Sugestões para melhorar o projeto, como:

- Adicionar conectividade Wi-Fi para enviar dados a um aplicativo.
- Implementar armazenamento de progresso em um cartão SD.
- Criar um aplicativo para visualizar as métricas de desempenho.

---

## Referências

1. [Documentação da Biblioteca LiquidCrystal](https://docs.arduino.cc/libraries/liquidcrystal/)
2. [Documentação da Biblioteca IRremote](https://github.com/Arduino-IRremote/Arduino-IRremote)
3. [Tinkercad - Circuitos e Simulação de Arduino](https://www.tinkercad.com/)

---

