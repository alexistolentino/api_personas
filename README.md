# API Personas

Uso de FastAPI y MariaDB para generar una API REST con las siguientes funciones: insertar, borrar, actualizar, buscar y listar registros de una tabla.

## 1. Configurar codespace

1.1 Actualizar versiones de librerias y paquetes instalables

````shell
$ sudo apt-get update
````

1.2 Instalar MariaDB

Para este proyecto se utilizara MariaDB como servidor de base de datos.

````shell
$ sudo apt-get install mariadb-server -y
````

1.3 Detener el servidor

En especifico con codespaces de GitHub es necesario detener el servicio de mysql para configurarlo y poder acceder a el.

````shell
$ sudo /etc/init.d/mysql stop
````

1.4 Iniciar el servidor

Hay que reiniciar el servicio de mysql en modo seguro para poder iniciar sesión con el usuario root (default)

````shell
$ sudo mysqld_safe --skip-grant-tables &
````

1.5 Conectando con el servidor MySQL

Iniciar una sesión dentro de mysql

````shell
$ mysql -u root
````

1.5 Configuración segura del servidor de base de datos

El servidor de base de datos en este momento no tiene contraseña para el usuario root, por lo que desde el shell se ejecuta el siguiente comando.

````shell
$ mysql_secure_installation
````

1.7 Conecarse al servidor después de configurarlo

Iniciar una sesión dentro de mysql utilizando la nueva contraseña que se establecio.

````shell
$ sudo mysql -u root -p
````

1.8 Ejecutar un script en MariaDB

El script contendrá la creación de la base de datos, creación de las tablas entre otros pasos, revisar el **punto 2**

````shell
MariaDB [(none)]> source sql/db_agenda.sql;
````

NOTA: el script crea un nuevo usuario de nombre **user** con el passoword **1234**, y le asigna todos los privilegios sobre la base de datos **db_agenda**, este será el usuario que estar utilizado para trabajar con la base de datos.

+ **host**: localhost
+ **user**: user
+ **password**: 1234
+ **port**: 3306
+ **dbname**: db_agenda

1.9 Salir de la MariaDB

Para salir del shell de MariaDB se utiliza exit

````shell
MariaDB [(none)]> exit;
````

## 2. Script para crear la base de datos

Para este ejercicio se tendrá una base de datos con una sola tabla.

2.1 Diccionario de datos de la tabla **personas**.

La descripción de los campos de la tabla **personas** permite tener el contexto de lo que se va a realizar.

|Atributos|Campo|Tipo de dato|Descripción|
| -- | -- | -- | -- |
| PK | id_persona | int | Identificador de la persona |
| - | nombre | varchar(50) | Nombre de la persona |
| - | primer_apellido | varchar(50) | Primer apellido de la persona |
| - | segundo_apellido | varchar (50) | Segundo apellido de la persona |
| - | email | varchar(100) |  Email de la persona |
| - | telefono | varchar(10) | Teléfono de la persona |

2.2 Script para crear la base de datos

* Crear la base de datos **db_agenda**.
* Crear la tabla **personas**.
* Insertar 2 registros en la tabla **personas**.

````sql
CREATE DATABASE db_agenda;

USE  db_agenda;

CREATE TABLE personas(
    id_persona int AUTO_INCREMENT PRIMARY KEY,
    nombre varchar(50) NOT NULL,
    primer_apellido varchar(50) NOT NULL,
    segundo_apellido varchar(50) NOT NULL,
    email varchar(100) NOT NULL,
    telefono varchar(10) NOT NULL
);

INSERT INTO personas (nombre,primer_apellido,segundo_apellido,email,telefono)
VALUES 
("Dejah", "Thoris", "Barsoon","dejah@email.com","1234567890"),
("John", "Carter", "Earth", "john@email.com","2345678901");

SELECT * FROM personas;

CREATE USER 'user'@'localhost' IDENTIFIED BY '1234';
GRANT ALL PRIVILEGES ON db_agenda.* TO 'user'@'localhost';
FLUSH PRIVILEGES;

SELECT "Usuario creado";
````

2.3 Crear la base de datos desde MariaDB shell

````shell
MariaDB [(none)]> source db_agenda.sql
````

## 3. Ambiente virtual

3.1 Crear el ambiente virtual

El ambiente virutal permite tener un espacio aislado donde se instalarán unicamente las librias necesarias, lo que permite evitar conflictos de versiones de librerias con otros proyectos.

````shell
$  python3 -m venv venv
````

3.2 Iniciar el ambiente virtual

Una vez creado  el ambiente virtual se debe inicializar con el siguiente comando.

````shell
$ source venv/bin/activate
````
Para saber si ya se activo el ambiente virtual debe aparacer (venv) en el shell, como se muestra en el siguiente ejemplo.

````shell
(venv)$
````
3.3 Actualizar PIP

Antes de instalar liberias es recomendable actualizar pip, esto permite que se descarguen las últimas versiones de las librerías.

````shell
(venv)$ pip install --upgrade pip
````

3.4 Instalar las librerias

Para este proyecto se utilizará [FastAPI](https://fastapi.tiangolo.com/#requirements)

````shell
(venv)$ pip install fastapi[standard]
````

3.5 Generar el archivo requirements.txt

Una vez que se instalaron las librerias se puede generar el archivo requirements.txt, este archivo permite llevar un control de las librerias instaladas y las versiones uitlizadas.

````shell
(venv)$ pip freeze > requirements.txt
````

En este caso estás son las librerias que se instalaron con la version **fastapi==0.115.3**

````shell
annotated-types==0.7.0
anyio==4.6.2.post1
certifi==2024.8.30
click==8.1.7
dnspython==2.7.0
email_validator==2.2.0
fastapi==0.115.3
fastapi-cli==0.0.5
h11==0.14.0
httpcore==1.0.6
httptools==0.6.4
httpx==0.27.2
idna==3.10
Jinja2==3.1.4
markdown-it-py==3.0.0
MarkupSafe==3.0.2
mdurl==0.1.2
pydantic==2.9.2
pydantic_core==2.23.4
Pygments==2.18.0
python-dotenv==1.0.1
python-multipart==0.0.12
PyYAML==6.0.2
rich==13.9.3
shellingham==1.5.4
sniffio==1.3.1
starlette==0.41.0
typer==0.12.5
typing_extensions==4.12.2
uvicorn==0.32.0
uvloop==0.21.0
watchfiles==0.24.0
websockets==13.1
````

3.6 Salir del ambiente virtual

Cuando ya no se requiera utilzar el ambiente virtual se puede salir con el siguiente comando, y cuando se requiera utilizarlo nuevamente se usa el comando del paso 3.2

````shell
deactivate
````
# Documentacion del GET 

| **Punto** | **Nombre**       | **Descripción**                                                                                       |
|-----------|------------------|-------------------------------------------------------------------------------------------------------|
| 1         | **Description**   | Regresa todos los registros.                                                                          |
| 2         | **Summary**       | Muestra todos los registros de la tabla personas.                                                   |
| 3         | **Method**        | GET                                                                                                 |
| 4         | **Endpoint**      | `http://localhost:8000/v1/personas`                                                                   |
| 5         | **Authentication**| No aplica (NA)                                                                                        |
| 6         | **Query Param**   | No aplica (NA)                                                                                        |
| 7         | **Path Param**    | No aplica (NA)                                                                                        |
| 8         | **Data**          | No aplica (NA)                                                                                        |
| 9         | **Status Code**   | 202 (Aceptado)                                                                                     |
| 10        | **Response Type** | Application/json                                                                                   |
| 11        | **Response**      | {"personas": [{"id_persona": " ", "nombre": " ", "primer_apellido": " ", "segundo_apellido": " ", "email": " ", "telefono": " "}]}
| 12        | **Error Status Code**| 501 (No Implementado)                                                                            |
| 13        | **Error Response Type** | Application/json                                                                               |
| 14        | **Error Response**| {"message": "error en la bd"}                                                                       |
| 15        | **cURL**          | `curl -X GET http://localhost:8000/v1/personas`                                                      |

# Documentación del GET(ID)

| **Punto**               | **Nombre**           | **Descripción**                                                                                                        |
|-------------------------|----------------------|------------------------------------------------------------------------------------------------------------------------|
| 1                       | **Description**      | Regresa un registro especifico en la tabla personas personas.                                                                     |
| 2                       | **Summary**          |Obtener los datos de una persona en especifica en la base de datos                                                                    |
| 3                       | **Method**           | GET                                                                                                                   |
| 4                       | **Endpoint**         | `http://localhost:8000/v1/personas`                                                                                   |
| 5                       | **Authentication**   | No aplica (NA)                                                                                                        |
| 6                       | **Query Param**      | No aplica (NA)                                                                                                        |
| 7                       | **Path Param**       | No aplica (NA)                                                                                                        |
| 8                       | **Data**             | No aplica (NA)                                                                                                        |
| 9                       | **Status Code**      | 200 (Aceptado)                                                                                                        |
| 10                      | **Response Type**    | Application/json                                                                                                      |
| 11                      | **Response**         | {"personas": [{"id_persona": " ", "nombre": " ", "primer_apellido": " ", "segundo_apellido": " ", "email": " ", "telefono": " "}]}
| 12                      | **Error Status Code**| 404 (No Implementado)                                                                                                 |
| 13                      | **Error Response Type** | Application/json                                                                                                     |
| 14                      | **Error Response**   | {"message": "registro no encontrado"}                                                                                         |
| 15                      | **cURL**             | `curl -X GET http://localhost:8000/v1/personas`                                                                       |

# Documentacion de PUT

| **Punto**               | **Nombre**                | **Descripción**                                                                                                        |
|-------------------------|---------------------------|------------------------------------------------------------------------------------------------------------------------|
| 1                       | **Description**           | Actualiza un registro específico en la tabla personas.                                                                 |
| 2                       | **Summary**               | Modifica los datos de una persona específica en la base de datos.                                                     |
| 3                       | **Method**                | PUT                                                                                                                   |
| 4                       | **Endpoint**              | `http://localhost:8000/v1/personas/{id-persona}`                                                                      |
| 5                       | **Authentication**        | No aplica (NA)                                                                                                        |
| 6                       | **Query Param**           | No aplica (NA)                                                                                                        |
| 7                       | **Path Param**            | `id-persona`                                                                                                          |
| 8                       | **Data**                  | `{"personas": {"nombre": "str", "primer_apellido": "str", "segundo_apellido": "str", "email": "str", "telefono": "str"}}` |
| 9                       | **Status Code**           | 200 (OK)                                                                                                              |
| 10                      | **Response Type**         | Application/json                                                                                                      |
| 11                      | **Response**              | `{"message": "Registro actualizado exitosamente"}`                                                                    |
| 12                      | **Error Status Code**     | 404 (No Encontrado)                                                                                                   |
| 13                      | **Error Response Type**   | Application/json                                                                                                      |
| 14                      | **Error Response**        | `{"message": "Registro no encontrado"}`                                                                               |
| 15                      | **CURL**                  | `curl -X PUT -H "Content-Type: application/json" -d '{"personas": {"nombre": "nuevo", "primer_apellido": "nuevo"}}' http://localhost:8000/v1/personas/{id-persona}` |

# Documentacion del POST
| **Punto**               | **Nombre**                | **Descripción**                                                                                                        |
|-------------------------|---------------------------|------------------------------------------------------------------------------------------------------------------------|
| 1                       | **Description**           | Crea un nuevo registro en la tabla personas.                                                                          |
| 2                       | **Summary**               | Agrega una nueva persona a la base de datos.                                                                          |
| 3                       | **Method**                | POST                                                                                                                  |
| 4                       | **Endpoint**              | `http://localhost:8000/v1/personas`                                                                                   |
| 5                       | **Authentication**        | No aplica (NA)                                                                                                        |
| 6                       | **Query Param**           | No aplica (NA)                                                                                                        |
| 7                       | **Path Param**            | No aplica (NA)                                                                                                        |
| 8                       | **Data**                  | `{"personas": {"nombre": "str", "primer_apellido": "str", "segundo_apellido": "str", "email": "str", "telefono": "str"}}` |
| 9                       | **Status Code**           | 201 (Created)                                                                                                         |
| 10                      | **Response Type**         | Application/json                                                                                                      |
| 11                      | **Response**              | `{"message": "Registro creado exitosamente"}`                                                                         |
| 12                      | **Error Status Code**     | 400 (Bad Request, si los datos no son válidos)                                                                        |
| 13                      | **Error Response Type**   | Application/json                                                                                                      |
| 14                      | **Error Response**        | `{"message": "Datos inválidos"}`                                                                                      |
| 15                      | **CURL**                  | `curl -X POST -H "Content-Type: application/json" -d '{"personas": {"nombre": "nuevo", "primer_apellido": "nuevo"}}' http://localhost:8000/v1/personas` |
