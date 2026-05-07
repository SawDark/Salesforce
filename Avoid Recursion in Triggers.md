# Evitar recursion en Tirggers

Cuando estamos desarrollando un requerimiento que involucra Apex triggers, y no se implementa adecuadamente la logica, es posible generar una recursion
en la ejecucion del programa. 

Esta recursion puede derivar en un limite llamado: `Maximum trigger depth exceeded`, el cual indica que dentro de una misma transaccion, se pueden ejecutar un maximo de 16 triggers en cadena. Es importante tener presente la palabra 'cadena', ya que es realmente lo que causa el error.

##  Ejemplo de No Resurcion 

```apex

trigger AccountTrigger on Account (after update) {
    update contactsToUpdate;      // Contact trigger fires and finishes
    update casesToUpdate;         // Case trigger fires and finishes
    update opportunitiesToUpdate; // Opportunity trigger fires and finishes
}
```
## Ejemplo de Recursion

```apex

Account trigger
  → updates Contact
      → Contact trigger
          → updates Case
              → Case trigger
                  → updates Opportunity
                      → Opportunity trigger
                          → ...
```

## Solucion

La solucion consiste en usar `variables estaticas`, ya sea una bandera, o un Set que almacene el Id de los registros ya procesados. Si bien una variable estatica hace referencia a un atributo que pertenece a la clase en vez de a una instancia, la clave aqui es que las variables estaticas en Salesforce perduran durante toda la transaccion, a diferencia de una variable normal, la cual se vuelve a crear en cada ejecuion del Trigger. 

### Variable normal

```apex
trigger AccountTrigger on Account (after update) {
    Boolean alreadyRun = false;

    if (!alreadyRun) {
        alreadyRun = true;
        // logic
    }
}
```
### Variable estatica
