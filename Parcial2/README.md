### Examen 2
**Universidad ICESI**  
**Curso:** Sistemas Operativos  
**Docente:** Daniel Barragán C.  
**Tema:** Namespaces, CGroups, LXC
**Correo:** daniel.barragan at correo.icesi.edu.co

**Nombre:** Julián David González Jiménez  
**Código:** A00315288  
**URL:** https://github.com/juliand7gj/so-exam2/tree/A00315288/Parcial2     

### Objetivos
* Comprender los fundamentos que dan origen a las tecnologías de contenedores virtuales
* Conocer y emplear funcionalidades del sistema operativo para asignar recursos a procesos
* Conocer y emplear capacidades de CentOS7 para la virtualización

### Prerrequisitos
* Virtualbox o WMWare
* Máquina virtual con sistema operativo CentOS7

### Descripción
El segundo parcial del curso sistemas operativos trata sobre el manejo de namespaces, cgroups y virtualización por medio de LXC/LXD

### Actividades
1. Incluir nombre, código (5%)
2. Ortografía y redacción cuando sea necesario (5%)
3. Realice una prueba de concepto empleando systemd y el recurso de control CPUQuota teniendo en cuenta los requerimientos que se describen a cotinuación. Incluya evidencias del funcionamiento de lo solicitado (30%):
 * Las pruebas se realizaran sobre un solo núcleo de la CPU
 * Se deben ejecutar dos procesos
 * Cada proceso debe poder acceder solo al 50% de la CPU
 * Cuando uno de los procesos se cancela, el que continua ejecutándose no debe acceder a mas del 50% de la CPU
4.  Realice una prueba de concepto empleando systemd y el recurso de control CPUShares teniendo en cuenta los requerimientos que se describen a continuación. Incluya evidencias del funcionamiento de lo solicitado (30%):
 * Las pruebas se realizaran sobre un solo núcleo de la CPU
 * Se deben ejecutar dos procesos
 * Uno de los procesos tendrá el 25% de la CPU mientras que el otro tendrá el 75% de la CPU
 * Cuando uno de los procesos se cancela, el que continua ejecutándose debe poder llegar al 100% de la CPU
5. Por medio de las evidencias obtenidas en los puntos anteriores y de fuentes de consulta en Internet, elabore las definiciones para los grupos de control CPUQuota y CPUShares, además concluya acerca de cuando es preferible usar un recurso de control sobre otro (20%)
6. El informe debe ser entregado en formato pdf a través del moodle y el informe en formato README.md debe ser subido a un repositorio de github. El repositorio de github debe ser un fork de https://github.com/ICESI-Training/so-exam2 y para la entrega deberá hacer un Pull Request (PR) respetando la estructura definida. El código fuente y la url de github deben incluirse en el informe (10%)  

### Desarrollo

3. 
* Las pruebas se realizaran sobre un solo núcleo de la CPU

![][1]

* Se deben ejecutar dos procesos  
Para este punto, cree un proceso el cual se va a correr dos veces en la maquina por medio de diferentes servicios.

![][2]  

Se le deben dar los permisos necesisarios a este proceso. 

* Cada proceso debe poder acceder solo al 50% de la CPU    

Se crean los dos servicios en el cual cada uno podrá acceder al 50% de la CPU.

![][3]  

Ahora, se debe ejecutar el comando: systemctl daemon-reload, y por siguiente, se deben iniciar los servicios que fueron creados  anteriormente.    

![][4]  

Se puede observar que los dos servicios están activos y corriendo.  
Con el comando top se pueden ver los procesos activos, aquí vamos a ver los dos servicios con los respectivos porcentajes de CPU que se les otorgo.    

![][5]    

* Cuando uno de los procesos se cancela, el que continua ejecutándose no debe acceder a mas del 50% de la CPU  

Para observar, que si se cancela uno de los dos procesos, el que queda activo no debe acceder a mas del 50% de la CPU, se va a matar el proceso 2488, por lo tanto, el proceso 2510 debe quedar con 50% de la CPU.  

![][6]  

4. Para este punto, se van a utilizar los mismos servicios del punto anterior.  

* Las pruebas se realizaran sobre un solo núcleo de la CPU. Esto ya se logró en el punto anterior.  

* Se deben ejecutar dos procesos. Se utilizará el proceso del punto anterior.  

* Uno de los procesos tendrá el 25% de la CPU mientras que el otro tendrá el 75% de la CPU  
Solo se cambiará la línea del CPUQuotas del .service, por CPUShares y para darle a cada uno su respectivo porcentaje, se hace con una relacion de 1 a mil, es decir, si se le quiere dar el 25% se debe colocar 250, y si se quiere darle el 75% se coloca 750.  

![][7]  

Ahora, se inician los procesos nuevamente para que queden con la nueva configuración asignada.    

![][8]  

* Cuando uno de los procesos se cancela, el que continua ejecutándose debe poder llegar al 100% de la CPU  

Se puede observar, que el uno de los servicios tiene el 25% de la CPU y el otro el 75%.  

![][9]   

Ahora se matará el proceso 2510 y el proceso 2542 quedá con el 100% de la CPU.  

![][10]  

5. 
* CPUQuota: Toma un valor porcentual, con el sufijo "%". El porcentaje especifica cuánto tiempo de CPU la unidad debe obtener al máximo, en relación con el tiempo total de CPU disponible en una CPU.  

* CPUShares: Asigna el peso compartido de tiempo de CPU especificado a los procesos ejecutados. Estas opciones toman un valor entero y controlan el atributo del grupo de control "cpu.shares". El rango permitido es de 2 a 262144. El valor predeterminado es 1024.  

Los dos grupos de control trabajan de forma diferente, el CPUQuota intenta limitar el uso de CPU, dandole el limite que puede tener, mientras que CPUShares establece el comportamiento de cada proceso con respecto a los recursos cuando los deben compartir con otros procesos. Por estos motivos, dependiendo de las ocaciones es mejor usar uno o el otro. Los casos son los siguientes: cuando se quiere que un proceso tenga un porcentaje de CPU fijo sin importar los demás procesos, se debe utilizar CPUQuotas. Cuando se quiere darle pesos indicados a procesos para que trabajen entre si, dando importancia cuando se cancela o se agregan mas, se debe utilizar CPUShares.  

### Referencias
https://github.com/ICESI/so-containers  
https://www.freedesktop.org/software/systemd/man/systemd.resource-control.html

[1]: 1nucleo.JPG
[2]: crearProceso.JPG
[3]: crearServicio2.JPG
[4]: status.JPG
[5]: top.JPG
[6]: topkill2488.JPG
[7]: catshares.JPG
[8]: starthares.JPG
[9]: topshares.JPG
[10]: top2510shares.JPG

