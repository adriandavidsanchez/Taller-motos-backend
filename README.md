Taller de Motos — PAVAS S.A.S
Sistema de control de alistamientos y reparaciones para un taller de motos. Permite registrar clientes, motos, crear ordenes de trabajo, gestionar items de reparacion y controlar el flujo de estados de cada orden.

Tecnologias
Backend

Node.js v24 + Express
Sequelize (ORM)
MySQL via XAMPP
dotenv, cors

Frontend

React + Vite
React Router DOM
Axios


Estructura del Proyecto
PRUEBA TÉCNICA FULL STACK – PAVAS S.A.S/
├── taller-backend/
│   ├── .env
│   ├── package.json
│   └── src/
│       ├── index.js                  # Punto de entrada del servidor
│       ├── config/
│       │   └── database.js           # Conexion a MySQL con Sequelize
│       ├── models/
│       │   ├── index.js              # Relaciones entre modelos
│       │   ├── Client.js
│       │   ├── Bike.js
│       │   ├── WorkOrder.js          # Incluye logica de transiciones de estado
│       │   └── OrderItem.js
│       ├── routes/
│       │   ├── clients.js
│       │   ├── bikes.js
│       │   └── workOrders.js         # Incluye logica de items y transacciones
│       └── middlewares/
│           └── errorHandler.js       # Manejo centralizado de errores
│
└── taller-frontend/
    └── src/
        ├── api/
        │   └── axios.js              # Instancia de Axios con URL base
        ├── components/
        │   ├── Navbar.jsx
        │   ├── StatusBadge.jsx
        │   └── Pagination.jsx
        ├── pages/
        │   ├── OrderList.jsx         # Listado con filtros y paginacion
        │   ├── CreateOrder.jsx       # Crear orden con registro rapido de moto
        │   └── OrderDetail.jsx       # Detalle, cambio de estado y gestion de items
        ├── App.jsx
        ├── main.jsx
        └── index.css

Variables de Entorno
Crear el archivo .env en la raiz de taller-backend/:
envDB_HOST=localhost
DB_PORT=3306
DB_NAME=taller_motos
DB_USER=root
DB_PASS=
PORT=4000

DB_PASS se deja vacio porque XAMPP no tiene contrasena por defecto en root.


Instalacion
Requisitos previos

XAMPP corriendo con Apache y MySQL activos
Node.js v18 o superior

Paso 1 — Base de datos
Abrir http://localhost/phpmyadmin, ir a la pestana Importar y cargar el archivo taller_motos.sql incluido en la entrega.
Paso 2 — Backend
bashcd taller-backend
npm install
Paso 3 — Frontend
bashcd taller-frontend
npm install

Ejecucion
Se necesitan dos terminales abiertas al mismo tiempo.
Terminal 1 — Backend
bashcd taller-backend
npm run dev
Respuesta esperada:
Conectado a MySQL
Servidor en http://localhost:4000
Terminal 2 — Frontend
bashcd taller-frontend
npm run dev
Abrir el navegador en: http://localhost:5173

Endpoints de la API
Base URL: http://localhost:4000
Clientes
MetodoEndpointDescripcionPOST/api/clientsCrear clienteGET/api/clients?search=Listar o buscar por nombre/telefonoGET/api/clients/:idObtener cliente con sus motos
Motos
MetodoEndpointDescripcionPOST/api/bikesCrear motoGET/api/bikes?plate=Listar o buscar por placaGET/api/bikes/:idObtener moto con datos del cliente
Ordenes de Trabajo
MetodoEndpointDescripcionPOST/api/work-ordersCrear ordenGET/api/work-orders?status=&plate=&page=&pageSize=Listar con filtros y paginacionGET/api/work-orders/:idDetalle completo (moto, cliente, items)PATCH/api/work-orders/:id/statusCambiar estado de la ordenPOST/api/work-orders/:id/itemsAgregar item a la ordenDELETE/api/work-orders/items/:itemIdEliminar un item

Flujo de Estados
RECIBIDA → DIAGNOSTICO → EN_PROCESO → LISTA → ENTREGADA
                ↓               ↓          ↓        ↓
            CANCELADA      CANCELADA  CANCELADA  (bloqueada)
Reglas de negocio:

Solo se puede avanzar al estado siguiente en el flujo
Se puede cancelar desde cualquier estado excepto ENTREGADA
Una orden ENTREGADA o CANCELADA no acepta mas cambios ni items
Cualquier transicion invalida retorna HTTP 400 con los estados validos disponibles


Validaciones
ValidacionHTTPMensajePlaca duplicada409Ya existe un registro con ese valorCliente no encontrado404Cliente no encontradoMoto no encontrada404Moto no encontradaOrden no encontrada404Orden no encontradaItem no encontrado404Item no encontradoCampos obligatorios faltantes400Mensaje descriptivo por campoCantidad menor o igual a 0400La cantidad debe ser mayor a 0Valor unitario negativo400El valor unitario no puede ser negativoTransicion de estado invalida400No se puede pasar de X a Y. Estados validos: [...]Agregar item a orden cerrada400No se pueden agregar items a una orden cerradaError no controlado500Error interno del servidor

Coleccion Postman
El archivo taller_motos.postman_collection.json incluido en la entrega tiene todos los endpoints listos para importar, organizados en 5 carpetas: Clientes, Motos, Ordenes, Estados e Items.
Importar en Postman:

Clic en Import (esquina superior izquierda)
Seleccionar el archivo .json
Crear una variable de entorno llamada base_url con valor http://localhost:4000


Autor
Prueba Tecnica Full Stack — PAVAS S.A.S