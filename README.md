# Repositorio de Juan González Cardesín
**Este repositorio le pertenece al alumno Juan González Cardesín
De 5to año azul de U.E.P. Colegio Jefferson. En este repositorio
Veras los proyectos y actividades que se realizaran en el primer Lapso**
Cuenta de tinkercard: juanig09
Pensamiento Computacional
Clase del 24/09 creación del GitHub.

**Clase del 1/10, actividad 1.**

En esta actividad vimos una tabla y respondimos los siguientes ejercicios 
<img width="608" height="508" alt="IMG_3097" src="https://github.com/user-attachments/assets/9e749904-083b-4d52-9eae-b0a239144546" />
En una arepera de Maracaibo, el dueño quiere automatizar los precios. Observa esta tabla de precios según la cantidad de arepas:

Actividad:
	1.	Encuentra el patrón que sigue el precio. ¿Cuánto aumenta por cada arepa adicional?
	2.	Escribe una fórmula matemática que relacione la cantidad de arepas (n) con el precio total (P).
	3.	Usa la fórmula para predecir el precio de 10 arepas.
	4.	Abstrae: si el precio de la primera arepa es “a” y el descuento por volumen sigue la misma lógica, escribe una fórmula general para cualquier cantidad n.
	5.	Reflexiona: ¿Por qué el precio no aumenta siempre lo mismo? ¿Qué pasaría si el patrón cambiara?

  La siguiente hoja tendrá las respuestas
<img width="3024" height="4032" alt="IMG_3095" src="https://github.com/user-attachments/assets/24c46b79-5f6f-4268-a5f5-92a566cdb459" />
<img width="3024" height="4032" alt="IMG_3096" src="https://github.com/user-attachments/assets/e03e1428-f23b-446b-9ded-2fd2f694c40f" />
**Actividad#2, clase de 8/10**

En esta clase hicimos dos ejercicios, un circuito, y otro ejercicio de calculo.

Ejercicio de circuito: 

<img width="1843" height="1307" alt="IMG_3128" src="https://github.com/user-attachments/assets/59f142a9-0727-48ba-835b-03f8cafdf53e" />

Link al circuito: https://www.tinkercad.com/things/ifSqxTSkzpZ-swanky-kasi/editel?returnTo=https%3A%2F%2Fwww.tinkercad.com%2Fdashboard&sharecode=_Hs1MfK65OGmmK2iutbWZBcVA8JBGEb2T6z4RiywKOo

Código:
// ----- Parte 1: 6 LEDs con pulsador -----
const int boton = 11;
const int leds[] = {2, 3, 4, 5, 6, 9};
const int numLeds = 6;
const unsigned long intervaloLeds = 8000; // 8 segundos

bool activado = false;
bool estadoLeds = LOW;
unsigned long ultimoCambioLeds = 0;

// ----- Parte 2: 2 LEDs con interruptor -----
const int interruptor = 12;
const int ledAzul = 10;
const int ledVerde = 13;
const unsigned long tiempoAlternar = 500; // medio segundo

bool turnoAzul = false;
unsigned long ultimoCambioAlternar = 0;

void setup() {
  pinMode(boton, INPUT_PULLUP);
  pinMode(interruptor, INPUT_PULLUP);
  for (int i = 0; i < numLeds; i++) {
    pinMode(leds[i], OUTPUT);
    digitalWrite(leds[i], LOW);
  }
  pinMode(ledAzul, OUTPUT);
  pinMode(ledVerde, OUTPUT);
}

void ponerLeds(bool estado) {
  for (int i = 0; i < numLeds; i++) {
    digitalWrite(leds[i], estado);
  }
}

void loop() {
  unsigned long ahora = millis();

  // ----- Parte 1 -----
  if (!activado) {
    if (digitalRead(boton) == LOW) {
      activado = true;
      estadoLeds = HIGH;
      ponerLeds(estadoLeds);
      ultimoCambioLeds = ahora;
    }
  } else if (ahora - ultimoCambioLeds >= intervaloLeds) {
    estadoLeds = !estadoLeds;
    ponerLeds(estadoLeds);
    ultimoCambioLeds = ahora;
  }

  // ----- Parte 2 -----
  if (digitalRead(interruptor) == LOW) {
    if (ahora - ultimoCambioAlternar >= tiempoAlternar) {
      turnoAzul = !turnoAzul;
      digitalWrite(ledAzul, turnoAzul);
      digitalWrite(ledVerde, !turnoAzul);
      ultimoCambioAlternar = ahora;
    }
  } else {
    digitalWrite(ledAzul, LOW);
    digitalWrite(ledVerde, LOW);
  }
}
Actividad desenchufada:
<img width="865" height="1242" alt="IMG_3134" src="https://github.com/user-attachments/assets/50247c34-83a7-41bb-b894-66a598493e34" />

