# PRÁCTICA 7.1: REPRODUCCIÓN DESDE MEMORIA INTERNA

## CÓDIGO
```ruby
#include "Arduino.h" 
#include "FS.h"
#include "HTTPClient.h"
#include "SPIFFS.h"
#include "SD.h"
#include "AudioGeneratorAAC.h"
#include "AudioOutputI2S.h"
#include "AudioFileSourcePROGMEM.h"
#include "sampleaac.h"

AudioFileSourcePROGMEM *in;
AudioGeneratorAAC *aac;
AudioOutputI2S *out;

void setup(){
  Serial.begin(9600);

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

## FUNCIONAMENTO

Para empezar añadimos las siguientes nueve, las cuatro últimas son librerias de la biblioteca de audio ESP8266 de Earle F.Philhower:

```ruby
#include "Arduino.h" 
#include "FS.h"
#include "HTTPClient.h"
#include "SPIFFS.h"
#include "SD.h"
#include "AudioGeneratorAAC.h"
#include "AudioOutputI2S.h"
#include "AudioFileSourcePROGMEM.h"
#include "sampleaac.h"
```

A continuación declaramos tres punteros, el primero `AudioFileSourcePROGMEM` lee un archivo de una matriz PROGREM, el segundo `AudioGeneratorAAC` reproduce un archivo AAC mono o estéreo utilizando el decodificador AAC de punto fijo Helix y por último el `AudioOutputI2` envía señales estéreo o mono a cualquier frecuencia establecida.

```ruby
AudioFileSourcePROGMEM *in;
AudioGeneratorAAC *aac;
AudioOutputI2S *out;
```

Después empezamos el `void setup()`, este en la primera línea inicializa una comunicación en serie a una velocidad de 115200 bauds. Primero declaramos el punetro **in* como una variable que llevará nuestro fichero de audio. Después para empezar la comunicación entre decodificador y la placa utilizaremos el punetro **aac*. Y por último para las diferentes configuraciones como: la ganancia, los pines de salida, forzar una salida mono, etc; Utilizaremos el puntero **out*. 

```rudy
void setup(){
  Serial.begin(9600);

  in = new AudioFileSourcePROGMEM(sampleaac, sizeof(sampleaac));
  aac = new AudioGeneratorAAC();
  out = new AudioOutputI2S();
  out -> SetGain(0.125);
  out -> SetPinout(26,25,22);
  aac->begin(in, out);
}
```

Para finalizar miraremos el `void loop()`, este para empezar hacemos un condicional donde miramos si la variable *Running* es *true* para saber que no hay ningún error y le hemos pasado el fichero correctamente, si es así llama a la función `loop()`. Si la variable fuese *false* se llamaría a la función `stop()` para cerrar el fichero de audio.   

```ruby
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
