# PRACTICA 5: BUSES DE COMUNICACIÓN II

## INFORME EJERCICIO 3

### Salida de depuración

<!--Images-->

![Terminal](Terminal.jpg)


### Codigo generado

```
#include "Arduino.h"
#include "FS.h"
#include "HTTPClient.h"
#include "SPIFFS.h"
#include "SD.h"
#include "SPI.h"
#include "AudioGeneratorAAC.h"
#include "AudioOutputI2S.h"
#include "AudioFileSourcePROGMEM.h"
#include "sampleaac.h"

AudioFileSourcePROGMEM *in;
AudioGeneratorAAC *aac;
AudioOutputI2S *out;

void setup(){

  Serial.begin(115200);
  in = new AudioFileSourcePROGMEM(sampleaac, sizeof(sampleaac));
  aac = new AudioGeneratorAAC();
  out = new AudioOutputI2S();
  out -> SetGain(0.125);
  out -> SetPinout(26,25,22);
  aac->begin(in, out);

}

void loop(){

  if (aac->isRunning()) {

    aac->loop();

    } else {

      aac -> stop();
      Serial.printf("Sound Generator\n");
      delay(1000);

  }

}
``` 

### Explicación del codigo

Primero de todo tenemos la definición de las librerias necesarias para el codigo y las variables in, aac y out que son de clases AudioFileSourcePROGMEM, AudioGeneratorAAC y AudioOutputI2S respectivamente.

En el setup se inicializa el Serial con una velocidad de monitor de 115200 y se asignan nuevas clases a in, aac y out. Hecho esto se define la ganancia de out y por ultimo aac relaciona la entrada (in) con la salida (out).

Por ultimo tenemos el loop que lo que hace es, cuando esta en bucle el aac decodificara, y en cuanto el bucle pare mostrara por el terminal el mensaje "Sound Generator"