#Avoid Recursion in Triggers

Cuando estamos desarrollando un requerimiento que involucra Apex triggers, y no se implementa adecuadamente la logica, es posible generar una recursion
en la ejecucion del programa. 

Esta recursion puede derivar en un limite llamado: Maximum trigger depth exceeded, el cual indica que un trigger se puede ejecutar en cadena hasta un total 
de 16 veces. 


