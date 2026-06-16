# Projeto Semáforo inteligente

## <pre>Feito por: [Nicolas Ribeiro](https://github.com/NicolasRibeiro06 "Nicolas Ribeiro"), [Barbara Cardoso](https://github.com/Babicardoso "Barbara Cardoso"), <br>[Miguel caparroz](https://github.com/MiguelCaparrozNeves "Miguel Caparroz"), [Riquelme Reis](https://github.com/riquelmereis "Riquelme Reis"), [Gustavo Cruz](https://github.com/gustavocruz-11 "Gustavo cruz") e [Sarah Mendonça](https://github.com/SarahhMendonca "Sarah Mendonça").
# SPRINT 1 
## Nós, da SBRNG na Sprint 1, tivemos nossa ideia inicial de proposta, como vista na [documentação da Sprint 1.](https://github.com/NicolasRibeiro06/semaforo_Inteligente/blob/main/SPRINT1/SBRNG..pdf "SBRNG")  
## Proposta de Projeto: Semáforos Inteligentes
Neste documento, apresentamos nossa proposta para combater o grande congestionamento na região metropolitana de São Paulo por meio de 4 semáforos inteligentes. O projeto é focado no apoio a situações de emergência e, inicialmente, funcionaria através de sensores e câmeras inteligentes. O código utilizado na programação foi o C++, desenvolvido para a plataforma Arduino.

<hr>

# SPRINT 2 E 3
## Sprint 2: Mudança de Escopo e Planejamento
## Na Sprint 2, mudamos nossa ideia inicial para focar no apoio a casos de violência contra a mulher por meio do nosso projeto como podem ver no [Documento da Sprint 2](https://github.com/NicolasRibeiro06/semaforo_Inteligente/blob/main/SPRINT2_3/Projeto%20SBRNG.pdf "Documento"). Com o passar das aulas, compreendemos o funcionamento de ferramentas essenciais para o desenvolvimento, como o Arduino e o Tinkercad.hr

<hr>

## Além disso, estruturamos a lista de materiais necessária para a execução do projeto:
Sensor de câmera;
LEDs;
Resistores;
Fios (jumpers) para a instalação do circuito;
Arduino Uno.
<hr>

# Sprint 3: Montagem e Testes

## Na Sprint 3, começamos a montagem da maquete, que é a nossa representação física do projeto. Tivemos algumas dificuldades para ajustar os códigos no Arduino Uno, mas seguimos firmes com a nossa ideia principal. Após várias tentativas de simulação no Tinkercad e de testes no Arduino Uno, substituímos o Push Button pelo sensor ultrassônico por mal funcionamento e assim alcançamos o nosso objetivo.

### O projeto no Tinkercad resultou nisto:
<hr>

### Fotos do processo de montagem da maquete:
---
<img src="https://github.com/NicolasRibeiro06/semaforo_Inteligente/blob/main/assents/Maquete_1.jpg" alt="Maquete 1" />
  <br> <br> <br>
  <img src="https://github.com/NicolasRibeiro06/semaforo_Inteligente/blob/main/assents/Maquete_2.jpg" alt="Maquete 2" />
  <br> <br> <br>
  <img src="https://github.com/NicolasRibeiro06/semaforo_Inteligente/blob/main/assents/Maquete_4.jpg" alt="Maquete 3" />
  <br> <br> <br>
<hr>

### Código para o funcionamento do projeto:
// ==== Declarações ====

// Via 1 = Semáforo 1
int vermelho1 = 11;
int amarelo1 = 12;
int verde1 = 13;

// Via 2 = Semáforo 1
int verde2 = 8;
int amarelo2 = 9;
int vermelho2 = 10;

// Via 1 = Semáforo 2
int vermelho3 = 7;
int amarelo3 = 6;
int verde3 = 5;

// Via 2 = Semáforo 2
int verde4 = 4;
int amarelo4 = 3;
int vermelho4 = 2;

// Sensor HC-SR04
int trigPin = A1;
int echoPin = A0;

// Distância limite (em cm) para detectar movimento
int distanciaLimite = 30; 

void setup() {
  pinMode(verde1, OUTPUT); pinMode(amarelo1, OUTPUT); pinMode(vermelho1, OUTPUT);
  pinMode(verde2, OUTPUT); pinMode(amarelo2, OUTPUT); pinMode(vermelho2, OUTPUT);
  pinMode(verde3, OUTPUT); pinMode(amarelo3, OUTPUT); pinMode(vermelho3, OUTPUT);
  pinMode(verde4, OUTPUT); pinMode(amarelo4, OUTPUT); pinMode(vermelho4, OUTPUT);

  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  
  Serial.begin(9600); // Para você testar no Monitor Serial se quiser
}

// ==== Função que lê o sensor e diz se houve movimento (true ou false) ====
bool checarSensor() {
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  // pulseIn com timeout de 20000 microssegundos para o sensor não travar o código
  long duracao = pulseIn(echoPin, HIGH, 20000); 
  int distancia = duracao * 0.034 / 2;

  // Se a distância for válida e menor que o limite, detectou!
  if (distancia > 0 && distancia <= distanciaLimite) {
    return true; 
  }
  return false;
}

// ==== Função que substitui o delay() e avisa se o sensor foi acionado ====
bool esperarEChecar(unsigned long tempoDesejado) {
  unsigned long tempoInicial = millis();
  
  // Enquanto o tempo não passar...
  while (millis() - tempoInicial < tempoDesejado) {
    // Checa o sensor continuamente durante a espera
    if (checarSensor()) {
      return true; // Interrompe e avisa que detectou movimento!
    }
    delay(50); // Pequena pausa de estabilização
  }
  return false; // Passou o tempo sem nenhum movimento
}

// ==== Rotina de Emergência (O que acontece quando detecta movimento) ====
void rotinaEmergencia() {
  // Desliga todos os verdes e vermelhos
  digitalWrite(verde1, LOW);    digitalWrite(vermelho1, LOW);
  digitalWrite(verde3, LOW);    digitalWrite(vermelho3, LOW);
  digitalWrite(verde2, LOW);    digitalWrite(vermelho2, LOW);
  digitalWrite(verde4, LOW);    digitalWrite(vermelho4, LOW);
  
  // Pisca ou deixa os amarelos alertas por 4 segundos
  digitalWrite(amarelo1, HIGH);   digitalWrite(amarelo2, HIGH);
  digitalWrite(amarelo3, HIGH);   digitalWrite(amarelo4, HIGH);
  delay(4000); 
  
  // Apaga amarelos e fecha todos os semáforos (tudo vermelho)
  digitalWrite(amarelo1, LOW);    digitalWrite(amarelo2, LOW);
  digitalWrite(amarelo3, LOW);    digitalWrite(amarelo4, LOW);

  digitalWrite(vermelho1, HIGH);
  digitalWrite(vermelho2, HIGH);
  digitalWrite(vermelho3, HIGH);
  digitalWrite(vermelho4, HIGH);

  // Fica tudo vermelho por 4 segundos antes de reiniciar o ciclo normal
  delay(4000); 
}

void loop() {
  // --- PASSO 1: Via 1 aberta ---
  digitalWrite(verde1, HIGH);    digitalWrite(vermelho1, LOW);
  digitalWrite(verde3, HIGH);    digitalWrite(vermelho3, LOW);
  digitalWrite(verde2, LOW);     digitalWrite(vermelho2, HIGH);
  digitalWrite(verde4, LOW);     digitalWrite(vermelho4, HIGH);
  
  // Espera 5 segundos checando o sensor. Se detectar algo, vai para a emergência e reinicia o loop
  if (esperarEChecar(5000)) { rotinaEmergencia(); return; }

  // --- PASSO 2: Via 1 amarela ---
  digitalWrite(verde1, LOW);     digitalWrite(amarelo1, HIGH);
  digitalWrite(verde3, LOW);     digitalWrite(amarelo3, HIGH);
  
  if (esperarEChecar(2000)) { rotinaEmergencia(); return; }

  digitalWrite(amarelo1, LOW);   digitalWrite(vermelho1, HIGH);
  digitalWrite(amarelo3, LOW);   digitalWrite(vermelho3, HIGH);

  // --- PASSO 3: Via 2 aberta ---
  digitalWrite(vermelho2, LOW);  digitalWrite(verde2, HIGH);
  digitalWrite(vermelho4, LOW);  digitalWrite(verde4, HIGH);
  
  if (esperarEChecar(5000)) { rotinaEmergencia(); return; }

  // --- PASSO 4: Via 2 amarela ---
  digitalWrite(verde2, LOW);     digitalWrite(amarelo2, HIGH);
  digitalWrite(verde4, LOW);     digitalWrite(amarelo4, HIGH);
  
  if (esperarEChecar(2000)) { rotinaEmergencia(); return; }

  digitalWrite(amarelo2, LOW);   digitalWrite(vermelho2, HIGH);
  digitalWrite(amarelo4, LOW);   digitalWrite(vermelho4, HIGH);
} 
<hr>

### Materiais utilizados:
### Para a confecção da maquete:
<li> Papelões, caixa de leite; <li>
Isopor; <li>
Tinta guache, Tinta de tecido, pincel; <li>
EVAs, folha sulfite; <li>
Cola quente, cola liquída, cola bastão, superbonder; <li>
Tesouras sem pontas, estilete; <li>
Impresssora; <li>
Fita autocolante cinza; <li>
Mini bonecos; <li>
Papel Crepom; <li>
Biscuit, <li>
Caneta permanente; <li>
Top coat; <li>
Carrinhos hot weels; 

### Para a programação:
<li> Arduino Uno. 
Cabos jumpers. <li>
4 Módulo Led 8mm tipo Semáforo. <li>
Protoboard. <li>
Conector de Bateria. <li>
Bateria Elgin 9v. <li>
Cabo USB. <li>
Resistor.
Sensor Ultrassônico - HC-SR04;
 
<hr>
Conclusão
Conseguimos, portanto, concluir o desafio proposto pela ABC Technology e, assim, apresentar uma solução para a região de São Paulo. Durante o processo de confecção da maquete e do desenvolvimento dos códigos, adquirimos um conhecimento muito mais sólido sobre o Arduino e a linguagem utilizada (C++).
<hr>
