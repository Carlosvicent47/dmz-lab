#DMZ-LAB

Se pretende realizar un laboratorio con 3 subredes:
    * Red Interna
    * Zona demilitalizada
    * Red externa (Simula la conexión a internet)

En este entorno se podrá simular un escenario real de una organización que necesita un servidor web accesible desde internet sin comprometer la seguridad de la red interna. Para ello se realizará una zona desmilitarizada segura utilizando un router Cisco y aplicando reglas NAT y ACLs, para controlar el tráfico de las redes.

La zona desmilitarizada permite conexión externa, a través de un enmascaramiento de IP, pero no permite ningún tipo de comunicación desde el servidor a la red interna. Pero sí que permite conexión de la red interna al servidor.


#Contenidos del Repositorio

* /evidencias --> Se encuentras las capturas de los pasos realizados y de las evidencias.
* DMZ_Project.pka --> El archivo realizado con toda la configuración