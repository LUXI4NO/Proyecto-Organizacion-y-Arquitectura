# Laboratorio Arduino 

## Autores 
- Alvarez Luciano Ezequiel

## Tabla de Contenidos
1. [Introducción](#introducción)
2. [Desarrollo](#desarrollo)
    - [Objetivo del Proyecto](#objetivo-del-proyecto)
    - [Metodología](#metodología)
3. [Informe del Funcionamiento del Código](#informe-del-funcionamiento-del-código)
4. [Conclusiones](#conclusiones)
5. [Bibliografía Consultada](#bibliografía-consultada)

## Introduccion	 
En este proyecto, nos embarcamos en la creación de una versión moderna del clásico juego de memoria Simón. Este juego electrónico, popularizado en los años 80, continúa siendo una herramienta divertida y desafiante para ejercitar la memoria y la concentración.

El juego Simón se fundamenta en una estructura simple: cuatro botones de colores (rojo, azul, amarillo y verde). Al iniciar, Simón genera una secuencia aleatoria iluminando uno de estos botones. El desafío consiste en repetir correctamente la secuencia. Con cada acierto, el juego añade un nuevo color, incrementando así la complejidad en cada ronda.

El objetivo principal del juego es mejorar las habilidades cognitivas al recordar y reproducir secuencias cada vez más extensas y complejas de colores.

En este proyecto, exploraremos mejoras que adapten el juego a las preferencias y tecnologías actuales. Esto abarcará el diseño de circuitos electrónicos, la programación del comportamiento del juego y la creación de una interfaz de usuario optimizada.

## Desarrollo	 
### Objetivo del Proyecto
El objetivo principal del proyecto fue diseñar y simular un juego "Simón Dice" utilizando una placa Arduino Nano y el software Proteus 8 Professional. El sistema debía incluir:

- Cuatro LEDs para mostrar las secuencias de colores.
- Cuatro botones para que el jugador ingrese la secuencia.
- Una terminal virtual que indique si la secuencia ingresada es correcta, incorrecta o si el jugador ha perdido por tiempo.

### Metodología
Para llevar a cabo el proyecto, se siguieron los siguientes pasos:

1. **Selección de Componentes:**
    - Placa Arduino Nano
    - 4 LEDs
    - 4 Botones
    - Software Proteus 8 Professional para la simulación
    - Arduino IDE para la programación

2. **Desarrollo del Código:** El código en Arduino fue diseñado para generar una secuencia aleatoria de colores, mostrarla mediante LEDs y verificar la secuencia ingresada por el jugador.

3. **Simulación en Proteus:** La simulación se llevó a cabo en Proteus 8 Professional, permitiendo validar el correcto funcionamiento del circuito y la lógica del juego.

![image](https://github.com/LUXI4NO/Proyecto-Arduino/assets/140111840/0a4e6d8a-1f93-4cd8-a1dc-7060901df0ba)

## Informe del Funcionamiento del Código
### Biblioteca y Variables
El código utiliza la biblioteca `EEPROM.h` para almacenar y recuperar la secuencia de colores generada. Las principales variables utilizadas son:

- `cntjuego`: un contador de juegos que lleva el número de secuencias generadas.
- `color`: una variable para almacenar el color generado aleatoriamente.

```cpp
#include <EEPROM.h>

int cntjuego; // Un contador de juegos que lleva el número de secuencias generadas
int color;    // Una variable para almacenar el color generado aleatoriamente
```

## Configuración Inicial

En la función `setup()`, se realiza la configuración inicial del programa. Las operaciones clave incluyen:

- Configuración de la comunicación serial a 9600 baudios para la depuración y la interacción con el usuario.
- Inicialización de los pines 4 a 11 como salidas. Estos pines son esenciales para controlar los LEDs que representan los colores y para leer las entradas del usuario durante el juego.

```cpp
void setup() {
  Serial.begin(9600); // Inicializa la comunicación serial a 9600 baudios
  pinMode(4, OUTPUT); // Configura el pin 4 como salida
  pinMode(5, OUTPUT); // Configura el pin 5 como salida
  pinMode(6, OUTPUT); // Configura el pin 6 como salida
  pinMode(7, OUTPUT); // Configura el pin 7 como salida
  pinMode(8, OUTPUT); // Configura el pin 8 como salida
  pinMode(9, OUTPUT); // Configura el pin 9 como salida
  pinMode(10, OUTPUT); // Configura el pin 10 como salida
  pinMode(11, OUTPUT); // Configura el pin 11 como salida
}
```

## Bucle Principal

El `loop()` principal del programa contiene tres llamadas a funciones clave que manejan el flujo del juego:

1. **`crearColor()`:** Esta función genera un nuevo color aleatorio y lo almacena en la EEPROM como parte de la secuencia del juego.

2. **`mostrarColor()`:** Muestra la secuencia de colores generada hasta el momento en los LEDs correspondientes durante 500 milisegundos cada uno.

3. **`comprobarRespuesta()`:** Verifica si la respuesta del usuario es correcta comparándola con la secuencia almacenada en la EEPROM. Espera la entrada del usuario hasta que se presione un botón. Si la respuesta es correcta, el juego continúa; de lo contrario, todos los LEDs parpadean para indicar un error. Si el usuario tarda demasiado en responder, se indica una pérdida por tiempo.

```cpp
void loop() {
  crearColor(); // Llama a la función que crea un color aleatorio
  mostrarColor(); // Muestra la secuencia de colores almacenados
  comprobarRespuesta(); // Comprueba si la respuesta del usuario es correcta
}
```

## Función `crearColor()`

La función `crearColor()` realiza las siguientes operaciones:

- Incrementa el contador de juegos.
- Genera un color aleatorio entre 1 y 4.
- Guarda el valor del color generado en la EEPROM.

Los valores de los colores se almacenan en la EEPROM con valores específicos (17, 18, 20 y 24) que corresponden a diferentes pines de salida.
```cpp
void crearColor() {
  cntjuego++; // Incrementa el contador de juegos
  color = random(1, 5); // Genera un color aleatorio entre 1 y 4

  // Guarda el color en la EEPROM en función del valor generado
  switch (color) {
    case 1:
      EEPROM.write(cntjuego, 17);
      break;
    case 2:
      EEPROM.write(cntjuego, 18);
      break;
    case 3:
      EEPROM.write(cntjuego, 20);
      break;
    case 4:
      EEPROM.write(cntjuego, 24);
      break;
    default:
      break;
  }
  Serial.println(cntjuego); // Imprime el número de juego
  Serial.print("Color creado: "); // Imprime el color creado
  Serial.println(EEPROM.read(cntjuego));
  Serial.println("");
}
```

## Función `mostrarColor()`

La función `mostrarColor()` recorre la secuencia de colores almacenados en la EEPROM y enciende el LED correspondiente a cada color durante 500 milisegundos.

```cpp
oid mostrarColor() {
  // Muestra la secuencia de colores almacenados en la EEPROM
  for (int i = 1; i < cntjuego + 1; i++) {
    if (i == 1) {
      Serial.println("Nueva Secuencia"); // Imprime que es una nueva secuencia
    }
    delay(500); // Espera 500 milisegundos
    Serial.print("Se ejecuta "); // Imprime el número de ejecución
    Serial.println(i);

    // Enciende y apaga el LED correspondiente al color almacenado
    switch (EEPROM.read(i)) {
      case 17:
        Serial.println("Color guardado: 17, salida 4");
        digitalWrite(4, HIGH);
        delay(500);
        digitalWrite(4, LOW);
        break;
      case 18:
        Serial.println("Color guardado: 18, salida 5");
        digitalWrite(5, HIGH);
        delay(500);
        digitalWrite(5, LOW);
        break;
      case 20:
        Serial.println("Color guardado: 20, salida 6");
        digitalWrite(6, HIGH);
        delay(500);
        digitalWrite(6, LOW);
        break;
      case 24:
        Serial.println("Color guardado: 24, salida 7");
        digitalWrite(7, HIGH);
        delay(500);
        digitalWrite(7, LOW);
        break;
      default:
        break;
    }
  }
}
```

## Función `comprobarRespuesta()`

En la función `comprobarRespuesta()`, se verifica la respuesta del usuario comparándola con la secuencia almacenada. El proceso implica esperar la entrada del usuario hasta que se presione un botón. Si la respuesta es correcta, el juego continúa; de lo contrario, todos los LEDs parpadean para indicar un error. Además, si el usuario tarda demasiado en responder, se activa una señal de pérdida por tiempo.
```cpp
void comprobarRespuesta() {
  // Comprueba la respuesta del usuario para cada color en la secuencia
  for (int i = 1; i < cntjuego + 1; i++) {
    float cntCompr = 0; // Contador para el tiempo de espera
    int respuesta = 0; // Variable para almacenar la respuesta del usuario
    Serial.print("Variable i= ");
    Serial.println(i);
    
    // Espera hasta que el usuario proporcione una respuesta
    while (1) {
      // Lee las entradas digitales y las combina en un valor
      respuesta = 0b00010000 | digitalRead(11) << 3 | digitalRead(10) << 2 | digitalRead(9) << 1 | digitalRead(8);
      if (respuesta != 16) {
        Serial.print("Respuesta = ");
        Serial.println(respuesta);
        // Si la respuesta es correcta, se rompe el bucle
        if (respuesta == EEPROM.read(i)) {
          Serial.println("Correcto");
          respuesta = 16;
          delay(500);
          break;
        } else {
          // Si la respuesta es incorrecta, se indica y se hace parpadear los LEDs
          while (1) {
            Serial.println("Incorrecto");
            digitalWrite(4, HIGH);
            digitalWrite(5, HIGH);
            digitalWrite(6, HIGH);
            digitalWrite(7, HIGH);
            delay(500);
            digitalWrite(4, LOW);
            digitalWrite(5, LOW);
            digitalWrite(6, LOW);
            digitalWrite(7, LOW);
            delay(500);
          }
        }
      }
      cntCompr++;
      // Si el contador de tiempo supera un umbral, se indica que se pierde por tiempo
      if (cntCompr >= 50000) {
        while (1) {
          Serial.println("Pierdes por tiempo");
          digitalWrite(4, HIGH);
          digitalWrite(5, HIGH);
          digitalWrite(6, HIGH);
          digitalWrite(7, HIGH);
          delay(500);
          digitalWrite(4, LOW);
          digitalWrite(5, LOW);
          digitalWrite(6, LOW);
          digitalWrite(7, LOW);
          delay(600);
        }
      }
    }
  }
}

```
## Conclusiones

El código presentado implementa un juego de memoria en un microcontrolador Arduino, donde el usuario debe reproducir una secuencia aleatoria de colores generada por el sistema. A lo largo del desarrollo del código, se utilizaron diversas técnicas para interactuar con el hardware, como el uso de la biblioteca EEPROM para almacenar datos de manera persistente y la manipulación de pines digitales para controlar LEDs.

Este programa no solo proporciona una experiencia de juego interactiva, sino que también demuestra varios conceptos importantes, como la generación de números aleatorios, la lectura de entradas digitales y el control de salidas. Además, la implementación de mecanismos para verificar las respuestas del usuario y gestionar errores (como respuestas incorrectas o demoras excesivas) muestra cómo se pueden manejar diferentes escenarios en una aplicación de tiempo real.

### Mejoras Futuras

1. **Mejoras en el generador de números aleatorios:** Para evitar patrones predecibles en la secuencia de colores.
   
2. **Agregar sonido:** Introducir sonidos para hacer el juego más interactivo. Utilizar diferentes tonos para indicar eventos como la creación de un color, la correcta reproducción de la secuencia y los errores.

3. **Reducción del tiempo de espera entre secuencias:** Disminuir el tiempo de espera entre cada nueva secuencia para hacer el juego más dinámico y ágil.


 
