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
