# Platform Events
Un Platform Event es una manera de comunicar diferentes sistemas de forma `desacoplada`. Esta comunicación se logra a través de eventos. 

Un Platform Event funciona como un “mensaje” que se publica en un bus de eventos. Luego, uno o varios suscriptores (Otros sistemas) escuchan ese mensaje y ejecutan una acción.

Cuando se habla del desacoplo, se refiere puntualmente a que el sistema que publica el evento o mensaje no tiene que conocer en absoluto a los sistemas que están suscritos.

La idea general es: 

1. Se crea la definición del Evento. Esta es la plantilla donde se especifica cómo se va llamar el evento y que campos se pueden especificar a la hora de publicar el mensaje.
2. Un sistema publica el evento
3. Salesforce almacena temporalmente el mensaje en el `Event bus`
4. Los sistemas suscriptores reciben el evento
5. Cada suscriptor ejecuta su lógica


## Crear un Platform Event

Para crear un Platform event es necesario ir a: `Setup → Platform Events → New Platform Event`

<img width="2512" height="784" alt="image" src="https://github.com/user-attachments/assets/dbf29ef7-3e3a-4227-9933-af577b6a2874" />

La creación es similar a la de un Objeto custom, solo que los tipos de campo que se pueden seleccionar a la hora de crear un Campo personalizado son más limitados. 

<img width="1291" height="732" alt="image" src="https://github.com/user-attachments/assets/fb313101-ac8f-466c-a0d5-0511ee698209" />

Aquí es importante aclarar que por defecto el Platform event ya cuenta con unos pocos campos estándar, dentro de los que se encuentran el `EventUuid` que no es más que el identificador único del mensaje que se publica, y el `ReplayId`, el cual representa la posición del mensaje dentro del eventbus, y que los suscriptores como Sistemas externos o un LWC pueden usar para ‘procesar’ eventos que por algún fallo no fueron escuchados en primera instancia.

> [!NOTE]
> El sufijo de los Platform Event es  `__e`

> [!NOTE]
> El eventbus, que no es más que el canal de Salesforce donde se publican y se distribuyen los eventos, puede retener los mensajes hasta un máximo de 72 horas.

## Publicar un evento

Publicar un evento se puede realizar de distintas maneras dependiendo de donde se esté haciendo, aquí se adjuntan algunas de las opciones:

### Apex

```apex

TestEvent__e eventMessage = new TestEvent__e(
	Data__c = 'Se ha generado un Event Platform'
);

Database.SaveResult result = EventBus.publish(eventMessage);
```

### Flow

Se usa el elemento Create como si se fuera a crear un registo normal de cualquier objeto. 

<img width="1097" height="842" alt="image" src="https://github.com/user-attachments/assets/15fa0a18-afae-4513-9d24-c8401f673c1b" />

### LWC


