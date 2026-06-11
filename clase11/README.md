
### Avances

La lista de plantas que usaremos son las plantas disponibles en la huerta del Museo Violeta Parra:

- Acelga
- Ajo
- Alcachofa
- Amaranto
- Apio
- Artemisa
- Arvejas
- Berenjena
- Berros
- Betarraga
- Brócoli
- Capuchina
- Cartucho enano o Perrito
- Cebollín
- Cilantro
- Espinaca
- Flor de Ajo
- Girasol
- Grosella
- Habas
- Kale
- Lechuga
- Llantén
- Malvas 
- Menta
- Mizuna Morada
- Mizuna Verde
- Oregano
- Papas
- Puerro
- Rabanito
- Repollo
- Romero
- Ruda
- Tagetes
- Topinambur
- Zinnias

``
#include <SoftwareSerial.h>

// Pin 10 = RX Arduino (recibe desde TX impresora)
// Pin 11 = TX Arduino (envía hacia RX impresora)
SoftwareSerial impresora(10, 11);

// Comandos ESC/POS
const byte ESC = 0x1B;
const byte GS  = 0x1D;

void setup() {
  impresora.begin(9600); // cambia si tu impresora usa otro baudrate
  delay(1000);           // esperar que la impresora esté lista
  imprimirPrueba();
}

void loop() {
  // vacío por ahora
}

void enviarComando(const byte* cmd, int largo) {
  for (int i = 0; i < largo; i++) {
    impresora.write(cmd[i]);
  }
  delay(50);
}

void imprimirTexto(const char* texto) {
  impresora.print(texto);
  delay(30);
}

void imprimirPrueba() {
  // Inicializar impresora
  byte init[] = {ESC, '@'};
  enviarComando(init, 2);

  // Centrar texto
  byte centrar[] = {ESC, 'a', 0x01};
  enviarComando(centrar, 3);

  // Negrita ON
  byte boldOn[] = {ESC, 'E', 0x01};
  enviarComando(boldOn, 3);

  imprimirTexto("HUERTA VIOLETA PARRA\n");

  // Negrita OFF
  byte boldOff[] = {ESC, 'E', 0x00};
  enviarComando(boldOff, 3);

  imprimirTexto("Saberes de la tierra\n");
  imprimirTexto("----------------------------\n");

  // Alinear izquierda
  byte izquierda[] = {ESC, 'a', 0x00};
  enviarComando(izquierda, 3);

  imprimirTexto("Planta: Menta\n");
  imprimirTexto("Uso: Digestiva, antipiretica\n");
  imprimirTexto("----------------------------\n");

  // Centrar para pie
  enviarComando(centrar, 3);
  imprimirTexto("Este saber es libre\n");

  // Avanzar papel y cortar
  byte avanzar[] = {ESC, 'd', 0x04};
  enviarComando(avanzar, 3);

  byte cortar[] = {GS, 'V', 0x41, 0x00};
  enviarComando(cortar, 4);
}

``
