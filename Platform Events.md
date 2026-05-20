# Platform Events
Un Platform Event es una manera de comunicar diferentes sistemas de forma desacoplada. Esta comunicación se logra a través de eventos. 

Un Platform Event funciona como un “mensaje” que se publica en un bus de eventos. Luego, uno o varios suscriptores (Otros sistemas) escuchan ese mensaje y ejecutan una acción.

Cuando se habla del desacoplo, se refiere puntualmente a que el sistema que publica el evento o mensaje no tiene que conocer en absoluto a los sistemas que están suscritos.

La idea general es: 

1. Se crea la definición del Evento. Esta es la plantilla donde se especifica cómo se va llamar el evento y que campos se pueden especificar a la hora de publicar el mensaje.
2. Un sistema publica el evento
3. Salesforce almacena temporalmente el mensaje en el Event bus
4. Los sistemas suscriptores reciben el evento
5. Cada suscriptor ejecuta su lógica
