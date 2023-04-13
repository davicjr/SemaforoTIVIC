int status = 1;
int check = 0;
int ESTADO_BOTAO = 0; 
int VAR = 0;

void setup() {
  Serial.begin(9600);
  pinMode(13, OUTPUT); // Semáforo 1 - LED verde
  pinMode(12, OUTPUT); // Semáforo 1 - LED amarelo
  pinMode(11, OUTPUT); // Semáforo 1 - LED vermelho
  pinMode(10, OUTPUT); // Semáforo 2 - LED verde
  pinMode(9, OUTPUT);  // Semáforo 2 - LED amarelo
  pinMode(8, OUTPUT);  // Semáforo 2 - LED vermelho
  pinMode(7, OUTPUT);  // Semáforo Pedestre - LED verde
  pinMode(6, OUTPUT);  // Semáforo Pedestre - LED vermelho
  pinMode(4, OUTPUT);  // LED azul
  pinMode(2, INPUT); // define o pino do botão como entrada
}

void loop() {
  if(ESTADO_BOTAO==0){
  switch (status){

//FUNCIONAMENTO A1 (S.A - VERDE / S.B - VERMELHO / S.P - VERMELHO)
  case 1:
    faseA1();
    status=2;
    imprimirSerial();  
    delay(3000);
  break;
            
//FUNCIONAMENTO A2 (S.A - AMARELO / S.B - VERMELHO / S.P - VERMELHO)
  case 2:
    faseA2();
    status=3;
    imprimirSerial();
    delay(1000);
  break;

//FUNCIONAMENTO A1 (S.A - VERMELHO / S.B - VERDE / S.P - VERMELHO)
  case 3:
    faseB1();
    status=4;
    imprimirSerial();
    delay(3000);
  break;


//FUNCIONAMENTO A1 (S.A - VERMELHO / S.B - AMARELO / S.P - VERMELHO)
  case 4:
    faseB2();
    status=5;
    imprimirSerial();
    delay(1000);
  break;
         
//FUNCIONAMENTO A1 (S.A - VERMELHO / S.B - VERMELHO / S.P - VERDE)  
  case 5:
    faseP();
    status=1;
    imprimirSerial();
    delay(2000);
  break;
}
}
  if(ESTADO_BOTAO==1 && check==5){
    faseA1();
    status=2;
    imprimirSerial();
    delay(3000);
  }
   if(ESTADO_BOTAO==1 && check==1){
    faseA2();
    status=2;
    imprimirSerial();
    delay(1000);
     
    status=5;
    imprimirSerial();
    faseP();
    delay(2000);
     
    status=3;
    check=3;
    ESTADO_BOTAO=0;
    digitalWrite(4, LOW);    

  }
   if(ESTADO_BOTAO==1 && check==2){  
    status=5;
    imprimirSerial();
    faseP();
    delay(2000);
     
    status=4;
    check=4;
    ESTADO_BOTAO=0;
    digitalWrite(4, LOW); 
  }
   if(ESTADO_BOTAO==1 && check==3){
    faseB2();
    status=5;
    ESTADO_BOTAO=0;
    digitalWrite(4, LOW);
    imprimirSerial(); 
    delay(1000);
   
  }
   if(ESTADO_BOTAO==1 && check==4){
    faseP();
    status=1;
    imprimirSerial();
    delay(3000);
  }
  
}

  void faseA1(){
    verificarBotao();
 	setPins(HIGH, LOW, LOW, LOW, LOW, HIGH, LOW, HIGH);
    check=status;
  }
   void faseA2(){
    verificarBotao();
  	setPins(LOW, HIGH, LOW, LOW, LOW, HIGH, LOW, HIGH);
    check=status;
  }
   void faseB1(){
    verificarBotao();
  	setPins(LOW, LOW, HIGH, HIGH, LOW, LOW, LOW, HIGH);
    check=status;
  }
   void faseB2(){
     verificarBotao();
 	 setPins(LOW, LOW, HIGH, LOW, HIGH, LOW, LOW, HIGH);
     check=status;
  }
   void faseP(){
    verificarBotao();
  	setPins(LOW, LOW, HIGH, LOW, LOW, HIGH, HIGH, LOW);
    check=status;
 }
  
void setPins(int pin13, int pin12, int pin11, int pin10,int pin9, int pin8, int pin7, int pin6) {
  digitalWrite(13, pin13);
  digitalWrite(12, pin12);
  digitalWrite(11, pin11);
  digitalWrite(10, pin10);
  digitalWrite(9, pin9);
  digitalWrite(8, pin8);
  digitalWrite(7, pin7);
  digitalWrite(6, pin6);
}

void verificarBotao(){
  
  VAR = digitalRead(2);
   
 if(VAR == 1){
 digitalWrite(4, HIGH);//Acende o led conectado ao pino 4
 ESTADO_BOTAO = 1;
 }
  else{
 digitalWrite(4, LOW);//Apaga o led conectado ao pino 4
  }
}
void imprimirSerial() {
  if (ESTADO_BOTAO == 1) { 
   Serial.print("Botao Pressionado! Aguarde! Fase Atual:");
   Serial.print(check);
    Serial.print(" Proxima Fase:");
   Serial.println(status);
    delay(500);
  } else {
   Serial.print("Fase Atual:");
   Serial.print(check);
    Serial.print(" Proxima Fase:");
   Serial.println(status);
    delay(500);

  }
}

