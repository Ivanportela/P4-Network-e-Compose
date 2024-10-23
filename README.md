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

Segue os pasos da guía de iniciación de docker-compose, e explica coas túas palabras os pasos que segues e qué fan

Agora que sabes algo máis de docker-compose, crea un arquivo (ou varios arquivos) de configuración que ó ser lanzados cun docker-compose up, resulten nunha rede docker á que estean conectados 3 contenedores, explica os parámetros do .yaml usado

Busca e proba 4 parámetros e configuracións diferentes que podes incluir no arquivo compose, explica qué fan. (por exemplo diferentes cousas que facer coa opción RUN)
