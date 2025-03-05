

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
    
    elegir_transaccion--> cerrando_sesion :Ha escogido cerrar sesion 
    
    
    cerrando_sesion -->  <<end>> 
    codigo_pin--> retencion_tarjeta : Supero el limite de intentos permitidos
    
    retencion_tarjeta --> <<end>>
