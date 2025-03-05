# Actividad-Evaluable-Diagrama-de-estados-9-3-2025

## Diagrama de estados 
```mermaid
stateDiagram-v2
    state "Esperando Tarjeta" as espera_tarjeta
    state "Validando Tarjeta" as validando_tarjeta
    state "Codigo Pin" as codigo_pin
    state "Valindando Pin" as validando_pin
    state "Fin Proceso" as selecion_proceso
    state "Retencion de tarjeta" as retencion_tarjeta
    state "Elegir Transaccion" as elegir_transaccion
    state "Realizando Transaccion" as realizando_transaccion
    state "Fin Transaccion" as fin_transaccion
    state "Cerrado de Sesion" as cerrando_sesion
    
    [*] --> espera_tarjeta
    
    espera_tarjeta--> validando_tarjeta : introducen la tarjeta
    validando_tarjeta --> espera_tarjeta: tarjeta rechazada
    validando_tarjeta --> codigo_pin: tarjeta valida
    
    codigo_pin--> selecion_proceso : introduce un codigo pin
    
    selecion_proceso --> codigo_pin: Ha cancelado Proceso
    selecion_proceso --> validando_pin:No desea cancelar el proceso
    
    validando_pin--> codigo_pin: Reintento\nMientras que  Intento < 3 
    validando_pin --> elegir_transaccion: Pin correcto
    
    elegir_transaccion--> realizando_transaccion : Ha elegido hacer una operacion
    
    realizando_transaccion --> fin_transaccion: Se esta operando la operacion
    fin_transaccion --> elegir_transaccion : Ha escogido hacer otra operacion
    
    elegir_transaccion--> cerrando_sesion :Vuelve a elegir operacion 
    
    
    cerrando_sesion -->  [*]
    codigo_pin--> retencion_tarjeta : Supero el limite de intentos permitidos
    
    retencion_tarjeta --> [*]
```
## Explicacion

El cajero esta en un estado de esperando tarjeta hasta que recibe una por la cual este comprobara si es una tarjeta valida,en caso negativo la tarjeta sera rechazada y volvera a su estado de espera y en caso positivo el cajero solicitara un codigo pin.
En el proceso de codigo pin antes de su comprobacion se decidira si se quiere cancelar el proceso y solo si este proceso se decide finalizar volvera otra vez a solicitar el pin sin,en el otro suceso se comprobara si el codigo pin introducido es el correcto,
en caso de que no lo sea se le devolvera otra vez al estado de introduccion del pin comprobando si el numero de intentos fallidos es inferior a 3,en todo caso si se llegase a esa cantidad el cajero se quedara la tarjeta y finalizara proceso.En caso de que el 
codigo pin introducido sea valido el cajero le llevara a un menu el cual tiene varias transacciones que puede realizar el usuario que tras su eleccion se realizara su proceso y solo si acaba se le devolvera al estado de eleccion de proceso hasta que el usuario
decida cerrar sesion asi finalizando el cajero
