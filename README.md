# Objetos_Inteligentes
## PROJETO: Aplicações Assistivas com Arduino: Criando um Manipulador Robótico para Cadeirantes

### i) Descrição do Projeto
Este repositório contém um projeto de manipulador robótico controlado por Arduino, projetado para auxiliar pessoas com mobilidade reduzida. O protótipo utiliza um Arduino Uno para controlar sensores e atuadores, permitindo a execução de tarefas assistivas. A comunicação e controle do dispositivo são realizados via internet usando o protocolo MQTT, com uma interface intuitiva fornecida pelo Node-RED.

### ii) Software e Documentação de Código
O software desenvolvido para este projeto inclui scripts em Arduino e fluxos Node-RED. A documentação detalhada do código está disponível, explicando cada função e sua finalidade, facilitando a compreensão e a reprodução do projeto.

~~~
#include <VarSpeedServo.h>
  
VarSpeedServo servo_x; // Cria objeto para controlar o servo que controla o eixo x (direta e esquerda)
VarSpeedServo servo_y; // Cria objeto para controlar o servo que controla o eixo y (para frente e para trás)
VarSpeedServo servo_z; // Cria objeto para controlar o servo que controla o eixo z (sobe e desce)
VarSpeedServo servo_w; // Cria objeto para controlar o servo que controla o eixo w (abre e fecha)
 
int js_x = A0; // Define que o joystick que controla o eixo x está conectado na porta A0 do Arduino
int js_y = A1; // Define que o joystick que controla o eixo y está conectado na porta A1 do Arduino
int js_z = A3; // Define que o joystick que controla o eixo z está conectado na porta A3 do Arduino
int js_w = A4; // Define que o joystick que controla o eixo w está conectado na porta A4 do Arduino
 
int s_x = 3;  // Define que o servo do eixo x está conectado na porta 3 do Arduino
int s_y = 5;  // Define que o servo do eixo y está conectado na porta 5 do Arduino
int s_z = 10; // Define que o servo do eixo x está conectado na porta 10 do Arduino
int s_w = 11; // Define que o servo do eixo w está conectado na porta 11 do Arduino
 
int val_x = 90; //Armazena o valor lido pelo eixo x do joystick
int val_y = 90; //Armazena o valor lido pelo eixo y do joystick
int val_z = 90; //Armazena o valor lido pelo eixo z do joystick
int val_w = 90; //Armazena o valor lido pelo eixo w do joystick
  
void setup() {
 Serial.begin(9600);
 servo_x.attach(s_x, 1, 180); // Inicializa o servo do eixo x
 servo_y.attach(s_y, 1, 180); // Inicializa o servo do eixo y
 servo_z.attach(s_z, 1, 180); // Inicializa o servo do eixo z
 servo_w.attach(s_w, 1, 180); // Inicializa o servo do eixo w
}
  
void loop() {
 
 if (analogRead(js_x) > 970){       // Verifica se o joystick do eixo x aponta para a direita
  val_x++;
  if (val_x > 180){val_x = 180;}
  servo_x.write(val_x, 120, true);  // Move o servo do eixo x para a direita
 }
 
 if (analogRead(js_x) < 50){        // Verifica se o joystick do eixo x aponta para a esquerda
  val_x--;
  if (val_x < 0){val_x = 0;}
  servo_x.write(val_x, 120, true);  // Move o servo do eixo x para a esquerda
 }
 
 if (analogRead(js_y) < 50){       // Verifica se o joystick do eixo y aponta para cima
  val_y++;
  if (val_y > 180){val_y = 180;}
  servo_y.write(val_y, 120, true); // Move o servo do eixo y para frente
 }
 
 if (analogRead(js_y) > 970){      // Verifica se o joystick do eixo y aponta para baixo
  val_y--;
  if (val_y < 0){val_y = 0;}
  servo_y.write(val_y, 120, true); // Move o servo do eixo y para trás
 }
 
 if (analogRead(js_z) < 50){       // Verifica se o joystick do eixo z aponta para baixo
  val_z++;
  if (val_z > 180){val_z = 180;}
  servo_z.write(val_z, 120, true); // Move o servo do eixo z para cima
 }
 
 if (analogRead(js_z) > 970){      // Verifica se o joystick do eixo z aponta para cima
  val_z--;
  if (val_z < 0){val_z = 0;}
  servo_z.write(val_z, 120, true);  // Move o servo do eixo z para baixo
 }
 
 if (analogRead(js_w) < 50){        // Verifica se o joystick do eixo w aponta para a direita 
  val_w++;
  if (val_w > 180){val_w = 180;}
  servo_w.write(val_w, 120, true);  // Abre o servo do eixo w
 }
 
 if (analogRead(js_w) > 970){       // Verifica se o joystick do eixo w aponta para a esquerda
  val_w--;
  if (val_w < 0){val_w = 0;}
  servo_w.write(val_w, 120, true);  // Fecha o servo do eixo w
 }
  
 delay(30);
}
~~~

### iii) Descrição do Hardware
O hardware utilizado neste projeto inclui:

* Placa Uno R3 + Cabo USB para Arduino
* Sensor Shield V5.0 para Arduino
* Joystick Arduino 3 Eixos
* Micro Servo 9g SG90 TowerPro
* Fonte DC Chaveada 9V 1A Plug P4
* Jumpers Fêmea-Fêmea e Jumpers Macho-Fêmea
* Braço Robótico em MDF

### iv) Documentação de Interfaces e Protocolos

**Interfaces de Comunicação**

Serial entre Arduino e PC: O Arduino Uno se comunica com o PC através de uma conexão serial. Esta conexão é essencial para transmitir dados dos sensores e receber comandos de controle. Utiliza-se a biblioteca padrão do Arduino para comunicação serial.

Node-RED para Interface Gráfica: O Node-RED, rodando no PC, fornece uma interface gráfica de usuário (GUI) acessível via navegador web. Esta GUI permite o controle interativo do manipulador robótico, exibindo informações em tempo real e permitindo o envio de comandos.

**Protocolo MQTT**

Configuração do Broker MQTT (Mosquitto): O broker Mosquitto é configurado no PC como um servidor central para gerenciar a comunicação MQTT. O Arduino, equipado com um módulo de comunicação (como um ESP8266 para Wi-Fi), publica dados para o broker, enquanto o Node-RED se inscreve nesse broker para receber atualizações.

Tópicos MQTT: Os tópicos MQTT são estruturados para organizar a comunicação. Por exemplo, pode haver tópicos separados para 'sensor/data' para dados dos sensores e 'manipulator/control' para comandos de controle do manipulador.

Segurança e Autenticação: A segurança na comunicação MQTT é assegurada através de autenticação (usuário e senha) e, idealmente, criptografia TLS/SSL para proteger os dados transmitidos.

**Módulos de Comunicação**

Arduino e MQTT: O Arduino publica dados para o broker MQTT usando uma biblioteca MQTT, como a PubSubClient para Arduino. Esta biblioteca permite ao Arduino conectar-se à rede Wi-Fi e enviar dados para o broker MQTT.

Node-RED e MQTT: No Node-RED, são utilizados nós de 'mqtt in' e 'mqtt out' para se inscrever e publicar em tópicos MQTT, respectivamente. Isso permite que o Node-RED receba dados do Arduino e envie comandos de volta.

Integração com o Broker Mosquitto: O broker Mosquitto é configurado para aceitar conexões do Arduino e do Node-RED. A configuração envolve definir as portas de comunicação, autenticação de usuários e, se necessário, configurações de criptografia.

### v) Comunicação e Controle via Internet
O projeto utiliza TCP/IP para comunicação e controle remoto. O protocolo MQTT é empregado para a troca eficiente de mensagens, com o broker Mosquitto atuando como intermediário. O Node-RED facilita a criação de uma interface de usuário para controle e monitoramento remotos.



Segue link de vídeo explicativo e com demonstração de funcionamento do projeto: <https://youtu.be/n3MrTmjxgf4?si=cQ-WG16vzuveoZbe>
