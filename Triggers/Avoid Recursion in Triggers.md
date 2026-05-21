# Evitar recursión en Triggers

Cuando estamos desarrollando un requerimiento que involucra Apex triggers, y no se implementa adecuadamente la lógica, es posible generar una recursión en la ejecución del programa. 

Esta recursión puede derivar en un límite llamado: `Maximum trigger depth exceeded`, el cual indica que dentro de una misma transacción, se pueden ejecutar un máximo de 16 triggers en cadena. Es importante tener presente la palabra 'cadena', ya que es realmente lo que causa el error.

##  Ejemplo de No Recursión

```apex

trigger AccountTrigger on Account (after update) {
    update contactsToUpdate;      // Contact trigger fires and finishes
    update casesToUpdate;         // Case trigger fires and finishes
    update opportunitiesToUpdate; // Opportunity trigger fires and finishes
}
```
## Ejemplo de Recursión

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

## Solución

La solución consiste en usar `variables estáticas`, ya sea una bandera, o un Set que almacena el Id de los registros procesados. Si bien una variable estática hace referencia a un atributo que pertenece a la clase en vez de a una instancia, la clave aquí es que las variables estáticas en Salesforce perduran durante toda la transacción, a diferencia de una variable normal, la cual se vuelve a crear en cada ejecución del Trigger.

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
En este ejemplo, la bandera siempre volverá a su valor original con cada ejecución del Trigger, por lo que no estaría cumpliendo la función que le corresponde. 

### Variable estatica

```apex

public class TriggerControl {
    public static Boolean alreadyRun = false;
}
```

```apex

trigger AccountTrigger on Account (after update) {
    if (TriggerControl.alreadyRun) {
        return;
    }

    TriggerControl.alreadyRun = true;

    // trigger logic here
}
```
Con la variable estática, la bandera mantendrá su valor durante toda la transacción, lo cual es perfecto para controlar la ejecución en cadena. Sin embargo, hay que tener presente un par de cosas, recordemos que un Trigger es básicamente un bloque de código, prácticamente una ventana anónima, es decir, técnicamente no se considera una clase, por lo que no tiene sentido crear variables estáticas allí, no funcionan correctamente. Practicamente funcionan como una variable normal.  

Adicionalmente, tampoco es posible crear una subclase dentro del Trigger y agregar la variable allí, por dos razones, la primera es porque se aconseja que el Trigger sea `logic-less`, lo que indica que debe tener el menor código posible,  y lo segundo, es porque no es viable crear variables de este tipo dentro de subclases, solo se pueden definir en clases principales.

## Solución más completa.

Aunque una bandera estática funciona bien, es importante tener presente que puede bloquear la ejecución de registros válidos cuando ocurre una transacción en lotes. 

Salesforce menciona que una operación dml que contiene más de 200 registros, es dividida en `lotes/chunks`, de 200 registros cada uno. Esto quiere decir que si yo actualizo 400 registros en una misma operación a la base de datos, el Trigger se ejecuta 2 veces, uno por cada lote. 

No obstante, el error está en creer que cada lote siempre representa una nueva ventana de ejecución que resetea los límites y las variables estáticas. Pero esto depende del lugar desde donde se hizo la operación. 

<img width="1522" height="536" alt="image" src="https://github.com/user-attachments/assets/13c9b8af-d9bb-4095-aa3f-dc86f876673a" />

Para abarcar los escenarios en donde los chunks ocurren en una misma transacción, lo mejor es usar un `Set estático` cuyo propósito es almacenar el Id de los registros que ya han sido procesados, dando luz verde a los que aún no se han manipulado. 

```apex

public class TriggerControl {
    public static Set<Id> processedAccountIds = new Set<Id>();
}
```

```apex

trigger AccountTrigger on Account (after update) {
   List<Account> accountsToProcess = new List<Account>();
   
    for (Account acc : Trigger.new) {
        if (!TriggerControl.processedAccountIds.contains(acc.Id)) {
            accountsToProcess.add(acc);
            TriggerControl.processedAccountIds.add(acc.Id);
        }
    }

    // lógica solo para cuentas no procesadas
}
```

> [!TIP]
> A pesar de que esta es la base para comprender cómo controlar las recursiones en un Trigger Apex, hoy por hoy existen soluciones mucho más completas que involucran patrones de diseño, y que recomiendo altamente usarlas. [Apex Hours Differents Frameworks](https://www.apexhours.com/trigger-framework-in-salesforce/), [GitHub Trigger Framework](https://github.com/dschach/salesforce-trigger-framework)

