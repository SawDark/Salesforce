# Variable RecordId en Flows

Los Flujos en Salesforce permiten configurar Parámetros de **entrada** y **salida**. Solo basta con crear una Variable y marcarla como corresponde. 

![image](https://github.com/user-attachments/assets/50947d0d-1dbc-48eb-b59e-1af9bdefa13f)

De esta manera, cada vez que yo invoque un Flujo, puedo especificarle los Parámetros de entrada. 

Sin embargo, hay un parámetro especial que puedo crear y que solo sirve si el Flujo se está llamando desde una Página de registro. Esa variable se tiene que llamar estrictamente **recordId**.

Dependiendo el Tipo de dato que seleccione al momento de su creación, puede representar el **Id** del registro donde estoy actualmente, o el **Registro entero**. 

![image](https://github.com/user-attachments/assets/03198223-139e-45fa-8b83-83a6603d6d45)

