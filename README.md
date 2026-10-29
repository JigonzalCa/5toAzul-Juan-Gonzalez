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

**Actividad#3, 15/10**

El link del circuito es  https://www.tinkercad.com/things/3dJht8wvH7m-exquisite-hango-tumelo/editel?returnTo=https%3A%2F%2Fwww.tinkercad.com%2Fdashboard&sharecode=7Kh-AW25yAqbSHBZRszd7RcoaP6D0GY_i698ufVAUWs

En esta actividad que consiste en crear un producto para solucionar un problema, yo y Samuel Gil realizamos el siguiente circuito. 

Problema principal: Llamada de turnos en una cola. 
El problema es que en lugares con mucha cola como en una panadería o una farmacia, mucha gente no sabe cuando lo llaman, en que turno le tocan, mucha gente se colea con intención o sin intención, y que el encargado tenga que gritar sus nombres. Esto genera problemas como mucho desorden y caos.

La solución a esto es el siguiente dispositivo que va ayudar a mantener el orden, va ayudar a que la gente  sepa cual es su turno en la cola, va ahorrar malentendidos, y no hay interrupciones.

El funcionamiento del dispositivo es que el encargado presiona un botón al terminar con un cliente. El sistema suma un turno, lo muestra en el monitor serial ("Turno 5, pase por favor") y hace parpadear un LED para llamar la atención.

 El código del programa es: 

const int boton = 2;
const int led = 13;
int turno = 0;

void setup() {
  pinMode(boton, INPUT_PULLUP);
  pinMode(led, OUTPUT);
  Serial.begin(9600);
  Serial.println("Sistema de turnos listo");
}

void loop() {
  if (digitalRead(boton) == LOW) {      // entrada: botón presionado
    turno++;                            // proceso: siguiente turno
    Serial.print("Turno ");
    Serial.print(turno);
    Serial.println(", pase por favor");
    for (int i = 0; i < 3; i++) {       // respuesta: LED parpadea
      digitalWrite(led, HIGH);
      delay(200);
      digitalWrite(led, LOW);
      delay(200);
    }
    while (digitalRead(boton) == LOW);  // espera a que suelten el botón
  }
}

<img width="1459" height="988" alt="IMG_3154" src="https://github.com/user-attachments/assets/b457aced-a8f3-4275-891b-ed3b03c21e55" />
<img width="2360" height="1640" alt="IMG_3156" src="https://github.com/user-attachments/assets/61da1c1f-ff39-4d35-921c-feb6890a4223" />

**Actividad del 22/10**
Puedes ver el PDF para ver completo todo


---

## Tarea #1

### Actividad 1

**La llegada de estudiantes:** Los estudiantes llegan al centro en momentos impredecibles y sin un orden fijo. El sistema debe detectar cada llegada y anotar la hora exacta, porque de ella depende el orden de atención y el cálculo del tiempo de espera.

**El registro:** Al llegar, cada estudiante se identifica e indica qué trámite necesita: entrega de documentos, solicitud de constancia o consulta general. También se verifica que no haya pasado la hora de cierre y que traiga los documentos completos. Si todo está en orden, recibe un número de turno.

**Las prioridades:** En este paso se decide si el estudiante es un caso prioritario, por ejemplo, por discapacidad, embarazo o salud. Los prioritarios entran a la cola prioritaria y los demás a la cola normal. Ambas colas respetan el orden de llegada.

**La atención:** Cada vez que la ventanilla queda libre, se elige al siguiente estudiante según las reglas de prioridad, sin que nadie espere indefinidamente. Luego se le llama, se atiende su trámite y se registran la hora de inicio y la hora de fin. Si no se presenta a tiempo, se marca como ausente.

**El cierre:** Al llegar la hora de cierre ya no se registran nuevos estudiantes, pero se atiende a todos los que estaban en fila. Al final se calcula el tiempo promedio de espera y se muestran los resultados, para evaluar si el sistema realmente redujo la espera.

### Actividad 2

Para definir cada dato nosotros vamos a hacer una tabla de datos que explicará todos los datos de los estudiantes:

| Dato | Tipo de dato | Para qué sirve | Ejemplo |
|---|---|---|---|
| numeroTurno | Entero | Identificar y llamar al estudiante en orden | 15 |
| codigoEstudiante | Texto | Identificar al estudiante (cédula o código) | V-30123456 |
| nombre | Texto | Llamarlo y dejar registro de la atención | Samuel Gil |
| tipoTramite | Lista (ENTREGA, CONSTANCIA, CONSULTA) | Saber qué trámite hará y estimar su duración | CONSTANCIA |
| tiempoEstimado | Entero (minutos) | Calcular el tiempo aproximado de espera | 10 |
| esPrioritario | Lógico (Verdadero/Falso) | Decidir si entra a la cola prioritaria o a la normal | Falso |
| motivoPrioridad | Texto | Justificar por qué es un caso prioritario | Ninguno |
| horaLlegada | Hora | Respetar el orden de llegada y medir la espera | 08:15 |
| documentosCompletos | Lógico (Verdadero/Falso) | Verificar en el registro que trae todo lo necesario | Verdadero |
| horaInicioAtencion | Hora | Calcular el tiempo de espera (inicio − llegada) | 08:40 |
| horaFinAtencion | Hora | Calcular cuánto duró realmente el trámite | 08:50 |
| estado | Lista (EN_ESPERA, EN_ATENCION, ATENDIDO, AUSENTE) | Hacer seguimiento de la situación del estudiante | ATENDIDO |

### Actividad 3

En esta actividad nosotros realizamos un diagrama de flujo para responder la actividad.

![Diagrama de flujo de la atención en ventanilla](Diagrama_de_flujo_atencion.png)

### Actividad 4

Usaría dos colas, una prioritaria y una normal, y en cada una se atiende por orden de llegada. Son prioritarios los estudiantes con discapacidad, las embarazadas, quienes tienen alguna condición de salud y quienes tienen un plazo que vence ese día. Elegí estos criterios porque son objetivos y verificables, no dependen de la opinión del personal.

No usé el método de atender primero los trámites más cortos porque, aunque reduce la espera promedio, rompe el orden de llegada y podría dejar a alguien esperando indefinidamente.

Para que el sistema sea justo, después de atender a dos prioritarios seguidos se atiende a uno normal, y si un estudiante normal lleva más de 30 minutos esperando, pasa de inmediato.

La espera se reduce sin alterar el orden: se verifican los documentos en el registro, se marca como ausente a quien no se presenta en tres minutos y se registra cuánto tarda cada trámite. Así se equilibran la equidad y la eficiencia.

---

## Tarea #2

### La optimización de la ruta del autobús escolar

#### 1. Representación de paradas y distancias

**Cantidad de estudiantes por parada:**

| Parada | Estudiantes |
|---|---|
| P1 | 4 |
| P2 | 3 |
| P3 | 6 |
| P4 | 2 |
| P5 | 5 |
| P6 | 3 |

**Matriz de distancias (en kilómetros):**

| Parada | P1 | P2 | P3 | P4 | P5 | P6 |
|---|---|---|---|---|---|---|
| **P1** | 0 | 2 | 5 | 7 | 10 | 3 |
| **P2** | 2 | 0 | 3 | 6 | 9 | 4 |
| **P3** | 5 | 3 | 0 | 4 | 7 | 6 |
| **P4** | 7 | 6 | 4 | 0 | 3 | 8 |
| **P5** | 10 | 9 | 7 | 3 | 0 | 11 |
| **P6** | 3 | 4 | 6 | 8 | 11 | 0 |

#### 2. Algoritmo paso a paso (algoritmo del vecino más cercano)

1. **Paso 1:** Definir el punto de partida (por ejemplo, la parada P1) y marcarla como visitada.
2. **Paso 2:** Desde la parada actual, revisar las paradas no visitadas y seleccionar aquella que se encuentre a la menor distancia.
3. **Paso 3:** Desplazarse a esa parada, registrarla como visitada y sumar los estudiantes a bordo.
4. **Paso 4:** Repetir los pasos 2 y 3 hasta que todas las paradas hayan sido visitadas.

#### 3. Aplicación del algoritmo y ruta propuesta

Partiendo de P1:

- Desde P1, la más cercana es P2 (2 km). (Estudiantes acumulados: 4 + 3 = 7)
- Desde P2, la más cercana no visitada es P3 (3 km). (Estudiantes acumulados: 7 + 6 = 13)
- Desde P3, la más cercana no visitada es P4 (4 km). (Estudiantes acumulados: 13 + 2 = 15)
- Desde P4, la más cercana no visitada es P5 (3 km). (Estudiantes acumulados: 15 + 5 = 20)
- Desde P5, la única parada restante es P6 (11 km, aunque en un circuito real se optimizaría con el colegio, aquí cerramos con la última parada). (Estudiantes acumulados: 20 + 3 = 23)

**Ruta propuesta:** P1 → P2 → P3 → P4 → P5 → P6

**Distancia total:** 2 + 3 + 4 + 3 + 11 = 23 km

#### 4. Comparación con una alternativa

**Alternativa (ruta secuencial ordenada):** P1 → P2 → P3 → P6 → P4 → P5

**Distancia:** P1 → P2 (2) + P2 → P3 (3) + P3 → P6 (6) + P6 → P4 (8) + P4 → P5 (3) = 22 km

**Criterio elegido:** Menor distancia total.

**Conclusión:** Aunque la alternativa secuencial resulta ligeramente menor en distancia total (22 km frente a 23 km), el algoritmo del vecino más cercano suele ser más eficiente en tiempo real para evitar desvíos bruscos, aunque en este caso específico la alternativa directa ofrece un ahorro de 1 km.

---

## Tarea #3

### 1. Errores y limitaciones

1. **No calcula el promedio.** Solo suma las notas y no divide entre 3, así que un estudiante con 25, 20 y 30 (promedio real 25) aparece como aprobado porque la suma da 75.
2. **No valida las notas.** Acepta valores imposibles, como −10 o 150, que alteran el resultado. Un estudiante con 150, 10 y 20 "aprobaría".
3. **No muestra el promedio.** El estudiante solo ve "Aprueba" o "No aprueba", sin saber con cuánto.
4. **Usa `=` para asignar.** Se confunde con una comparación. En pseudocódigo lo correcto es usar `←`.

### 2. Pseudocódigo corregido

```
Inicio
   Leer nota1, nota2, nota3
   promedio ← (nota1 + nota2 + nota3) / 3
   Si promedio >= 60 Entonces
      Escribir "Aprueba"
   SiNo
      Escribir "No aprueba"
   FinSi
Fin
```

Los paréntesis son necesarios para que se sumen las tres notas antes de dividir.

### 3. Con validación de 0 a 100

```
Inicio
   suma ← 0
   Para i ← 1 Hasta 3 Hacer
      Escribir "Ingrese la nota ", i, ":"
      Leer nota
      Mientras nota < 0 O nota > 100 Hacer
         Escribir "Nota inválida. Debe estar entre 0 y 100:"
         Leer nota
      FinMientras
      suma ← suma + nota
   FinPara
   promedio ← suma / 3
   Si promedio >= 60 Entonces
      Escribir "Aprueba"
   SiNo
      Escribir "No aprueba"
   FinSi
Fin
```

El ciclo `Mientras` vuelve a pedir la nota hasta que sea válida, así ningún valor fuera de rango entra al cálculo.

### 4. Mejora adicional

La propuesta es mostrar el promedio, indicar si el estudiante debe ir a recuperación y permitir elegir cuántas notas ingresar. Para la recuperación se supuso un rango de 40 a 59.

```
Inicio
   Escribir "¿Cuántas notas desea ingresar?"
   Leer cantidad
   Mientras cantidad < 1 Hacer
      Escribir "Debe ingresar al menos 1 nota:"
      Leer cantidad
   FinMientras
   suma ← 0
   Para i ← 1 Hasta cantidad Hacer
      Escribir "Ingrese la nota ", i, ":"
      Leer nota
      Mientras nota < 0 O nota > 100 Hacer
         Escribir "Nota inválida. Debe estar entre 0 y 100:"
         Leer nota
      FinMientras
      suma ← suma + nota
   FinPara
   promedio ← suma / cantidad
   Escribir "Promedio: ", promedio
   Si promedio >= 60 Entonces
      Escribir "Aprueba"
   SiNo
      Si promedio >= 40 Entonces
         Escribir "Debe presentar recuperación"
      SiNo
         Escribir "No aprueba"
      FinSi
   FinSi
Fin
```
**Actividad 29/10**
En esta actividad de 5 partes, nosotros realizamos las primeras 4 partes que sigue la explicación del siguiente párrafo:

El circuito utiliza una placa Arduino Uno para controlar tres servomotores, cada uno con una forma de activación distinta. La protoboard distribuye la alimentación: el pin de 5V del Arduino se conecta a la fila positiva y el pin GND a la fila negativa. Las filas superiores e inferiores están unidas con cables, de modo que todos los componentes comparten la misma alimentación y la misma tierra. Cada servomotor tiene su cable de potencia en la fila positiva, su cable de tierra en la fila negativa y su cable de señal conectado directamente a un pin digital del Arduino.
El primer servomotor, ubicado a la izquierda, recibe su señal por el pin 11 y se activa con un pulsador conectado entre el pin 2 y GND. En el código se activa la resistencia pull-up interna del Arduino, por lo que el pin lee un nivel alto con el botón suelto y un nivel bajo al presionarlo. Cuando el programa detecta la pulsación, el servo gira a 150°, se mantiene un segundo en esa posición y regresa automáticamente a 0°. Es una acción momentánea que ocurre una vez por cada pulsación.
El segundo servomotor, ubicado en el centro, recibe su señal por el pin 10 y se controla con un switch deslizante. El terminal común del switch está conectado al pin 3 y uno de sus extremos a GND, también con la resistencia pull-up interna activada. Según la posición del switch, el servo adopta una posición fija de 30° o de 150° y se mantiene así mientras el switch no cambie, por lo que funciona como un control por estado.
El tercer servomotor, ubicado a la derecha, recibe su señal por el pin 9 y se controla con un potenciómetro. Los extremos del potenciómetro van a 5V y GND, y su terminal central a la entrada analógica A0. El Arduino lee un valor entre 0 y 1023, lo convierte en un ángulo de 0° a 180° mediante la función map(), y el servo sigue el giro de la perilla en tiempo real. Este control analógico y proporcional se diferencia claramente de los otros dos.
Finalmente, el programa usa la función millis() en lugar de delay() para medir el tiempo sin detener el Arduino. Gracias a esto, los tres servomotores pueden funcionar al mismo tiempo y de forma independiente, sin que la espera de uno interrumpa el movimiento de los demás.

*El código es :*
/*
  =====================================================================
   PROYECTO: Control de tres servomotores con Arduino
   Plataforma: Tinkercad Circuits / Arduino Uno R3
  =====================================================================

   DESCRIPCIÓN GENERAL
   -------------------
   Este programa controla tres servomotores de forma independiente,
   cada uno con un tipo de activación distinto:

   a) SERVO 1 (pin 11) -> Controlado por un PULSADOR (pin 2).
      Cuando se presiona el pulsador, el servo gira de 0° a 150°,
      se mantiene ahí durante 1 segundo y luego regresa solo a 0°.
      Es una acción MOMENTÁNEA: ocurre una vez por cada pulsación.

   b) SERVO 2 (pin 10) -> Controlado por un SWITCH DESLIZANTE (pin 3).
      El servo adopta una posición fija según la posición del switch:
        - Switch conectado a GND (lectura LOW)   -> 30°
        - Switch en la otra posición (HIGH)      -> 150°
      Es un control por ESTADO: el servo se queda en esa posición
      mientras el switch no cambie.

   c) SERVO 3 (pin 9)  -> Controlado por un POTENCIÓMETRO (pin A0).
      El ángulo del servo sigue de forma continua el giro del
      potenciómetro (0° a 180°). Es un control ANALÓGICO y
      PROPORCIONAL: cada posición de la perilla corresponde a un
      ángulo diferente del servo.

   CONEXIONES
   ----------
   - Servo 1 (izquierda): Señal -> pin 11
   - Servo 2 (centro):    Señal -> pin 10
   - Servo 3 (derecha):   Señal -> pin 9
   - Los 3 servos: Potencia -> fila + (5V) | Tierra -> fila - (GND)
   - Pulsador: una pata -> pin 2 | pata DIAGONAL opuesta -> GND
   - Switch deslizante: terminal común -> pin 3 | un extremo -> GND
   - Potenciómetro: extremos -> 5V y GND | terminal central -> A0

   NOTA IMPORTANTE
   ---------------
   El programa usa millis() en lugar de delay(). Así el Arduino nunca
   se queda "congelado" esperando, y los tres servos pueden responder
   al mismo tiempo (por ejemplo, mover el potenciómetro mientras el
   servo 1 está esperando en 150°).
  =====================================================================
*/

// Se incluye la librería Servo, que genera automáticamente las señales
// PWM de 50 Hz que necesitan los servomotores para posicionarse.
#include <Servo.h>

// Se crean tres objetos de tipo Servo, uno por cada servomotor.
// Cada objeto permite controlar su servo con funciones como write().
Servo servo1, servo2, servo3;

// ---------------------------------------------------------------------
// DEFINICIÓN DE PINES
// Se usan constantes (const) para que los números de pin no cambien
// por error y para que el código sea más fácil de leer y modificar.
// ---------------------------------------------------------------------
const int PIN_BOTON  = 2;    // Pin digital donde se conecta el pulsador
const int PIN_SWITCH = 3;    // Pin digital donde se conecta el switch deslizante
const int PIN_POT    = A0;   // Pin analógico donde se conecta el potenciómetro

const int PIN_SERVO1 = 11;   // Servo 1 (izquierda) -> pulsador
const int PIN_SERVO2 = 10;   // Servo 2 (centro)    -> switch
const int PIN_SERVO3 = 9;    // Servo 3 (derecha)   -> potenciómetro

// ---------------------------------------------------------------------
// VARIABLES DE CONTROL DEL SERVO 1
// ---------------------------------------------------------------------
// Indica si el servo 1 está realizando su movimiento (true) o si está
// en reposo (false). Sirve para ignorar nuevas pulsaciones mientras
// el servo todavía no ha regresado a 0°.
bool s1Activo = false;

// Guarda la lectura anterior del pulsador. Comparándola con la lectura
// actual se detecta el instante exacto en que el botón se presiona
// (detección de flanco), evitando que el movimiento se repita
// mientras el botón se mantiene presionado.
// Empieza en HIGH porque, con INPUT_PULLUP, el botón suelto lee HIGH.
bool botonPrevio = HIGH;

// Guarda el momento (en milisegundos) en que comenzó el movimiento
// del servo 1. Se usa para medir cuánto tiempo lleva en 150°.
// Se declara "unsigned long" porque millis() devuelve números muy
// grandes que no caben en un int.
unsigned long s1Inicio = 0;

// =====================================================================
// SETUP: se ejecuta UNA sola vez al encender o reiniciar el Arduino.
// Aquí se configuran los pines y la posición inicial de los servos.
// =====================================================================
void setup() {
  // Se configura el pin del pulsador como entrada con resistencia
  // PULL-UP interna. Esto mantiene el pin en HIGH (5 V) cuando el botón
  // está suelto, y lo lleva a LOW (0 V) cuando se presiona, porque el
  // botón lo conecta a GND. Así no se necesita resistencia externa.
  pinMode(PIN_BOTON, INPUT_PULLUP);

  // El switch también usa la resistencia PULL-UP interna:
  // - Si el switch conecta el pin 3 con GND  -> se lee LOW.
  // - Si el switch lo desconecta de GND      -> la pull-up lo pone en HIGH.
  pinMode(PIN_SWITCH, INPUT_PULLUP);

  // Se asocia cada objeto Servo con el pin digital donde está conectado
  // su cable de señal. A partir de aquí la librería empieza a enviar
  // pulsos a esos pines.
  servo1.attach(PIN_SERVO1);   // Servo 1 en el pin 11
  servo2.attach(PIN_SERVO2);   // Servo 2 en el pin 10
  servo3.attach(PIN_SERVO3);   // Servo 3 en el pin 9

  // Se colocan los servos en su posición inicial para que el sistema
  // siempre arranque desde un estado conocido.
  servo1.write(0);     // Servo 1 en reposo a 0°
  servo2.write(30);    // Servo 2 en su posición inicial de 30°
  servo3.write(0);     // Servo 3 a 0° (luego seguirá al potenciómetro)

  // Se inicia el Monitor Serie para ver mensajes de prueba.
  Serial.begin(9600);
  Serial.println("Sistema listo");
}

// =====================================================================
// LOOP: se repite continuamente mientras el Arduino esté encendido.
// En cada vuelta se revisan las tres entradas y se actualizan los
// tres servos. Como no hay delay(), cada vuelta tarda muy poco tiempo.
// =====================================================================
void loop() {
  // Se lee el tiempo actual en milisegundos desde que arrancó el Arduino.
  // Se guarda en una variable para usar el mismo valor en todo el ciclo.
  unsigned long ahora = millis();

  // -------------------------------------------------------------------
  // SERVO 1: CONTROL CON PULSADOR
  // Objetivo: al presionar el botón, girar a 150° y regresar a 0°
  // después de 1 segundo.
  // -------------------------------------------------------------------

  // Se lee el estado actual del pulsador:
  // HIGH = suelto, LOW = presionado (por el uso de INPUT_PULLUP).
  bool boton = digitalRead(PIN_BOTON);

  // Se inicia el movimiento solo si se cumplen TRES condiciones:
  // 1) boton == LOW           -> el botón está presionado ahora.
  // 2) botonPrevio == HIGH    -> en la vuelta anterior estaba suelto
  //                              (es decir, se ACABA de presionar).
  // 3) !s1Activo              -> el servo no está ya en movimiento.
  if (boton == LOW && botonPrevio == HIGH && !s1Activo) {
    s1Activo = true;        // Se marca que el servo 1 está en acción
    s1Inicio = ahora;       // Se guarda el momento en que empezó
    servo1.write(150);      // Se ordena al servo girar a 150°
    Serial.println("Servo 1: pulsador presionado -> 150 grados");
  }

  // Se actualiza la lectura previa para compararla en la próxima vuelta.
  botonPrevio = boton;

  // Si el servo 1 está activo y ya pasaron 1000 ms (1 segundo) desde que
  // empezó, se ordena el regreso a 0° y se vuelve al estado de reposo.
  // La resta (ahora - s1Inicio) da el tiempo transcurrido.
  if (s1Activo && ahora - s1Inicio >= 1000) {
    servo1.write(0);        // El servo regresa a su posición inicial
    s1Activo = false;       // Queda listo para una nueva pulsación
    Serial.println("Servo 1: regresa a 0 grados");
  }

  // -------------------------------------------------------------------
  // SERVO 2: CONTROL CON SWITCH DESLIZANTE
  // Objetivo: adoptar una posición distinta según la posición del switch.
  // -------------------------------------------------------------------

  // Se lee el switch en cada vuelta del loop:
  if (digitalRead(PIN_SWITCH) == LOW) {
    // Switch conectado a GND -> posición de 30°
    servo2.write(30);
  } else {
    // Switch en la otra posición (pin en HIGH por la pull-up) -> 150°
    servo2.write(150);
  }
  // Nota: enviar el mismo ángulo repetidamente no causa problemas; el
  // servo simplemente se mantiene en esa posición.

  // -------------------------------------------------------------------
  // SERVO 3: CONTROL CON POTENCIÓMETRO
  // Objetivo: que el servo siga de forma proporcional el giro de la
  // perilla del potenciómetro.
  // -------------------------------------------------------------------

  // 1) analogRead(PIN_POT) lee el voltaje del potenciómetro y lo
  //    convierte en un número entre 0 (0 V) y 1023 (5 V).
  // 2) map() convierte ese rango de 0-1023 al rango de ángulos 0-180.
  //    Ejemplo: lectura 512 (mitad) -> ángulo 90°.
  int angulo3 = map(analogRead(PIN_POT), 0, 1023, 0, 180);

  // Se envía el ángulo calculado al servo 3. Como esto ocurre en cada
  // vuelta del loop, el servo responde de inmediato al mover la perilla.
  servo3.write(angulo3);
}

**Fotos**
<img width="1754" height="1058" alt="IMG_3209" src="https://github.com/user-attachments/assets/b97cea12-96a8-41ab-95e6-1e27e0191124" />
