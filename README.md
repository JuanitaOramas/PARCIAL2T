## Escuela Colombiana de Ingeniería

### PDSW – Procesos de desarrollo de Software
### Parcial Segundo Tercio

#### Integrantes: Juanita Oramas y Wilson Delgado 

**IMPORTANTE**

* Deseable Trabajar en Linux (para evitar problemas con las instrucciones finales).
* Se puede consultar en la Web: APIs/Documentación de lenguaje y frameworks (Primefaces, Guice, MyBatis, etc), y enunciados de los laboratorios (se pueden revisar los fuentes incluidos con los dichos enunciados).
* No se permite: Usar memorias USB, acceder a redes sociales, clientes de correo, o sistemas de almacenamiento en la nube (Google Drive, DropBox, etc). El uso de éstos implicará anulación.
* Clone el proyecto con GIT, NO lo descargue directamente.
* NO modifique lo indicado en consultaPaciente.xhtml.
* El filtrado y ordenamiento de los datos DEBE realizarse en el motor de base de datos, a través del uso de SQL. Consultar todos los datos y filtrarlos en el servidor de aplicaciones -que es supremamente INEFICIENTE- se evaluará como INCORRECTO.


Se le han dado las fuentes de un avance parcial de una plataforma de consultas de pacientes de una IPS en línea. En esta plataforma los médicos podrán registrar y buscar pacientes así como buscar y registrar las consultas.
Adicionalmente, la secretaria de salud puede hacer búsquedas para control epidemiológico.

Para el Sprint en curso, se han seleccionado las siguientes historias de usuario del Backlog de producto:

Recuerde que en el formato XML no se puede utilizar '<' y '>', por ejemplo al realizar comparaciones, 
 utilice '&amp;lt;' o '&amp;gt;' respectivamente. 

## Historia de usuario #1

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  > **Como** Usuario de la plataforma de consultas médicas
  >
  > **Quiero** Poder consultar un paciente a partir de su número y tipo de identificación.
  >
  > **Para** Poder hacer una revisión de las consultas realizadas por un paciente cuyo documento ya conozco, y así evitar la búsqueda por el nombre del paciente.
  >
  > **Criterio de aceptación:** Se debe mostrar la fecha de nacimiento del paciente, su nombre, y cada una de las consultas realizadas. Las consultas deben estar organizadas de la más reciente (mostrados arriba) a la más antígua, y deben mostrar la fecha y el resúmen.

## Historia de usuario #2

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  > **Como** Usuario de la secretaría de salud de la plataforma
  >
  > **Quiero** Tener un reporte de las consultas de los menores de edad (menóres de 18 años) en las que en el resúmen se encuentren enfermedades contagiosas.
  >
  > **Para** Conocer con rapidez qué pacientes debo revisar y tomar medidas al respecto.
  >
  > **Criterio de aceptación:** El reporte NO debe requerir entrar parámetro alguno. Se considerán como enfermedades contagiosas: 'hepatitis' y 'varicela'. El reporte sólo debe contener el número y tipo de identificación  del paciente y la fecha de nacimiento, ordenados por edad de mayor a menor.
  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

El modelo de base de datos y de clases asociados a la implementación parcial son los siguientes:

![](./img/Diagram.png)

![](./img/Model.png)

A partir de la aplicación base suministrada, debe realizar lo siguiente:

Dado un número y tipo de identificación de un paciente, mostrar el paciente y las consultas que ha realizado esde paciente.

Mostrar los pacientes menores de edad que en sus consultas se encuentren las enfermedades: hepatitis o varicela.


1. (20%) A partir de la especificación hecha en los métodos
    *consultarPacientesPorId* y *consultarMenoresConEnfermedadContagiosa* de la fachada de
    servicios (la parte lógica de la aplicación), implemente solo una prueba (la que considere más importante para validar las especificaciones y los criterios de aceptación). Siga el esquema usado en ServicesJUnitTest para poblar la base de datos volátil y verificar el comportamiento de las operaciones de la lógica.
    
    Se realizan las pruebas para validar el correcto funcionamiento:

Primero se realizó el test, en el cual se validó si la identificación, el tipo de identificación y el nombre coinciden. 

![img.png](img/img.png)

![img_1.png](img/img_1.png)

2. (40%) Implemente la historia de usuario #1, agregando todo lo que haga falta en la capa de presentación, lógica y de persistencia. La vista debe implementarse en consultaPaciente.xhtml.

Para esta historia de usuario se implementa el método *consultarPacientesPorId*: 
![img_2.png](img/img_2.png)

![](img/img_15.png)

Se realiza la implementación del método *load* de la interfaz del DAO:

![img_3.png](img/img_3.png)

En la interfaz de *PacienteMapper* se añade el método *getPaciente*, el cual va a ser usado por el método *load* del DAO *(MyBatisDAOPaciente)*:
![img_4.png](img/img_4.png)

Definimos un resultmap en el cual, se define para la clase *Paciente*, que propiedades estarán asociadas para cada atributo:
![img_5.png](img/img_5.png)

Se define la consulta según la historia de *usuario 1*:

> **Como** Usuario de la plataforma de consultas médicas
>
> **Quiero** Poder consultar un paciente a partir de su número y tipo de identificación.
>
> **Para** Poder hacer una revisión de las consultas realizadas por un paciente cuyo documento ya conozco, y así evitar la búsqueda por el nombre del paciente.
>
> **Criterio de aceptación:** Se debe mostrar la fecha de nacimiento del paciente, su nombre, y cada una de las consultas realizadas. Las consultas deben estar organizadas de la más reciente (mostrados arriba) a la más antígua, y deben mostrar la fecha y el resúmen.


![img_6.png](img/img_6.png)

Se define un bean llamado *PacientesBean* que permitirá la comunicación con la capa de presentación (la cual llamara un método *getData* que está en este bean):

![img_7.png](img/img_7.png)

En la capa de presentación se muestra la consulta del paciente según id, en forma de tabla:
![img_17.png](img/img_17.png)

En presentación, verificando el correcto funcionamiento:

![img_16.png](img/img_16.png)

3.  (40%)Implemente la historia de usuario #2, agregando todo lo que haga falta en la capa de presentación, lógica y de persistencia. La vista debe implementarse en consultarMenoresEnfermedadContagiosa.xhtml.

Para esta historia de usuario se implementa el método consultarMenoresConEnfermedadContagiosa:

![img_8.png](img/img_8.png)

Se realiza la implementación del método *loadMenoresConEnfermedad* de la interfaz del DAO, que va a ser usada por el método de *consultarMenoresConEnfermedadContagiosa*:

![img_9.png](img/img_9.png)

En la interfaz de *PacienteMapper* se añade el método *getPacientesEnfermosMenores*, el cual va a ser usado por el método *loadMenoresConEnfermedad* del DAO *(MyBatisDAOPaciente)*:

![img_10.png](img/img_10.png)

Con el bean llamado PacientesBean se define un método que permitirá la comunicación con la capa de presentación(la cual llamara un método que está en este bean):

![img_12.png](img/img_12.png)


Se define la consulta según la historia de usuario 2:

> **Como** Usuario de la secretaría de salud de la plataforma
>
> **Quiero** Tener un reporte de las consultas de los menores de edad (menóres de 18 años) en las que en el resúmen se encuentren enfermedades contagiosas.
>
> **Para** Conocer con rapidez qué pacientes debo revisar y tomar medidas al respecto.
>
> **Criterio de aceptación:** El reporte NO debe requerir entrar parámetro alguno. Se considerán como enfermedades contagiosas: 'hepatitis' y 'varicela'. El reporte sólo debe contener el número y tipo de identificación  del paciente y la fecha de nacimiento, ordenados por edad de mayor a menor.




![img_11.png](img/img_11.png)

En la capa de presentación se muestra la consulta con pacientes menores de edad, que en su descripción se encuentran enfermos de varicela o hepatitis:

![img_13.png](img/img_13.png)

Formulario con los menores de edad en orden de edad (de mayor a menor) , organizados en una tabla para mejor visualización:

![img_14.png](img/img_14.png)