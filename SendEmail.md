# Formas de enviar notificaciones de correo en Salesforce

## Flows

Poder enviar notificaciones vía correo electrónico es una tarea que frecuentemente solicitan los clientes. Aquí específico las diferentes formas en las que esta actividad se puede realizar.

### Generando el contenido directamente desde el Flow

Esta opción te permite crear tanto el **cuerpo** como el **asunto** del correo en el mismo Flow.

Para ello necesitamos seleccionar el elemento **action** y buscar la acción **Send Email**. Una vez seleccionada, podemos comenzar a llenar toda la información correspondiente a la notificación, la cual incluye: Destinatario, CC, BCC, el Remitente, el Asunto, el Cuerpo, y otras opciones específicas que se detallan aquí.

<img width="726" height="616" alt="image" src="https://github.com/user-attachments/assets/bc932e6f-b3f9-419c-8ad9-dda87a7c0914" /><br/>

<img width="630" height="500" alt="image" src="https://github.com/user-attachments/assets/8ade2bb0-2823-4087-8304-db43309d3779" /><br/>

Para el apartado del **destinatario** se debe seleccionar una lista separada por comas, o una colección de direcciones de correo, aun cuando solo se vaya a enviar a una persona.  

<img width="644" height="520" alt="image" src="https://github.com/user-attachments/assets/139afac5-6949-4746-87ab-8c9279f0adec" /><br/>

Para especificar el remitente se cuenta con tres opciones: El usuario que disparó la notificación, el Default Workflow user que esté configurado (Setup --> Process Automation Settings), o un correo corporativo creado precisamente para este propósito (Setup --> Organization-Wide Addresses). 

<img width="944" height="362" alt="image" src="https://github.com/user-attachments/assets/3c5ce3c6-e1d6-4e81-a1b1-a90ace1c8647" /><br/>

El **asunto** y el **cuerpo** del correo se pueden configurar directamente desde esta vetana. 

<img width="758" height="701" alt="image" src="https://github.com/user-attachments/assets/16220a03-d0cc-4d4d-b44c-3d9abd8244d9" /><br/>

Tambien es posible usar recursos para estos dos apartados. Una variable/constante de tipo Texto o una Formula, para el asunto, y un Text template para el cuerpo.

<img width="1386" height="620" alt="image" src="https://github.com/user-attachments/assets/f775d3b4-c70b-44cf-89ed-d0b99b5c7399" />

