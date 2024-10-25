P4 Network e compose.
Docker network:

**1.Crear unha rede en docker**
Para crear unha rede en docker deberemos de utilizar o comando 
```
docker network create <nome da red>
```

No meu caso utilizarei o meu nome polo que a rede tendra o nome de "red_ivan"

**2.Crear dous contenedores unidos a esa rede**

Para crear os contenedores unidos a nosa rede deberemos utilizar o seguinte comando dúas veces para ter os dous:
```
docker run -dit --name <nome do contenedor> --network <nome da rede xa creadea> ubuntu
```
Unha vez utilizado o comando e creado os dous contenedores que necesitamos imos a vincularlo, no meu caso os contenedores chamanse **Alfa** e **Beta** polo que o comando sera o seguinte:
```
docker run -dit --name Alfa --network red_ivan ubuntu
```
Repetiremos o comando co outro contenedor.
```
docker run -dit --name Beta --network red_ivan ubuntu
```

**3.Comprobar que os contenedores están na rede**

Para comprobar que os contenedores atopanse na rede deberemos de utilizar o seguinte comando:
```
docker network inspect red_ivan
```
Esto mostrara toda a nosa rede e poderemos comprobar se os contenedores atopanse ahi.

**4.Comprobar que os contenedores poden verse entre eles**

Para comezar este paso o primeiro e ter iniciados ambos contenedores, para poder abrir a terminal do contenedor desde a terminal de bash utilizamos o comando:
```
docker exec -it Alfa bash
```
Tamen o faremos co outro contenedor.

Podemos ter un problema se docker non esta actualizado ou se non temos o comando ping instalado polo que utilizaremos os seguintes dous comandos para estar seguros disto.
```
apt update
```
```
apt install -y iputils-ping
```
Con isto xa podemos facer ping aos contenedores polo que faremos ping co seguinte comando:
```
ping Beta
```
Si esta todo correcto deberian estar conectados ambos contenedores

**5.Listar os contenedores conectados á rede**

Para lista os contenedores deberemos de utilizar o comando:
```
docker network inspect red_ivan
```
Aparecera un seccion de "Containers" aparecera se estan conectados a rede.

**6.Listar as propiedades da rede**

As propiedades da rede podense comprobar utilizando o mesmo comando **inspect**:
```
docker network inspect red_ivan
```


**7.Crea outra rede**

Para crear outra rede facemos o mesmo que no primer paso, deberemos poner o comando:
```
docker network create <nome da red>
```

O nome da miña segunda rede è "red_ivan2"

**8.Lanza dous contenedores novos conectados a esa nova rede**

Para isto volveremos a utilizar o mesmo que fixemos no paso 2 polo que os comandos cos meus contenedores e rede son:
```
run -dit --name Delta --network red_ivan2 ubuntu
```
e o outro:
```
docker run -dit --name Epsilon --network red_ivan2 ubuntu
```

**9.Comproba as posibles conexións entre os 4 contenedores**

Debremos de facer o mesmo que no paso 4, e dicir actualizaremos e descargaremos o necesario para poder facer ping e nos conectaremos ao primeiro contenedor "alfa".
**Se facemos ping a beta, mostrara o mismo que no paso 4 pero se facemos ping ao contenedor "Delta" esta dara un erro xa que estos dous contenedores atopanse en redes diferentes.**

**O mesmo pasa se nos contectamos ao contenedor "Delta" este podra conectarse co contenedor "Epsilon" pero non podera conectarse cos contenedores "Alfa" e "Beta".**

Docker compose:

**Segue os pasos da guía de iniciación de docker-compose, e explica coas túas palabras os pasos que segues e qué fan**

Imos comezar cos pasos para realizar este apartado:
**Paso 1: Configuración inicial**

Crear un directorio para o proxecto:

    Créase unha carpeta chamada composetest para almacenar os arquivos do proxecto.
    Crear o arquivo app.py:
        Este arquivo contén o código dunha aplicación web en Python usando Flask.
        A aplicación ten un contador que conta cantas veces se accedeu á páxina, almacenando este dato nun servizo Redis.
        Redis é un almacén de datos en memoria que se usará para gardar o número de visitas.
    Crear o arquivo requirements.txt:
        Este arquivo lista as dependencias que necesita a aplicación (flask e redis), as cales se instalarán máis adiante.
    Crear un Dockerfile:
        Especifica como construir a imaxe de Docker para a aplicación.
        Inclúe pasos como copiar os arquivos ao contedor, instalar dependencias, expoñer un porto para a aplicación e configurar o comando para executar a aplicación.

**Paso 2: Definir os servizos nun arquivo Compose**

Crear o arquivo compose.yaml:

    Este arquivo define os servizos que se executarán con Docker Compose.
    Inclúe dous servizos:
        Web: A aplicación web que se constrúe a partir do Dockerfile.
        Redis: Utiliza unha imaxe de Redis preconstruída dende Docker Hub.

**Paso 3: Construír e executar a aplicación con Compose**

Executar docker compose up:

    Con un só comando, créanse e iníciase os servizos definidos en compose.yaml.
    Isto inclúe a creación dunha rede predeterminada para a comunicación entre os servizos.

**Paso 4: Editar o arquivo Compose para usar "Compose Watch"**

Engadir soporte para "watch" no arquivo compose.yaml:

    Isto permite que os cambios nos arquivos se sincronicen automáticamente co contedor, sen necesidade de reiniciar manualmente.

**Paso 5: Volver a construir e executar a aplicación**

Usar docker compose watch ou docker compose up --watch:

    Executa a aplicación e habilita a sincronización automática de arquivos, o que facilita o desenvolvemento.

**Paso 6: Actualizar a aplicación**

Modificar o mensaxe en app.py e gardar os cambios:

    Cando se garda o arquivo, a aplicación actualízase automáticamente no contedor, grazas á sincronización habilitada.

**Paso 7: Dividir os servizos en varios arquivos Compose**

Crear un novo arquivo infra.yaml:

    Mover o servizo Redis a este arquivo para modularizar a configuración.
    Actualízase compose.yaml para incluír o novo arquivo.

**Paso 8: Experimentar con outros comandos**

Correr os servizos en modo "detached" -d:

    Isto permite executar os servizos en segundo plano.
    Usar docker compose ps para ver os servizos en execución.
    Deter os servizos con docker compose stop ou eliminalos con docker compose down.


**Agora que sabes algo máis de docker-compose, crea un arquivo (ou varios arquivos) de configuración que ó ser lanzados cun docker-compose up, resulten nunha rede docker á que estean conectados 3 contenedores, explica os parámetros do .yaml usado**
Versión: Isto indica a versión de Docker Compose que estou utilizando.

Servizos: Esta sección define os diferentes contedores que se executarán. No meu caso, definín tres servizos: web1, web2 e web3.

web1, web2, web3: Cada un destes é un servizo que representa un contedor de Nginx.

imagen: nginx Utilizo a imaxe oficial de Nginx.

container_name: nome do contedor

redes: Indica as redes ás que se conectará o contedor.

ports: Mapea un porto do host ao porto do contedor.

restart: always: Este parámetro indica que o contedor reiniciarase automáticamente se se detén.

volumes: Monta un directorio do host nun directorio do contedor.

redes: Aquí define as redes que usarán os contedores.

mi_red: É o nome da rede que creei. O driver bridge é o predeterminado e permite a comunicación entre contedores na mesma rede.


**Busca e proba 4 parámetros e configuracións diferentes que podes incluir no arquivo compose, explica qué fan. (por exemplo diferentes cousas que facer coa opción RUN)**

Environment: Establece variables de entorno que poden ser utilizadas pola aplicación dentro do contedor.

depends_on: Este parámetro especifica que un servizo depende doutro, garantindo que se inicie na orde correcta.

build: Se necesitas crear unha imaxe personalizada, podes especificar o contexto de construción.

healthcheck: Podes definir un control de saúde para monitorizar o estado do contedor.
