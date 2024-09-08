# Proyecto De MongoDB II 🎬

------

## - **Juan David Rodriguez Ospina**

------

# Instalaciones

- Clonacion del repositorio

  ```
  git clone https://github.com/JuanDr08/Proyecto_Mongo_II.git
  
  code Proyecto_Mongo_II
  ```

- Nada mas clonar el repo, debemos seleccionar la version de node que usemos, el proyecto fue construido en la version 20.17 por lo que para mayor compatibilidad se recomienda usar la misma

  ```
  npm i
  ```

- Esto instalará las dependencias necesarias para poder correr el repo

## Como empezar?

- Primero que nada, una vez instaladas las dependencias del repo deberemos asignar las respectivas variables de entorno y modificar el nombre del archivo .env.template --> .env

  - Las variables de entorno de administrador del sistema son las siguientes

    ```javascript
    HOST="mongodb://"
    VITE_MONGOUSER="Administrador"
    VITE_PASSWORD="1234"
    CLUSTER="serveo.net"
    PORT="27018"
    DB_NAME="cineCampus"
    
    EXPRESS_PORT_BACKEND=3000
    VITE_PORT_FRONTEND=5173
    VITE_HOST="localhost"
    ```

  - El usuario de mongo y la contraseña iran cambiando a medida que  creemos usuarios y queramos trabajar con ellos

  - Cabe recalcar que esas serán las variables de entorno con las que se deberan trabajar si se desea usar mi base de datos, de otro modo, si desea usar su propia base de datos de manera local deberá:

    - Importar los datos de la carpeta server/backup
    - Importar los esquemas de la carpeta storage/schemas
    - Por ultimo debera cambiar las variables de entorno por las que desee usar y que sean compatibles con su base de datos local

- Una vez hecho todo esto podremos lanzar el programa corriendo el siguiente comando

  ```javascript
  npm start --> Backend
  npm run dev --> Frontend
  ```

- De este modo ya tendremos corriendo nuestro programa conectado a la base de datos que hayamos usado

> [!IMPORTANT]
>
> - Por temas de tiempo no se pudo implementar un sistema de login de usuarios, por lo que si queremos switchear entre usuarios deberemos hacerlo cambiando las variables de entorno y re lanzando el proyecto
> - La creacion de usuarios y edicion de usuarios tampoco fue desarrollada en el frontend, por lo que si se quiere crear un usuario debe hacerse desde la API directamente
> - Cada que cambiemos de variables, ya sea por un cambio de usuario o lo que sea deberemos reiniciar el programa, es decir, volver a ejecutar el comando antes mencionado, de lo contrario se le presentarán problemas
> - La API completa puede ser consumida sin problemas desde una interfaz como postman, insomnia o thunder client

# Endpoints

```javascript
GET 'http://localhost:3000/movies' --> Lista las peliculas disponibles
GET 'http://localhost:3000/movie/:id' --> Lista los detalles de una pelicula especifica
GET 'http://localhost:3000/movie/:id/functions' --> Lista todas las funciones que reproduciran una misma funcion
POST'http://localhost:3000/user' --> Crear un usuario
GET 'http://localhost:3000/user/:id' --> Lista los detalles de un usuario especifico
PATCH 'http://localhost:3000/user/:id' --> Actualizar los roles de un usuario
GET 'http://localhost:3000/:rol' --> Todos los usuarios que tienen un rol especifico
GET 'http://localhost:3000/users' --> Listar a todos los usuarios

POST 'http://localhost:3000/ticket/:id' --> Compra de un ticket en estado de reserva
GET 'http://localhost:3000/room/:id' --> Busca una sala segun su id
GET 'http://localhost:3000/movie/:id/seats' --> Lista todos los asientos disponibles de una funcion
POST 'http://localhost:3000/movie/:id/seat' --> Reservar un asiento
DELETE 'http://localhost:3000/ticket/:id' --> eliminar la reserva
GET 'http://localhost:3000/user/:id/tickets' --> todos los tickets comprados por un usuario
GET 'http://localhost:3000/ticket/:id' --> ticket segun su id
```



## Modo de uso

- Para empezar a utilizar el programa iremos en orden, mencionando el modo de uso de los primeros casos de uso hasta los ultimos

  ### peliculas.js

  - El archivo peliculas.js cuenta con la clase Movies, la cual dispone de multiples metodos que nos permiten tanto listar todas las peliculas en cartelera o listar los detalles de una pelicula especifica

  ### Main.js

  #### Caso de uso 1 y 2

  - Para usar estos metodos deberemos escribir el main de la siguiente forma

    - ##### Listar las peliculas en cartelera

      ```javascript
      GET 'http://localhost:3000/movies' --> Devolvera un objeto con el estado y la data
      ```
      
      - Una vez ejecutado este comando deberia ver en consola un arreglo de objetos con los detalles de todas las peliculas en cartelera

    - ##### Listar los detallgies de una pelicula especifica

      ```javascript
      GET 'http://localhost:3000/movie/66a80379a5aad36c22a20c80' --> Este es el identificador unico de la pelicula la cual deseamos saber, deberá cambiarlo en base a la pelicula que desee conocer y pasarlo como parametro a la función
      ```
      
      - Una vez ejecutado deberia poder observar en la consola un objeto con los detalles de la pelicula
    
      - Dado el caso de que el id de la pelicula que ingreso no exista, el programa deberia mostrarle en consola un objeto como este

        ```javascript
          {
            status: 404,
            message: 'Not movies found on the DB'
          }
        ```

    - Eso seria el proceso para completar con exito los casos de uso 1 y 2

- Ahora seguiremos con los procesos de compra de boletos y verificacion de disponibilidad de asientos

  ### boletosYAsientos.js

  - Este archivo contiene lo que vendria siendo la clase de Entries, la cual contiene los distintos metodos relacionados con el tema de boletos y asientos

    #### Casos de uso 3 y 4

    - **API para comprar boletos** 

      ```javascript
      POST 'http://localhost:3000/ticket/66d4f7c242eeef74e1cc3412' --> Id de un ticket existente y en estado de reservada
      // No puede ser un ticket inexistente o no comprado
      
      body: 
      // Estos datos son ejemplos de lo que el frontend debe mandarle a la API, por si se desean ingresar manualmente
      // desde postman o alguno de esos
      {
          "total": 14000, --> Precio de ejemplo y base de cada boleta
          "fechaFuncion": 'vie 30 sep 2024', --> Fecha ejemplo
          "hora": '18:00' --> Hora ejemplo
          "sala": 'Sala 1' --> Sala de ejemplo
      }
      ```

      - Si todos los datos se ingresaron bien el el programa, usted deberia ser capaz de observar un mensaje de retorno como el siguiente
    
        ```javascript
        {
            status: 201,
            message: 'Ticket comprado exitosamente',
            data: [Object Object]
        }
        ```
    
      - Si no es el caso, puede que le hayan ocurrido problemas con los datos y este observando alguno de los siguientes errores
    
        ```javascript
        { //Si no existe el ticket
            status: 404,
            msg: `El ticket de id: 66d4f7c242eeef74e1cc3412  no fue encontrado o no existe, ingrese uno valido`
        }
        
        { // Cualquier otro error
            status: 500,
            message: 'Ha ocurrido un problema durante la compra del ticket',
            data: arg
        }
        ```
        
        - Si este es alguno de sus casos porfavor rectifique la autenticidad de los datos que intenta registrar

    - **API para verificar la disponibilidad de asientos**
    
      - En este caso decidi no solo mostrar los asientos disponibles, sino tambien los ocupados, esto con el fin de acercarme a lo que es comprar boletas en linea en los cines reales, donde nos enseñan cuales si y cuales no
    
        ```javascript
        GET 'http://localhost:3000/movie/66cf136cbd321146f37c704e/seats' --> id de una funcion existente
        ```
        
        - Si el id de la funcion la cual quisimos consultar es corrrecto deberiamos ser capaces de observar en la consola un resultado como el siguiente
    
          ```javascript
          {
              status: 200,
              msg: [
                    { codigo: 'A1', estado: 'disponible' },
                    { codigo: 'A2', estado: 'reservada' },
                    { codigo: 'A3', estado: 'comprada' },
                    { codigo: 'B1', estado: 'disponible' },
                    { codigo: 'B2', estado: 'reservada' },
                    { codigo: 'B3', estado: 'comprada' },
                    { codigo: 'C1', estado: 'comprada' },
                    { codigo: 'C2', estado: 'comprada' },
                    { codigo: 'C3', estado: 'comprada' },
                    { codigo: 'D1', estado: 'comprada' },
                    { codigo: 'D2', estado: 'comprada' },
                    { codigo: 'D3', estado: 'comprada' }
                  ]
          }
          ```
          
          - De otro modo, si el id que haya ingresado no existe, entonces verá en consola un mensaje como este
          
            ```javascript
            { // Si la funcion ingresada no existe
                status: 404,
                msg: `La funcion de id: 66cf136cbd321146f37c704e no se encuetra disponible`
            }
            ```

    #### Casos de uso 5 y 6
    
    - **Reservar asientos**

      Para poder reservar un asiento es importante conocer:
    
      1. La funcion en la cual deseamos reservar el asiento
      2. El codigo del asiento de dicha funcion que queremos reservar
      3. Que el asiento este disponible para la reserva
    
      ```javascript
      POST 'http://localhost:3000/movie/:id/seat'
      body:
      {
          "seatsCode" : ["A1"] --> Si o si un arreglo con los codigos de asientos, cada codigo debe seguir el formato
          -- A-Z 1-20
      }
      
      bad Body Request:
      // Si ingresamos mal el body nos debera saltar un error de un objeto con los detalles
      ```
    
      - Si completamos exitosamente la reserva del asiento y todos los datos fueron correctos, deberiamos ser capaces de observar un mensaje como el siguiente
    
        ```javascript
        {
          acknowledged: true,
          modifiedCount: 1,
          upsertedId: null,
          upsertedCount: 0,
          matchedCount: 1
        },
         ticketGenerado = '66d4f7c242eeef74e1cc3412'
        ```
    
      - Si este no es su caso y se presento un problema durante la ejecucion de la reserva es probable que haya un dato erroneo y pueda observar un mensaje de salida como los siguientes
    
        ```javascript
        { // El id de la funcion es erroneo
        	Error: 'El id de la funcion ingresada no existe', status: "404"
        }
        { // El asiento ingresado no existe dentro de la funcion
        	Error: 'El asiento que desea reservar no existe', status: "404", asientos: []
        }
        { // El asiento ingresado si existe dentro de la funcion pero no esta disponible
        	Error: 'El asiento que desea reservar no esta disponible actualmente, escoja otro', 
        	status: "409", 
        	asientos: []
        }
        ```

        - Si alguno de estos es su caso, por favor debe modificar los datos que intenta ingresar
    
    - **Cancelar reserva**
    
      Al igual que el proceso de reserva de asientos, para la cancelacion deberá conocer:
    
      1. Id de la funcion en donde reservo el asiento
      2. Codigo del asiento que  reservo en la funcion
      3. El asiento debe tener el estado de reservada
    
      ```javascript
      DELETE 'http://localhost:3000/ticket/66d4f7c242eeef74e1cc3412' --> id del ticket Existente en estado de reserva
      ```
      
      - Si todo sale bien y los datos son correctos deberiamos poder observar en la terminal una salida como esta
      
        ```javascript
        {
          acknowledged: true,
          modifiedCount: 1,
          upsertedId: null,
          upsertedCount: 0,
          matchedCount: 1
        }
        ```
      
      - Y si  tenemos conexion a la base de datos de manerá visual, podremos observar que el asiento que antes estaba reservado pasó a estar disponible
      
        - Si este no ha sido su caso y se han presentado problemas es posible que vea alguno de los siguientes mensajes de error
      
          ```javascript
          { // Id de la funcion incorrecto
          	Error: 'El id de la funcion ingresada no existe', status: "404"
          }
          { // El asiento no existe dentro de los registrados en la funcion
          	Error: 'El asiento ingresado no existe', status: "404 ", asientosSala: asientos
          }
          { // El asiento existe dentro de la funcion pero dicho asiento no esta reservado
          	Error: 'El asiento existe, pero su estado no esta reservado, por lo que no hay nada que cancelar', status: 400, asientos: asientos
          }
          ```
      
          - Si alguno de esos es su caso entonces deberá modificar los datos a algunos que sean correctos para realizar la cancelacion de manera efectiva
    
    #### Casos de uso 7 y 8
    
    - **API para aplicar descuentos & API para validar tarjeta VIP**
    
      - No existe un metodo en si que aplique los descuentos o valide la tarjeta VIP, ya que decidi hacerlo de manerá automatica dentro de la compra
    
      - **Criterios de descuento:** 

        1. La boleta base tiene un costo de 14.000

        2. Si el usuario esta comprando un silla que corresponde a la fila VIP de la sala entonces el precio de su boleta aumentara en un 97% (precios basados en cinemark)
        3. Si el usuario cuenta con tarjeta VIP se le aplicara un descuento del 20% en el total de su boleta
    
      - **Dichos descuentos se pueden ser observados aplicandose en el archivo boletosYAsientos.js, desde la linea 43 hasta la 56**
    
    #### Roles definidos
    
    - Se crearon los 3 roles propuestos
      1. Administrador - con permisos generales a la base de datos completa
      2. Usuario estandar - cuenta con permisos como:
         - Lectura de usuarios
         - Lectura, Escritura, Edicion de boletos
         - Lectura, Edicion de Funcion
         - Lectura de Pelicula
      3. Usuario Vip - cuenta con permisos como: 
         - Lectura de usuarios
         - Lectura, Escritura, Edicion de boletos
         - Lectura, Edicion de Funcion
         - Lectura de Pelicula
    
    #### Casos de uso 9 - 12
    
    ### usuarios.js
    
    > [!IMPORTANT]
    >
    > Para realizar estos casos de uso, en especial la creacion y edicion de usuarios deberá estar logueado como Administrador del sistema, de lo contrario solo obtendrá excepciones de autenticacion
    
    - **API para crear usuarios**
    
      ```javascript
      POST'http://localhost:3000/user' --> EndPoint para crear usuario
      
      body:
      {
          "nombre": "karen",
          "nick": "karen",
          "rol": ("UsuarioEstandar", "UsuarioVip", "Admin"),
          "contrasenia": 123,
          "email": "karen@gmail.com",
          "telefono": 3222352673
      }
      ```
    
      - Es importante tener en cuenta que el Nick y la contrasenia registrados serán los que se deberán reemplazar por el VITE_MONGOUSER y VITE_PASSWORD en el archivo .env respectivamente para poder conectarnos con la cadena de conexion  creada para dichos usuarios
    
      - La contrasenia tambien será usado como identificador unico del registro del usuario en su respectiva coleccion
    
      - Si la creacion del usuario se hizo con exito deberiamos ser capaces de observar un mensaje como el siguiente
    
        ```javascript
        {
          "status": 400,
          "msg": {
            "acknowledged": true,
            "insertedId": 123
          }
        }
        ```
    
        - Algunos de los mensajes de fallo en la creacion que podriamos obtener serian
        
          ```javascript
          { // Este error es porque no esta autorizado a crear usuarios
            ok: 0,
            errmsg: '',
            code: 13,
            codeName: 'Unauthorized'
          }
          // Este error es por intentar registrar un usuario con una identificacion que ya se encuentra usada en el sistema
          {
            "status": 409,
            "msg": "Ya hay un usuario existente con el codigo 123"
          }
          ```
        
          - Si alguno de estos errores es su caso por favor, revise cautelosamente las credenciales con las que esta intentando ingresar el nuevo usuario, que la contrasenia sea unica y que todos los datos esten correctamente bien tipados
    
    - **Api para obtener detalles de un usuario**
    
      ```javascript
      GET 'http://localhost:3000/user/123' --> Endpoint, cambiar el id por el id del usuario que querramos
      ```
    
      - Si ingresamos correctamente la cedula o identificador del usuario que deseamos conocer deberiamos de poder observar en pantalla un resultado como este
    
        ```javascript
        {
          "status": 200,
          "msg": {
            "Usuario": {
              "_id": 123,
              "Nombre": "karen",
              "Nick": "karen",
              "rol": "UsuarioEstandar",
              "contrasenia": 123,
              "email": "karen@gmail.com",
              "telefono": "3222352673"
            },
            "roles": [
              {
                "role": "UsuarioEstandar",
                "db": "cineCampus"
              }
            ]
          }
        }
        ```
        
        - Si se ingreso de manera incorrecta la cedula o identificacion del usuario obtendremos un mensaje como este y por lo tanto deberemos volverlo a intentar con una identificacion que si exista
        
          ```javascript
          {
            "status": 404,
            "msg": "No se encontró el usuario con id: 1233"
          }
          ```
    
    - **API para Actualizar Rol de Usuario:**

      ```javascript
      PATCH 'http://localhost:3000/user/123' --> cambiar el id por el del usuario que desea actualizar
      
      body:
      {
          "tarjeta": true --> El campo de tarjeta indica si se quiere que el usuario sea vip o no, si es 'true'
          -- el sistema generara una tarjeta vip al usuario, si es 'false' el sistema asignara la tarjeta del usuario
          -- a inactiva
      }
      ```

      - El campo de 'tarjeta' es sumamente importante, ya que con este decidimos que accion tomar
        - Si dejamos el campo en TRUE, al usuario se le asignara una tarjeta VIP, y el rol del usuario pasará a ser UsuarioVip
        - Si dejamos el campo en FALSE, al usuario se le cambiara el estado de la tarjeta a inactiva, y pasara de ser UsuarioVip a UsuarioEstandar
    
      - Si realizamos correctamente el registro del objeto y el usuario existe obtendremos una salida como esta

        ```javascript
        {
          "status": 201,
          "msg": {
            "ok": 1
          }
        }
        ```
    
      - Si el usuario al que le queremos modificar el rol no existe, obtendremos una salida como esta
    
        ```javascript
        {
          "status": 404,
          "msg": "No se encontró el usuario con id: 1233"
        }
        ```
    
    - **API para Listar Usuarios:**
    
      ```javascript
      GET 'http://localhost:3000/users/admin' --> ENDPOINT para filtrar usuarios administradores
      GET 'http://localhost:3000/users/vip' --> ENDPOINT para filtrar usuarios Vip
      GET 'http://localhost:3000/users/estandar' ENDPOINT para filtrar usuarios estandar
      GET 'http://localhost:3000/users/' --> ENDPOINT para listar a todos los usuarios
      ```
      
      - Sencillo, tenemos cuatro posibles filtros, estandar, vip, admin y " "
        - Deberemos escoger cualquiera de ellos dependiendo del tipo de usuario que queramos listar
        - Los filtros pueden estar en mayuscula o minuscula, eso da igual, pero tienen que estar escritos tal cual como estan plazmados
        - El filtro " " lo que hace es no aplicar filtro y simplemente nos trae todos lo detalles de todos los usuarios
      
      - Por ejemplo, si aplicamos el filtro de estandar deberiamos obtener un resultado como el siguiente
      
        ```javascript
        {
          "status": 200,
          "msg": [
            {
              "_id": "cineCampus.karen",
              "userId": "af34bd45-712e-45b4-9a7d-6801d46763b7",
              "user": "karen",
              "db": "cineCampus",
              "roles": [
                {
                  "role": "UsuarioEstandar",
                  "db": "cineCampus"
                }
              ],
              "mechanisms": [
                "SCRAM-SHA-1",
                "SCRAM-SHA-256"
              ]
            }
          ]
        }
        ```
        
        - Algunos de los errores que nos pueden ocurrir son errores de autenticacion, pero eso ya depende de las credenciales que se usen

​				
