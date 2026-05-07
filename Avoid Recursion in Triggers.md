# Evitar recursion en Tirggers

Cuando estamos desarrollando un requerimiento que involucra Apex triggers, y no se implementa adecuadamente la logica, es posible generar una recursion
en la ejecucion del programa. 

Esta recursion puede derivar en un limite llamado: Maximum trigger depth exceeded, el cual indica que dentro de una misma transaccion, se pueden ejecutar un maximo de 16 triggers en cadena. Es importante tener presente la palabra 'cadena', ya que es realmente lo que causa el error.

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


