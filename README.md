# Documentacion
![Tinkercad](./img/ArduinoTinkercad.jpg)


## Integrantes 
- Tobias Esquivel


## Proyecto: Contador 7segmentos.
![Tinkercad](./img/proyectoIMg.png)


## Descripción
Este proyecto consta de una placa de arduino en la cual el usuario puede contar hasta 99 aumentando, reduciendo y reiniciando
el valor numerico reflejado en los 7segmentos

## Función principal
Esta funcion se encarga de en primer instancia apagar los leds que previamente
pudieron quedar encendidos en el 7 segmentos actual seguido de 
el prender de el numero pasado por parametro reflejado visualmente en el 7 segmentos


~~~ C (lenguaje en el que esta escrito)
void encenderNumero(int numero)
{
  apagarLeds();
  switch (numero)
  {
    case 0:
      encenderNumeroCero();
      break;
    case 1:
      encenderNumeroUno();
      break;
    case 2:
      encenderNumeroDos();
      break;
    case 3:
      encenderNumeroTres();
      break;
    case 4:
      encenderNumeroCuatro();
      break;
    case 5:
      encenderNumeroCinco();
      break;
    case 6:
      encenderNumeroSeis();
      break;
    case 7:
      encenderNumeroSiete();
      break;
    case 8:
      encenderNumeroOcho();
      break;
    case 9:
      encenderNumeroNueve();
      break;
  }
}
~~~

## Link al proyecto
- [proyecto](https://www.tinkercad.com/things/fQp0TJIsrR3-copy-of-tobias-esquivel-tercer-parte-primer-parcial/editel?sharecode=MbqTXbX8fXxpuOUObsU5HDWkv8g9skBMmPj2RHbyJSA)

## Esquema del proyecto
- [esquema](./Esquema/esquema.png)



## Multiplexación

En este proyecto la multiplexacion se controla principalmente segun las siguientes lineas de codigo

if (digitalRead(UNIDAD) == LOW)
{
  digitalWrite(DECENA,LOW);
  digitalWrite(UNIDAD,HIGH);  
}
else 
{
  digitalWrite(DECENA,HIGH);
  digitalWrite(UNIDAD,LOW);
}

En ellas lo que se hace es que en el caso de que el pin que esta conectado al 7 segmentos ,que 
representa la unidad numerca, este dandole un 0 por valor, es decir que se pueda encender,
se intercambie por 1 para que no pueda encenderse y al mismo tiempo que ocurre esto, el 7 segmentos
de las decenas se activa y opera como debe.
En el otro caso de que esa condicion sea falsa ocurre lo contrario, para mostrar el valor requerido
para el 7segmentos de turno, entran en juego las siguientes lineas:

if (digitalRead(UNIDAD) == LOW)
{
  numeroAMostrar = contador % 10;
}
else
{
  numeroAMostrar = contador / 10;
}
encenderNumero(numeroAMostrar);

en ellas lo que sucede es que en caso de que se requiera mostrar la unidad solo se queda con el valor unitario del numero compuesto
y en caso de que se requiera mostrar la decena, solo se queda con el valor que ocupa la misma en el numero.



## investigación -> Objeto: Control remoto

El control remoto es un objeto que nos permite que a travez de sus botones, podamos asignarle 
a cada uno una funcionalidad en particular para afectar a nuestro arduino

La integracion del mismo en el proyecto es la de que a travez de sus botones 1 y 2, suba y baje el contador
del numero representado en el 7 segmentos

  Control remoto: Descripcion del componente

  El mismo para funcionar tiene un receptor el cual es el que nos permite recibir las ondas
  de frecuencia proporcionadas por el control remoto.

  A travez de la funcion "IrReceiver.begin(PIN_REMOTO, DISABLE_LED_FEEDBACK); " en el 
  apartado de setup, podremos inicializar el mismo en nuestro arduino.



Funcionamiento dentro del programa y cuales son las instrucciones proporcionadas:

  // Control remoto
  ~~~ C
  if (IrReceiver.decode()){ // Decodifica la entrada del control remoto y retorna 1 si es correcta
    switch (IrReceiver.decodedIRData.decodedRawData) // Al valor que decodeo el receptor, lo contemplamos para verificar su valor
    {
      case 0xEF10BF00: // En caso de que sea el equivalente al retorno que seria la representacion del boton 1
      mostrarPrimos ? indiceNumeroPrimo++ : numeroEnteroActual++; // Aumenta el contador del numero respectivo
      	break;
      case 0xEE11BF00:// En caso de que sea el equivalente al retorno que seria la representacion del boton 2
      mostrarPrimos ? indiceNumeroPrimo-- : numeroEnteroActual--; // Reduce el contador del numero respectivo
      	break;
      
    }
      control.resume();
    
  }
  ~~~ 
  // Fin control remoto

## investigación -> Objeto: Sensor de luz ambiental

El sensor de luz ambiental es un componente que se puede agregar al arduino y su funcion es
la de detectar la cantidad de luz que hay en el ambiente.

El mismo tiene 2 patas en las cuales podemos hacer las conexiones. Una es la llamada "collector"
que es la encargada de estar conectada a la fuente de alimentacion para que tenga voltaje. La otra 
es la llamada "emitter" que es la que nos permitira, ademas de pasarle la energia que nos 
transmite GND, vincularla al arduino conectandola en una salida analogica para asi, obtener la 
informacion que el sensor detecte.

Para aplicarla al proyecto luego de en el setup configurarla como input, debemos llamar a la 
instruccion "analogRead(PIN_DEL_SENSOR)" para obtener su lectura. Esto nos dara un valor el cual
deberemos de convertir a un valor real y comprensible por lo que debemos de multiplicar ese 
valor por "0.0976".

Finalmente tras finalizar esos pasos, obtendremos la cantidad de luz que hay en el ambiente.


Implementacion del Sensor de luz ambiental en mi proyecto:

Sobre lo que se encarga el mismo es en controlar el contador cuando se cumplan ciertos casos.

 ~~~ C
   // Sensor de luz ambiental
  
  if (luzNueva > 65 && luzNueva < 90) // En caso que el valor de luz ambiental sea mayor a 65 y menor a 90
  {
     mostrarPrimos ? indiceNumeroPrimo-- : numeroEnteroActual--; // Reducir el contador
  }
  else if(luzNueva > 90) // En caso que el valor de luz ambiental sea mayor a 90
  {
    mostrarPrimos ? indiceNumeroPrimo++ : numeroEnteroActual++; // Aumentar el contador
  }  
  // Fin sensor de luz ambiental
  ~~~

Explicacion: 
En el caso de que la luz ambiental tenga un valor de 0, el control 
de aumentar y disminuir el contador sera manual a travez de los botones asignados a esa tarea.

En el caso de que el la luz ambiental tenga un valor mayor a 65 y menor a 90
se comenzara a disminuir el contador.

En el caso de que el la luz ambiental tenga un valor mayor 90
se comenzara a aumentar el contador.

## Modificaciones 4ta parte parcial

Ademas de cambiarles el nombre a las variables correpondientes para que en lugar de hacer referencia
a los numeros primos lo hagan a los numeros multiplos de 5, cambie el codigo de las funciones

  ~~~ C
  int contarNumerosMultiplos(int maximo)
  {
      int contador = 0;
      for (int i = 1; i < maximo; i++){
          bool multiplo = false;

          if(i % 5 == 0)
          {
              multiplo = true;
          }

          if(multiplo)
          {
              contador++;
          }
          }

      return contador;
  }

  int* obtenerNumerosMultiplos(int maximo, int *arraySize)
  {
      int size = contarNumerosMultiplos(maximo);
      int *array = (int*)calloc(size, sizeof(int));

      int arrayIndex = 0;
      for (int i = 1; i < maximo; i++)
      {
          bool multiplo = false;
          if(i % 5 == 0)
          {
              multiplo = true;
          }
          if(multiplo)
          {
              *(array + arrayIndex) = i;
              arrayIndex++;
          }
      }

      *arraySize = size;

      return array;
  }
  ~~~

Los cambios son
En la funcion "contarNumerosMultiplos", ahora recorre
todos los numeros del 1 hasta el maximo ingresado y si ese numero al 
sacarle el resultado del modulo de 5, si ese resultado es 0, que quiere decir
que no hay resto y por lo tanto es divisor, aumenta el contador

En la funcion "obtenerNumerosMultiplos", se aplica la misma logica al momento de guardar
en las direcciones de memoria asignadas al array y escribe en las mismas sus valores 
respectivos
