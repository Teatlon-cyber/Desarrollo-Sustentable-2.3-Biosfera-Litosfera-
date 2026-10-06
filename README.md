[README Litosfera.md](https://github.com/user-attachments/files/33128373/README.Litosfera.md)
# 🌱 Monitor de humedad del suelo con Arduino UNO R4 WiFi

Práctica de electrónica y programación con un **sensor de humedad de suelo** y una placa **Arduino UNO R4 WiFi**. El sistema mide la humedad de la tierra, muestra los datos en el Monitor Serie y enciende un **LED de alerta cuando la tierra está seca y necesita riego**.

| | |
|---|---|
| 🎥 **Video de la práctica** | [Ver en YouTube](https://youtube.com/shorts/aXIc1PdyHog?feature=share) |
| 📄 **Informe de resultados (PDF)** | [`docs/Informe_Practica_Sensor_Humedad_Arduino.pdf`](docs/Informe_Practica_Sensor_Humedad_Arduino.pdf) |
| 💻 **Código fuente** | [`src/monitor_humedad/monitor_humedad.ino`](src/monitor_humedad/monitor_humedad.ino) |

## 🎥 Video de la práctica

[![Video de la práctica en YouTube](https://img.youtube.com/vi/aXIc1PdyHog/0.jpg)](https://youtube.com/shorts/aXIc1PdyHog?feature=share)

> Haz clic en la imagen para ver el video en YouTube.

## 📋 Descripción

La práctica se realizó con dos condiciones de prueba: **tierra seca** y **tierra húmeda**. El sensor entrega una señal analógica que la placa lee por el pin `A0`; el programa la compara con un umbral y decide el estado del LED.

| Condición del suelo | Lectura del sensor | LED | Interpretación |
|---|---|---|---|
| Tierra **seca** | Menor que el umbral (400) | 🔴 **Encendido** | Necesita riego |
| Tierra **húmeda** | Mayor o igual que el umbral (400) | ⚫ **Apagado** | No necesita riego |

## 🧰 Materiales

- Arduino UNO R4 WiFi
- Sensor de humedad de suelo (2 puntas, salida analógica)
- LED rojo y resistencia limitadora
- Protoboard y cables jumper
- Cable USB
- Laptop con [Arduino IDE](https://www.arduino.cc/en/software)
- Vaso con tierra y agua

## 🔌 Diagrama del circuito (Tinkercad)

![Diagrama del circuito en Tinkercad](images/diagrama-tinkercad.png)

| Elemento | Conexión |
|---|---|
| Sensor – VCC (cable rojo) | 5 V |
| Sensor – GND (cable negro) | GND |
| Sensor – señal (cable azul) | `A0` |
| Pin digital `13` | Resistencia → ánodo (+) del LED |
| Cátodo (–) del LED | GND |

## 📸 Montaje real

![Montaje real de la práctica](images/montaje-real.jpg)

Fotogramas del video:

<p align="center">
  <img src="images/video-fotograma-1.png" alt="Fotograma 1 del video" height="380">
  <img src="images/video-fotograma-2.png" alt="Fotograma 2 del video" height="380">
</p>

## 💻 Código

Archivo completo: [`src/monitor_humedad/monitor_humedad.ino`](src/monitor_humedad/monitor_humedad.ino)

```cpp
const int PIN_SENSOR = A0;   // Señal analógica del sensor
const int LED_PIN    = 13;   // LED (integrado y externo) de alerta de riego

// Umbral: ajústalo según las lecturas que veas en el Monitor Serie
const int UMBRAL_HUMEDAD = 400;

void setup() {
  Serial.begin(9600);
  pinMode(LED_PIN, OUTPUT);
  Serial.println("=== Monitor de humedad del suelo ===");
}

void loop() {
  int lectura = analogRead(PIN_SENSOR);

  // A menor lectura, menor porcentaje de humedad
  int porcentaje = map(lectura, 0, 1023, 0, 100);
  porcentaje = constrain(porcentaje, 0, 100);

  Serial.print("Lectura: ");
  Serial.print(lectura);
  Serial.print("  |  Humedad: ");
  Serial.print(porcentaje);
  Serial.print("%  |  Estado: ");

  // Lectura baja -> TIERRA SECA -> ENCIENDE el LED (alerta de riego)
  if (lectura < UMBRAL_HUMEDAD) {
    Serial.println("TIERRA SECA -> Se recomienda regar");
    digitalWrite(LED_PIN, HIGH);
  }
  // Lectura alta -> TIERRA HÚMEDA -> APAGA el LED
  else {
    Serial.println("TIERRA HUMEDA -> No necesita riego");
    digitalWrite(LED_PIN, LOW);
  }

  delay(1000);
}
```

### Cómo funciona

1. `analogRead()` lee la señal del sensor (valores de 0 a 1023).
2. `map()` y `constrain()` convierten la lectura a un porcentaje de 0 a 100 %.
3. Si la lectura es **menor que 400**, la tierra está seca y se **enciende** el LED.
4. En caso contrario, la tierra está húmeda y el LED **se apaga**.
5. Se repite una medición cada segundo (`delay(1000)`).

> **Nota:** en la versión usada durante la práctica, los mensajes del Monitor Serie estaban intercambiados respecto al estado real del suelo (el LED sí funcionaba bien). En este repositorio los textos ya están corregidos. Más detalles en el informe, sección 9.2.

## ▶️ Cómo reproducir la práctica

1. Arma el circuito siguiendo el diagrama de Tinkercad.
2. Abre Arduino IDE y selecciona la placa **Arduino UNO R4 WiFi** (instala el paquete *Arduino UNO R4 Boards* si es necesario).
3. Abre `src/monitor_humedad/monitor_humedad.ino` y cárgalo a la placa.
4. Abre el **Monitor Serie** a **9600 baudios**.
5. Clava el sensor en tierra seca y luego en tierra húmeda, y observa el LED.
6. Si es necesario, calibra `UMBRAL_HUMEDAD` con las lecturas de tu propio suelo.

## 📄 Informe de resultados

El informe completo (objetivos, marco teórico, conexiones, resultados, análisis y conclusiones) está en
[`docs/Informe_Practica_Sensor_Humedad_Arduino.pdf`](docs/Informe_Practica_Sensor_Humedad_Arduino.pdf).

## 🚀 Posibles mejoras

- Promediar varias lecturas para reducir el ruido.
- Usar un sensor capacitivo, que no se corroe.
- Enviar alertas por WiFi aprovechando la UNO R4 WiFi.
- Automatizar el riego con una bomba y un relé.

## 📁 Estructura del repositorio

```
.
├── README.md
├── .gitignore
├── docs/
│   └── Informe_Practica_Sensor_Humedad_Arduino.pdf
├── images/
│   ├── diagrama-tinkercad.png
│   ├── montaje-real.jpg
│   ├── video-fotograma-1.png
│   └── video-fotograma-2.png
└── src/
    └── monitor_humedad/
        └── monitor_humedad.ino
```
