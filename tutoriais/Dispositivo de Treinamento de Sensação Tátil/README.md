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

Forneça o código para a configuração dos sensores. Por exemplo, para medir temperatura e batimentos cardíacos:

**Exemplo em C para ESP32:**

```cpp
#include <DHT.h>

#define DHTPIN 2     // Pino do sensor DHT
#define DHTTYPE DHT11 

DHT dht(DHTPIN, DHTTYPE);

void setup() {
  Serial.begin(9600);
  dht.begin();
}

void loop() {
  float temp = dht.readTemperature();
  Serial.println(temp);
  delay(2000);
}
```

**Exemplo em Python para Raspberry Pi:**

```python
import Adafruit_DHT

sensor = Adafruit_DHT.DHT11
pin = 4  # Pino GPIO

humidity, temperature = Adafruit_DHT.read_retry(sensor, pin)
print(f"Temperatura: {temperature}ºC")
```

### Passo 2: Processamento e Lógica de Alerta

Adicione a lógica para processar os dados e acionar atuadores, como LEDs ou buzzer, caso as leituras excedam um determinado limite.

---

## Teste e Validação

Descreva os testes para validar cada parte do projeto:

1. **Testando Sensores**: Verifique se as leituras são consistentes e corretas.
2. **Validação dos Atuadores**: Confirme que os atuadores funcionam corretamente.
3. **Monitoramento em Tempo Real**: Teste o sistema completo em condições simuladas para garantir que funciona conforme o esperado.

---

## Expansões e Melhorias

Sugestões para melhorar o projeto, como:

- Adicionar comunicação Wi-Fi (ESP32) para enviar dados para uma nuvem.
- Integrar um banco de dados para registro das leituras.
- Conectar-se a uma aplicação móvel para visualização remota.

---

## Referências

Liste todas as referências e links úteis para guias, bibliotecas, e materiais adicionais que ajudem a complementar o tutorial.

---

Espero que esse modelo ajude a organizar o conteúdo e fornecer uma estrutura clara e completa para tutoriais de IoT no contexto da saúde.
