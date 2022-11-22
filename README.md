# proyecto-base-de-datos

CREATE DATABASE insumosinformaticos			--Creación de la base de datos
USE insumosinformaticos			--Selecciona cualquier base de datos existente en el SQL esquema

--Creación de la tabla Provincias 
CREATE TABLE provincias (
	id_provincia INT, 
	descripcion VARCHAR(20) NOT NULL,
	CONSTRAINT PK_provincia PRIMARY KEY (id_provincia), 
)



--Creación de la tabla Localidades 
CREATE TABLE localidades ( 
	id_provincia INT, 
	id_localidad INT, 
	descripcion VARCHAR(100) NOT NULL, 
	CONSTRAINT PK_localidad PRIMARY KEY (id_provincia, id_localidad), 
)


--Creación de la tabla Perfiles 
CREATE TABLE perfiles ( 
	id_perfil INT, 
	descripcion VARCHAR(20) NOT NULL, 
	CONSTRAINT PK_perfil PRIMARY KEY (id_perfil), 
)


--Creación de la tabla Usuarios 
CREATE TABLE usuarios (	
	id_usuario INT, 
	id_perfil INT, 
	nom_y_ape VARCHAR(100) NOT NULL, 
	contrasenia VARCHAR(8) NOT NULL, 
	email VARCHAR(100) NOT NULL, 
	domicilio VARCHAR(120) NOT NULL, 
	telefono VARCHAR(15) NOT NULL, 
	cod_provincia INT, 
	cod_localidad INT, 
	CONSTRAINT PK_usuario PRIMARY KEY (id_usuario), 
	CONSTRAINT FK_usuario_perfil FOREIGN KEY (id_perfil) REFERENCES perfiles(id_perfil), 
	CONSTRAINT FK_usuario_localidad FOREIGN KEY (cod_provincia, cod_localidad) REFERENCES localidades(id_provincia, id_localidad) 
)

--Creación de la tabla Tipo de Pago 
CREATE TABLE tipo_pago ( 
	id_tipo_pago INT, 
	descripcion VARCHAR(30) NOT NULL, 
	descuento REAL NOT NULL, 
	CONSTRAINT PK_tipo_pago PRIMARY KEY (id_tipo_pago)
)

--Creación de la tabla Sucursales 
CREATE TABLE sucursales( 
	cuit int NOT NULL CONSTRAINT UQ_sucursalCuit UNIQUE, 
	nombre VARCHAR(80) NOT NULL, 
	direccion VARCHAR(120) NOT NULL, 
	telefono VARCHAR(15) NOT NULL, 
	email VARCHAR(100) NOT NULL, 
	horarios VARCHAR(50) NOT NULL, 
	URL VARCHAR(150) NOT NULL, 
	CONSTRAINT PK_sucursal PRIMARY KEY (cuit) 
)


--Creación de la tabla Categorías 
CREATE TABLE categorias(
	id_categoria INT, 
	descripcion VARCHAR(30) NOT NULL, 
	CONSTRAINT PK_categoria PRIMARY KEY (id_categoria),
)


--Creacion de la tabla Insumos 
CREATE TABLE insumos ( 
	id_insumos INT,
	id_categoria INT NOT NULL,
	descripcion VARCHAR(50) NOT NULL, 
	img VARCHAR(150) NOT NULL, 
	stock INT NOT NULL, 
	stockMin INT NOT NULL, 
	CONSTRAINT PK_insumo PRIMARY KEY (id_insumos), 
	CONSTRAINT FK_insumo_categoria FOREIGN KEY (id_categoria) REFERENCES Categorias(id_categoria), CONSTRAINT CK_insumoStock CHECK (stock > 0), --Restricción CHECK (valores válidos mayores a 0 en el atributo stock) 
	CONSTRAINT CK_insumoStockMin CHECK (stockMin > 0), --Restricción CHECK (valores válidos mayores a 0 en el atributo stockMin) 
	CONSTRAINT CK_insumo_StockMayorStockMin CHECK (stock >= stockMin) --Restricción CHECK (valores válidos mayores o iguales a stock en el atributo stockMin) )
)

--Creacion de la tabla Proveedores
CREATE TABLE proveedores ( 
	id_proveedor INT, 
	nombre VARCHAR(80) NOT NULL, 
	cuit VARCHAR(10) NOT NULL CONSTRAINT UQ_proveedorCuit UNIQUE, 
	domicilio VARCHAR(120) NOT NULL, 
	email VARCHAR(100) NOT NULL, 
	telefono VARCHAR(15) NOT NULL, 
	CONSTRAINT PK_proveedor PRIMARY KEY (id_proveedor), 
)


--Creación de la tabla Insumo-Proveedor 
CREATE TABLE insumos_proveedor ( 
	id_insumo INT, 
	id_proveedor INT, 
	precio_Costo REAL NOT NULL, 
	precio_Venta REAL NOT NULL, 
	CONSTRAINT PK_insumoProveedor PRIMARY KEY (id_insumo, id_proveedor),
	CONSTRAINT FK_insumoProveedor_insumo FOREIGN KEY (id_insumo) REFERENCES insumos(id_insumos),
	CONSTRAINT FK_insumoProveedor_proveedor FOREIGN KEY (id_proveedor) REFERENCES proveedores(id_proveedor) 
)


--Creación de la tabla Venta Detalle 
CREATE TABLE venta_detalle ( 
	id_detalle INT NOT NULL, 
	id_insumo INT NOT NULL, 
	id_proveedor INT NOT NULL, 
	cantidad INT NOT NULL, 
	precio_unitario REAL NOT NULL, 
	CONSTRAINT PK_ventaDetalle PRIMARY KEY (id_detalle), 
	CONSTRAINT FK_ventaDetalle_InsumoProveedor FOREIGN KEY (id_insumo, id_proveedor) REFERENCES insumos_proveedor(id_insumo, id_proveedor) 
)


--Creación de la tabla Venta Cabecera 
CREATE TABLE venta_cabecera ( 
	id_cabecera INT NOT NULL, 
	id_detalle INT NOT NULL, 
	fecha_y_hora DATETIME NOT NULL, 
	total REAL NOT NULL, 
	id_usuario INT NOT NULL, 
	id_tipo_pago INT NOT NULL, 
	cuit INT NOT NULL, 
	CONSTRAINT PK_ventaCabecera PRIMARY KEY (id_cabecera,id_detalle), 
	CONSTRAINT FK_ventaCabecera_Detalle FOREIGN KEY (id_detalle) REFERENCES venta_detalle(id_detalle), 
	CONSTRAINT FK_ventaCabecera_usuarios FOREIGN KEY (id_usuario) REFERENCES usuarios(id_usuario), 
	CONSTRAINT FK_ventaCabecera_tipo_pago FOREIGN KEY (id_tipo_pago) REFERENCES tipo_pago(id_tipo_pago), 
	CONSTRAINT FK_ventaCabecera_sucursales FOREIGN KEY (cuit) REFERENCES sucursales(cuit) 
)


--Lote Trabajo de Campo

USE insumosinformaticos 
select * from provincias 
/*PROVINCIAS*/

Insert into provincias (id_provincia, descripcion) values (1, 'Capital Federal') 
Insert into provincias (id_provincia, descripcion) values (2, 'Buenos Aires') 
Insert into provincias (id_provincia, descripcion) values (3, 'Catamarca') 
Insert into provincias (id_provincia, descripcion) values (4, 'Chaco') 
Insert into provincias (id_provincia, descripcion) values (5, 'Chubut') 
Insert into provincias (id_provincia, descripcion) values (6, 'Córdoba') 
Insert into provincias (id_provincia, descripcion) values (7, 'Corrientes') 
Insert into provincias (id_provincia, descripcion) values (8, 'Entre Rios') 
Insert into provincias (id_provincia, descripcion) values (9, 'Formosa') 
Insert into provincias (id_provincia, descripcion) values (10, 'Jujuy') 
Insert into provincias (id_provincia, descripcion) values (11, 'La Pampa') 
Insert into provincias (id_provincia, descripcion) values (12, 'La Rioja') 
Insert into provincias (id_provincia, descripcion) values (13, 'Mendoza') 
Insert into provincias (id_provincia, descripcion) values (14, 'Misiones') 
Insert into provincias (id_provincia, descripcion) values (15, 'Neuquén')
Insert into provincias (id_provincia, descripcion) values (16, 'Río Negro') 
Insert into provincias (id_provincia, descripcion) values (17, 'Salta') 
Insert into provincias (id_provincia, descripcion) values (18, 'San Juan')
Insert into provincias (id_provincia, descripcion) values (19, 'San Luis')
Insert into provincias (id_provincia, descripcion) values (20, 'Santa Cruz')
Insert into provincias (id_provincia, descripcion) values (21, 'Santa Fe')
Insert into provincias (id_provincia, descripcion) values (22, 'Santiago del Estero')
Insert into provincias (id_provincia, descripcion) values (23, 'Tierra del Fuego') 
Insert into provincias (id_provincia, descripcion) values (24, 'Tucuman')

/*Localidades*/ 
ALTER TABLE localidades ALTER COLUMN descripcion varchar (100)

INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (1, 1, 'Ciudad Autónoma de Buenos Aires')
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (1, 2, 'Colegiales') 
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (1, 3, 'Retiro') 
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (2, 1, 'Adrogué')
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (2, 2, 'Alberti')
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (2, 3, 'Arrecifes') 
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (2, 4, 'Avellaneda')
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (3, 1, 'Ancasti')
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (3, 2, 'Andalgalá') 
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (3, 3, 'Antofagasta de la Sierra')
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (3, 4, 'Bañado de Ovanta')
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (4, 1, 'Aviá Terai') 
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (4, 2, 'Barranqueras') 
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (4, 3, 'Basail')
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (4, 4, 'Campo Largo') 
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (5, 1, 'Alto Río Senguer') 
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (5, 2, 'Camarones') 
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (5, 3, 'Comodoro Rivadavia') 
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (5, 4, 'Dolavón')
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (6, 1, 'Achiras') 
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (6, 2, 'Adelia María') 
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (6, 3, 'Agua de Oro')
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (6, 4, 'Alejandro Roca') 
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (7, 1, 'Alvear') 
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (7, 2, 'Bella Vista') 
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (7, 3, 'Berón de Astrada')
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (7, 4, 'Bonpland') 
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (8, 1, 'Aldea San Antonio')
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (8, 2, 'Aranguren') 
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (8, 3, 'Bovril') 
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (8, 4, 'Caseros')
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (9, 1, 'Clorinda')
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (9, 2, 'Comandante Fontana')
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (9, 3, 'El Colorado') 
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (9, 4, 'Espinillo') 
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (10, 1, 'Abra Pampa')
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (10, 2, 'Caimancito')
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (10, 3, 'Calilegua') 
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (10, 4, 'El Aguilar') 
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (11, 1, 'Algarrobo del Águila')
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (11, 2, 'Alpachiri') 
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (11, 3, 'Alta Italia') 
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (11, 4, 'Anguil')
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (12, 1, 'Aimogasta') 
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (12, 2, 'Aminga') 
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (12, 3, 'Arauco') 
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (12, 4, 'Castro Barros') 
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (13, 1, 'General Alvear')
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (13, 2, 'General Lavalle')
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (13, 3, 'Godoy Cruz') 
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (13, 4, 'Junín')
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (14, 1, 'Alba Posse')
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (14, 2, 'Almafuerte')
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (14, 3, 'Apóstoles') 
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (14, 4, 'Aristóbulo del Valle')
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (15, 1, 'Aluminé') 
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (15, 2, 'Andacollo') 
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (15, 3, 'Añelo') 
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (15, 4, 'Barrancas')
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (16, 1, 'Allen')
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (16, 2, 'Catriel') 
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (16, 3, 'Cervantes')
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (16, 4, 'Chichinales') 
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (17, 1, 'Apolinario Saravia') 
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (17, 2, 'Cachí')
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (17, 3, 'Cafayate')
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (17, 4, 'Campo Quijano')
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (18, 1, 'Albardón') 
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (18, 2, 'Calingasta') 
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (18, 3, 'Caucete') 
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (18, 4, 'Chimbas') 
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (19, 1, 'Buena Esperanza')
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (19, 2, 'Candelaria')
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (19, 3, 'Concarán') 
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (19, 4, 'Justo Daract')
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (20, 1, '28 de Noviembre')
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (20, 2, 'Caleta Olivia')
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (20, 3, 'Comandante Luis Piedra Buena')
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (20, 4, 'El Calafate') 
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (21, 1, 'Armstrong') 
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (21, 2, 'Arroyo Seco') 
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (21, 3, 'Arrufó') 
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (21, 4, 'Avellaneda') 
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (22, 1, 'Añatuya') 
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (22, 2, 'Arraga') 
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (22, 3, 'Bandera')
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (22, 4, 'Beltrán')
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (23, 1, 'Río Grande')
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (23, 2, 'Ushuaia')
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (24, 1, 'Aguilares') 
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (24, 2, 'Alderetes') 
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (24, 3, 'Banda del Río Salí')
INSERT INTO localidades (id_provincia, id_localidad, descripcion) VALUES (24, 4, 'Bella Vista')

/*Perfiles*/

INSERT INTO perfiles(id_perfil,descripcion) VALUES (1,'Administrador') 
INSERT INTO perfiles(id_perfil,descripcion) VALUES (2,'Empleado') 
INSERT INTO perfiles(id_perfil,descripcion) VALUES (3,'Cliente')


/*Usuarios*/

INSERT INTO usuarios(id_usuario,id_perfil,nom_y_ape,contrasenia,email,domicilio,telefono,cod_provincia,cod_localidad) VALUES (1,1,'Alvaro Gonzales','AlvaroGo','AlvaroGonzales@gmail.com','Barrio molina punta sector 5 casa 3','3794528364',1,1) 
INSERT INTO usuarios(id_usuario,id_perfil,nom_y_ape,contrasenia,email,domicilio,telefono,cod_provincia,cod_localidad) VALUES (2,1,'Nicolas Gomez','NicolasG','NicolasGomez@gmail.com','Barrio Rotonda sector 2 casa 5','3794458356',1,2) 
INSERT INTO usuarios(id_usuario,id_perfil,nom_y_ape,contrasenia,email,domicilio,telefono,cod_provincia,cod_localidad) VALUES (3,1,'Ramirez Jorge Esteban','jorgera','AlvaroGonzales@gmail.com','Barrio vizcacha sector 10 casa 32','374449272',1,3)
INSERT INTO usuarios(id_usuario,id_perfil,nom_y_ape,contrasenia,email,domicilio,telefono,cod_provincia,cod_localidad) VALUES (4,1,'Alcaraz Sonia','soniaalc','AlvaroGonzales@gmail.com','Barrio laguna seca sector 3 casa 13','374449276',2,1) 
INSERT INTO usuarios(id_usuario,id_perfil,nom_y_ape,contrasenia,email,domicilio,telefono,cod_provincia,cod_localidad) VALUES (5,2,'Almada De Emerenciana','almadaem','AlvaroGonzales@gmail.com','Barrio 17 de agosto sector 15 casa 3','374449378',2,2)
INSERT INTO usuarios(id_usuario,id_perfil,nom_y_ape,contrasenia,email,domicilio,telefono,cod_provincia,cod_localidad) VALUES (6,2,'Ramirez Elias Esteban','ramireze','AlvaroGonzales@gmail.com','Barrio camba cua sector 75 casa 20','374449376',2,3)
INSERT INTO usuarios(id_usuario,id_perfil,nom_y_ape,contrasenia,email,domicilio,telefono,cod_provincia,cod_localidad) VALUES (7,2,'Marin De Antolina','marinde','AlvaroGonzales@gmail.com','Barrio san geronimo sector 8 casa 9','374449375',2,4) 
INSERT INTO usuarios(id_usuario,id_perfil,nom_y_ape,contrasenia,email,domicilio,telefono,cod_provincia,cod_localidad) VALUES (8,2,'Gayozo Ramon','gayozo','AlvaroGonzales@gmail.com','Barrio 1000 viviendas sector 15 casa 6','374449374',3,1) 
INSERT INTO usuarios(id_usuario,id_perfil,nom_y_ape,contrasenia,email,domicilio,telefono,cod_provincia,cod_localidad) VALUES (9,3,'Billordo Gabriel','gabriel','AlvaroGonzales@gmail.com','Barrio ciudad gotica sector 4 casa 20','374449373',3,2) 
INSERT INTO usuarios(id_usuario,id_perfil,nom_y_ape,contrasenia,email,domicilio,telefono,cod_provincia,cod_localidad) VALUES (10,3,'Montenegro Petrona','petrona','AlvaroGonzales@gmail.com','Barrio Quintana sector 25 casa 34','374449371',3,3)
INSERT INTO usuarios(id_usuario,id_perfil,nom_y_ape,contrasenia,email,domicilio,telefono,cod_provincia,cod_localidad) VALUES (11,3,'Paredes Graciela','graciela','AlvaroGonzales@gmail.com','Barrio punta taitalo sector 13 casa 35','374449311',3,4)
INSERT INTO usuarios(id_usuario,id_perfil,nom_y_ape,contrasenia,email,domicilio,telefono,cod_provincia,cod_localidad) VALUES (12,3,'Ramirez Ramona','ramona','AlvaroGonzales@gmail.com','Barrio guemes sector 2 casa 1','374449362',4,1)

/*Tipo de Pago*/ 


INSERT INTO tipo_pago(id_tipo_pago,descripcion,descuento) VALUES(1,'Trasferencia',10) 
INSERT INTO tipo_pago(id_tipo_pago,descripcion,descuento) VALUES(2,'Tarjeta De Credito',0) 
INSERT INTO tipo_pago(id_tipo_pago,descripcion,descuento) VALUES(3,'Tarjeta De Debito',5) 
INSERT INTO tipo_pago(id_tipo_pago,descripcion,descuento) VALUES(4,'Mercado Pago',0)

/*Sucursales*/

INSERT INTO sucursales(cuit,nombre,direccion,telefono,email,horarios,URL) VALUES (1111,'Luis A.Cuadrado','Rioja 1592','3794567352','LuisACuadrado@gmail.com','08:00 a 13:00//15:00 a 18:00','http://LuisACuadrado.com') 
INSERT INTO sucursales(cuit,nombre,direccion,telefono,email,horarios,URL) VALUES (2222,'Uno Informatica','Salta 273','367395321','UnoInformatica@gmail.com','07:00 a 13:00//16:00 a 20:00','http://UnoInformatica.com') 
INSERT INTO sucursales(cuit,nombre,direccion,telefono,email,horarios,URL) VALUES (3333,'Pixeles','Junin 333','3794760373','Pixeles@gmail.com','09:00 a 13:00//15:00 a 19:00','http://Pixeles.com') 
INSERT INTO sucursales(cuit,nombre,direccion,telefono,email,horarios,URL) VALUES (4444,'RyR Computacion','Buenos Aires 3000','383941612','RyRComputacion@gmail.com','08:00 a 13:00//15:00 a 20:00','http://RyRComputacion.com') 
INSERT INTO sucursales(cuit,nombre,direccion,telefono,email,horarios,URL) VALUES (5555,'Tecno House','Maipu 1348','344816293','TecnoHouse@gmail.com','08:00 a 12:00//15:00 a 19:00','http://TecnoHouse.com')

/*Categorias*/ 


INSERT INTO categorias(id_categoria,descripcion) VALUES (1, 'Interno') 
INSERT INTO categorias(id_categoria,descripcion) VALUES (2, 'Externo')

/*Insumos*/ 


INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (1,1,'Procesador Intel i3','img',500,50) 
INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (2,1,'Procesador Intel i5','img',500,50) 
INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (3,1,'Procesador Intel i7','img',500,50) 
INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (4,1,'Procesador Intel i9','img',500,50) 
INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (5,1,'Procesador Ryzen 3','img',500,50) 
INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (6,1,'Procesador Ryzen 5','img',500,50)
INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (7,1,'Procesador Ryzen 7','img',500,50) 
INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (8,1,'Procesador Ryzen 9','img',500,50)

INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (9,1,'Placa Madre Asus Micro ATX','img',500,50) 
INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (10,1,'Placa Madre Asus ATX','img',500,50)
INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (11,1,'Placa Madre Gigabyte Micro ATX','img',500,50) 
INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (12,1,'Placa Madre Gigabyte ATX','img',500,50)
INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (13,1,'Placa Madre Asrock Micro ATX','img',500,50) 
INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (14,1,'Placa Madre Asrock ATX ','img',500,50)

INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (15,1,'Memoria RAM 4GB DDR4','img',1000,100)
INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (16,1,'Memoria RAM 8GB DDR4','img',1000,100)
INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (17,1,'Memoria RAM 16GB DDR4','img',1000,100) 
INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (18,1,'Memoria RAM 32GB DDR4','img',1000,100)

INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (19,1,'Placa de Video RTX 3060','img',500,50)
INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (20,1,'Placa de Video RTX 3070','img',500,50) 
INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (21,1,'Placa de Video RTX 3080','img',500,50) 
INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (22,1,'Placa de Video RTX 3090','img',500,50) 
INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (23,1,'Placa de Video RX 6600 XT','img',500,50) 
INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (24,1,'Placa de Video RX 6700 XT','img',500,50)
INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (25,1,'Placa de Video RX 6800 XT','img',500,50)
INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (26,1,'Placa de Video RX 6900 XT','img',500,50)

INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (27,1,'Disco 1TB HDD','img',1000,100) 
INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (28,1,'Disco 2TB HDD','img',1000,100)
INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (29,1,'Disco 3TB HDD','img',1000,100)
INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (30,1,'Disco 4TB HDD','img',1000,100)
INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (31,1,'Disco 120GB SSD','img',1000,100)
INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (32,1,'Disco 256GB SSD','img',1000,100)
INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (33,1,'Disco 480GB SSD','img',1000,100) 
INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (34,1,'Disco 1TB SSD','img',1000,100)

INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (35,1,'Fuente 500W ASUS','img',1000,100) 
INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (36,1,'Fuente 600W ASUS','img',1000,100) 
INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (37,1,'Fuente 700W ASUS','img',1000,100)
INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (38,1,'Fuente 500W GIGABYTE','img',1000,100)
INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (39,1,'Fuente 600W GIGABYTE','img',1000,100)

INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (40,1,'Fuente 700W GIGABYTE','img',1000,100)
INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (41,1,'Fuente 500W SEASONIC','img',1000,100)

INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (42,1,'Fuente 600W SEASONIC','img',1000,100)
INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (43,1,'Fuente 700W SEASONIC','img',1000,100)

INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (44,2,'Gabinete MSI MICRO ATX','img',1000,100) 
INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (45,2,'Gabinete MSI ATX','img',1000,100)
INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (46,2,'Gabinete ASUS MICRO ATX','img',1000,100)
INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (47,2,'Gabinete ASUS ATX','img',1000,100)
INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (48,2,'Gabinete THERMALTAKE MICRO ATX','img',1000,100) 
INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (49,2,'Gabinete THERMALTAKE ATX','img',1000,100)
INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (50,2,'NZXT MICRO ATX','img',1000,100) 
INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (51,2,'NZXT ATX','img',1000,100)

INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (52,2,'Monitor LG 24 pulgadas','img',500,50) 
INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (53,2,'Monitor Samsung 24 pulgadas','img',500,50)
INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (54,2,'Monitor ACER 24 pulgadas','img',500,50)
INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (55,2,'Monitor ASUS 24 pulgadas','img',500,50)

INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (56,2,'Teclado Mecanico Logitech','img',500,50) 
INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (57,2,'Teclado Mecanico Hyperx','img',500,50)
INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (58,2,'Teclado Mecanico Asus','img',500,50) 
INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (59,2,'Teclado Mecanico XPG','img',500,50) 
INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (60,2,'Teclado Mecanico Redragon','img',500,50)

INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (61,2,'Mouse Logitech','img',1000,100) 
INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (62,2,'Mouse Hyperx','img',1000,100) 
INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (63,2,'Mouse Asus','img',1000,100) 
INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (64,2,'Mouse Redragon','img',1000,100)
INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (65,2,'Mouse XPG','img',1000,100)
INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (66,2,'Mouse Cougar','img',1000,100)

INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (67,2,'Auriculares Logitech','img',1000,100)
INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (68,2,'Auriculares Hyperx','img',1000,100)
INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (69,2,'Auriculares Asus','img',1000,100) 
INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (70,2,'Auriculares Redragon','img',1000,100)

INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (71,2,'Mousepad Logitech','img',1000,100) 
INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (72,2,'Mousepad Hyperx','img',1000,100)
INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (73,2,'Mousepad Redragon','img',1000,100) 
INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (74,2,'Mousepad Corsair','img',1000,100)

INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (75,2,'Microfono Logitech','img',300,30)
INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (76,2,'Microfono Hyperx','img',300,30) 
INSERT INTO insumos(id_insumos,id_categoria,descripcion,img,stock,stockMin) VALUES (77,2,'Microfono Redragon','img',300,30)

/*Proveedores*/


INSERT INTO proveedores(id_proveedor,nombre,cuit,domicilio,email,telefono) VALUES(1,'JDS Informaticos','2016615621','Lavalle 3215','jdsinformaticos@gmail.com','3587485135') 
INSERT INTO proveedores(id_proveedor,nombre,cuit,domicilio,email,telefono) VALUES(2,'PC Arts','2025822051','Mendoza 654','pc_arts@gmail.com','3165862678') 
INSERT INTO proveedores(id_proveedor,nombre,cuit,domicilio,email,telefono) VALUES(3,'AFD Informatica','2023658912','Buenos Aires 783','afdinformatica@gmail.com','3716568715') 
INSERT INTO proveedores(id_proveedor,nombre,cuit,domicilio,email,telefono) VALUES(4,'NeoTech','2021726328','San Juan 5132','neotech@gmail.com','3387213947')

/*Insumo Proveedor*/

INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(1,1,10000.00,20000.00) 
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(2,1,20000.00,40000.00) 
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(3,1,40000.00,70000.00) 
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(4,1,50000.00,100000.00)
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(9,1,5000.00,10000.00) 
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(10,1,10000.00,20000.00) 
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(11,1,5000.00,10000.00) 
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(17,1,8000.00,16000.00)
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(18,1,12000.00,24000.00) 
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(19,1,50000.00,100000.00) 
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(20,1,80000.00,150000.00) 
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(21,1,100000.00,180000.00)
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(22,1,120000.00,240000.00)
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(33,1,5000.00,10000.00)
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(34,1,25000.00,50000.00)
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(35,1,25000.00,50000.00)
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(36,1,25000.00,50000.00)
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(37,1,25000.00,50000.00) 
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(52,1,25000.00,50000.00) 
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(53,1,28000.00,53000.00)

INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(5,2,10000.00,20000.00) 
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(6,2,10000.00,20000.00) 
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(7,2,10000.00,20000.00) 
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(8,2,10000.00,20000.00) 
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(12,2,10000.00,20000.00)
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(13,2,10000.00,20000.00)
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(14,2,10000.00,20000.00)
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(15,2,10000.00,20000.00)
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(16,2,10000.00,20000.00)
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(23,2,10000.00,20000.00)
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(24,2,10000.00,20000.00)
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(25,2,10000.00,120000.00)
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(26,2,10000.00,20000.00)
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(27,2,10000.00,20000.00)
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(28,2,10000.00,20000.00)
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(29,2,10000.00,20000.00)
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(30,2,10000.00,15000.00)
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(38,2,10000.00,20000.00)
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(39,2,5000.00,10000.00) 
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(40,2,10000.00,20000.00)

INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(41,3,10000.00,20000.00)
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(42,3,10000.00,20000.00)
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(43,3,10000.00,20000.00)
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(50,3,10000.00,20000.00)
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(51,3,10000.00,20000.00)
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(54,3,10000.00,20000.00)
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(55,3,10000.00,20000.00)
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(56,3,10000.00,20000.00)
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(57,3,10000.00,20000.00)
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(61,3,10000.00,20000.00)
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(62,3,10000.00,20000.00)
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(63,3,10000.00,20000.00)
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(67,3,10000.00,20000.00)
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(68,3,10000.00,20000.00)
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(71,3,10000.00,20000.00)
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(72,3,10000.00,20000.00)
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(75,3,10000.00,20000.00)

INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(31,4,10000.00,20000.00)
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(32,4,10000.00,20000.00)
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(44,4,10000.00,20000.00)
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(45,4,10000.00,20000.00)
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(46,4,10000.00,20000.00)
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(47,4,10000.00,20000.00)
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(48,4,10000.00,20000.00)
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(49,4,10000.00,20000.00)
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(58,4,10000.00,20000.00)
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(59,4,10000.00,20000.00)
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(60,4,10000.00,20000.00)
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(64,4,10000.00,20000.00)
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(65,4,10000.00,20000.00)
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(66,4,10000.00,20000.00)
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(69,4,10000.00,20000.00)
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(70,4,10000.00,20000.00)
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(73,4,10000.00,20000.00)
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(74,4,10000.00,20000.00)
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(76,4,10000.00,20000.00)
INSERT INTO insumos_proveedor(id_insumo,id_proveedor,precio_Costo,precio_Venta) VALUES(77,4,10000.00,20000.00)

/*venta_detalle*/ 


INSERT INTO venta_detalle(id_detalle,id_insumo,id_proveedor,cantidad,precio_unitario) VALUES (1,3,1,1,70000) 
INSERT INTO venta_detalle(id_detalle,id_insumo,id_proveedor,cantidad,precio_unitario) VALUES (2,10,1,1,20000)
INSERT INTO venta_detalle(id_detalle,id_insumo,id_proveedor,cantidad,precio_unitario) VALUES (3,20,1,1,150000)
INSERT INTO venta_detalle(id_detalle,id_insumo,id_proveedor,cantidad,precio_unitario) VALUES (4,33,1,1,10000)
INSERT INTO venta_detalle(id_detalle,id_insumo,id_proveedor,cantidad,precio_unitario) VALUES (5,3,1,1,70000) 
INSERT INTO venta_detalle(id_detalle,id_insumo,id_proveedor,cantidad,precio_unitario) VALUES (6,6,2,1,20000) 
INSERT INTO venta_detalle(id_detalle,id_insumo,id_proveedor,cantidad,precio_unitario) VALUES (7,12,2,1,20000)
INSERT INTO venta_detalle(id_detalle,id_insumo,id_proveedor,cantidad,precio_unitario) VALUES (8,25,2,1,120000)
INSERT INTO venta_detalle(id_detalle,id_insumo,id_proveedor,cantidad,precio_unitario) VALUES (9,39,2,1,10000)
INSERT INTO venta_detalle(id_detalle,id_insumo,id_proveedor,cantidad,precio_unitario) VALUES (10,30,2,1,15000)
INSERT INTO venta_detalle(id_detalle,id_insumo,id_proveedor,cantidad,precio_unitario) VALUES (11,41,3,1,20000)
INSERT INTO venta_detalle(id_detalle,id_insumo,id_proveedor,cantidad,precio_unitario) VALUES (12,55,3,1,20000)
INSERT INTO venta_detalle(id_detalle,id_insumo,id_proveedor,cantidad,precio_unitario) VALUES (13,63,3,1,20000)
INSERT INTO venta_detalle(id_detalle,id_insumo,id_proveedor,cantidad,precio_unitario) VALUES (14,72,3,1,20000)
INSERT INTO venta_detalle(id_detalle,id_insumo,id_proveedor,cantidad,precio_unitario) VALUES (15,75,3,1,20000)

/*venta_cabecera*/ 

INSERT INTO venta_cabecera(id_cabecera,id_detalle,fecha_y_hora,total,id_usuario,id_tipo_pago,cuit) VALUES (1,1,'20221019',70000,1,1,1111) 
INSERT INTO venta_cabecera(id_cabecera,id_detalle,fecha_y_hora,total,id_usuario,id_tipo_pago,cuit) VALUES (1,2,'20221019',20000,1,1,1111) 
INSERT INTO venta_cabecera(id_cabecera,id_detalle,fecha_y_hora,total,id_usuario,id_tipo_pago,cuit) VALUES (1,3,'20221019',150000,1,1,1111)
INSERT INTO venta_cabecera(id_cabecera,id_detalle,fecha_y_hora,total,id_usuario,id_tipo_pago,cuit) VALUES (1,4,'20221019',10000,1,1,1111) 
INSERT INTO venta_cabecera(id_cabecera,id_detalle,fecha_y_hora,total,id_usuario,id_tipo_pago,cuit) VALUES (1,5,'20221019',70000,1,1,1111)

INSERT INTO venta_cabecera(id_cabecera,id_detalle,fecha_y_hora,total,id_usuario,id_tipo_pago,cuit) VALUES (2,6,'20221025',20000,2,2,3333) 
INSERT INTO venta_cabecera(id_cabecera,id_detalle,fecha_y_hora,total,id_usuario,id_tipo_pago,cuit) VALUES (2,7,'20221025',20000,2,2,3333) 
INSERT INTO venta_cabecera(id_cabecera,id_detalle,fecha_y_hora,total,id_usuario,id_tipo_pago,cuit) VALUES (2,8,'20221025',20000,2,2,3333) 
INSERT INTO venta_cabecera(id_cabecera,id_detalle,fecha_y_hora,total,id_usuario,id_tipo_pago,cuit) VALUES (2,9,'20221025',20000,2,2,3333) 
INSERT INTO venta_cabecera(id_cabecera,id_detalle,fecha_y_hora,total,id_usuario,id_tipo_pago,cuit) VALUES (2,10,'20221025',20000,2,2,3333)

INSERT INTO venta_cabecera(id_cabecera,id_detalle,fecha_y_hora,total,id_usuario,id_tipo_pago,cuit) VALUES (3,11,'20221029',20000,3,4,5555)
INSERT INTO venta_cabecera(id_cabecera,id_detalle,fecha_y_hora,total,id_usuario,id_tipo_pago,cuit) VALUES (3,12,'20221029',20000,3,4,5555)
INSERT INTO venta_cabecera(id_cabecera,id_detalle,fecha_y_hora,total,id_usuario,id_tipo_pago,cuit) VALUES (3,13,'20221029',20000,3,4,5555)
INSERT INTO venta_cabecera(id_cabecera,id_detalle,fecha_y_hora,total,id_usuario,id_tipo_pago,cuit) VALUES (3,14,'20221029',20000,3,4,5555)
INSERT INTO venta_cabecera(id_cabecera,id_detalle,fecha_y_hora,total,id_usuario,id_tipo_pago,cuit) VALUES (3,15,'20221029',20000,3,4,5555)

use insumosinformaticos

--PROVINCIAS
--Procedimiento almacenado para insertar una provincia

create procedure InsertarProvincia
	@id_provincia int,
	@descripcion varchar(30)
AS
BEGIN
INSERT INTO provincias (id_provincia, descripcion) VALUES(@id_provincia, @descripcion)
END

create procedure EliminarProvincia
	@id_provincia int
AS
BEGIN
DELETE FROM provincias
where id_provincia= @id_provincia
END

create procedure ModificarProvincia
	@id_provincia int,
	@descripcion varchar(30)
AS
BEGIN
UPDATE provincias
SET
	descripcion = @descripcion 
where id_provincia= @id_provincia
END


--LOCALIDADES
--Procedimiento almacenado para insertar una localidad
create procedure InsertarLocalidad
	@id_provincia int,
	@id_localidad int,
	@descripcion varchar(30)
AS
BEGIN
INSERT INTO localidades(id_provincia,id_localidad, descripcion) VALUES(@id_provincia,@id_localidad, @descripcion)
END

create procedure EliminarLocalidad
	@id_provincia int,
	@id_localidad int
AS
BEGIN
DELETE FROM localidades
where id_provincia= @id_provincia and id_localidad= @id_localidad
END

create procedure ModificarLocalidad
	@id_provincia int,
	@id_localidad int,
	@descripcion varchar(30)
AS
BEGIN
UPDATE localidades
SET
	descripcion = @descripcion 
where id_provincia= @id_provincia and id_localidad= @id_localidad
END

--PERFILES
--Procedimiento almacenado para insertar un perfil

create procedure InsertarPerfil
	@id_perfil int,
	@descripcion varchar(20)
AS
BEGIN
INSERT INTO perfiles VALUES(@id_perfil,@descripcion)
END

create procedure EliminarPerfil
	@id_perfil int
AS
BEGIN
DELETE FROM perfiles
where id_perfil= @id_perfil
END

create procedure ModificarPerfil
	@id_perfil int,
	@descripcion varchar(20)
AS
BEGIN
UPDATE perfiles
SET
	descripcion = @descripcion 
where id_perfil= @id_perfil
END


---USUARIO----
--Procedimiento almacenado para insertar un usuario
create procedure InsertarUsuario
	@id_usuario INT,
	@id_perfil INT,
	@nom_y_ape VARCHAR(100),
	@contrasenia VARCHAR(8),
	@email VARCHAR(100),
	@domicilio VARCHAR(120),
	@telefono VARCHAR(15),
	@cod_provincia INT,
	@cod_localidad INT
AS
BEGIN
INSERT INTO usuarios VALUES(@id_usuario,@id_perfil,@nom_y_ape,@contrasenia,@email,@domicilio,@telefono,@cod_provincia,@cod_localidad)
END

create procedure EliminarUsuario
	@id_usuario int
AS
BEGIN
DELETE FROM usuarios
where id_usuario= @id_usuario
END

create procedure ModificarUsuario
	@id_usuario INT,
	@id_perfil INT,
	@nom_y_ape VARCHAR(100),
	@contrasenia VARCHAR(8),
	@email VARCHAR(100),
	@domicilio VARCHAR(120),
	@telefono VARCHAR(15),
	@cod_provincia INT,
	@cod_localidad INT
AS
BEGIN
UPDATE usuarios
SET
	id_usuario=@id_usuario,
	id_perfil=@id_perfil,
	nom_y_ape=@nom_y_ape,
	contrasenia=@contrasenia,
	email=@email,
	domicilio=@domicilio,
	telefono=@telefono,
	cod_provincia=@cod_provincia,
	cod_localidad=@cod_localidad
where id_usuario= @id_usuario
END


---TIPOPAGO---
--Procedimiento almacenado para insertar un tipopago
create procedure InsertarTipoPago
	@id_tipo_pago INT,
	@descripcion VARCHAR(30),
	@descuento REAL
AS
BEGIN
INSERT INTO tipo_pago VALUES(@id_tipo_pago,@descripcion,@descuento)
END

create procedure EliminarTipoPago
	@id_tipo_pago int
AS
BEGIN
DELETE FROM tipo_pago
where id_tipo_pago= @id_tipo_pago
END

create procedure ModificarTipoPago
	@id_tipo_pago INT,
	@descripcion VARCHAR(30),
	@descuento REAL
AS
BEGIN
UPDATE tipo_pago
SET
	id_tipo_pago=@id_tipo_pago,
	descripcion=@descripcion,
	descuento=@descuento
where id_tipo_pago= @id_tipo_pago
END

---SUCURSALES---
--Procedimiento almacenado para insertar una sucursal
create procedure InsertarSucursal
	@cuit int,
	@nombre VARCHAR(80),
	@direccion VARCHAR(120),
	@telefono VARCHAR(15),
	@email VARCHAR(100),
	@horarios VARCHAR(50),
	@URL VARCHAR(150)
AS
BEGIN
INSERT INTO sucursales VALUES(@cuit,@nombre,@direccion,@telefono,@email,@horarios,@URL)
END

create procedure EliminarSucursal
	@cuit int
AS
BEGIN
DELETE FROM sucursales
where cuit= @cuit
END

create procedure ModificarSucursal
	@cuit int,
	@nombre VARCHAR(80),
	@direccion VARCHAR(120),
	@telefono VARCHAR(15),
	@email VARCHAR(100),
	@horarios VARCHAR(50),
	@URL VARCHAR(150)
AS
BEGIN
UPDATE sucursales
SET
	cuit=@cuit,
	nombre=@nombre,
	direccion=@direccion,
	telefono=@telefono,
	email=@email,
	horarios=@horarios,
	URL=@URL
where cuit= @cuit
END


---CATEGORIAS---
--Procedimiento almacenado para insertar una categoria
create procedure InsertarCategoria
	@id_categoria INT,
	@descripcion VARCHAR(30)
AS
BEGIN
INSERT INTO categorias VALUES(@id_categoria,@descripcion)
END

create procedure EliminarCategoria
	@id_categoria int
AS
BEGIN
DELETE FROM categorias
where id_categoria= @id_categoria
END

create procedure ModificarCategoria
	@id_categoria INT,
	@descripcion VARCHAR(30)
AS
BEGIN
UPDATE categorias
SET
	id_categoria=@id_categoria,
	descripcion=@descripcion
where id_categoria= @id_categoria
END


---INSUMOS---
--Procedimiento almacenado para insertar un insumo
create procedure InsertarInsumo
	@id_insumos INT,
	@id_categoria INT,
	@descripcion VARCHAR(50),
	@img VARCHAR(150),
	@stock INT,
	@stockMin INT
AS
BEGIN
INSERT INTO insumos VALUES(@id_insumos,@id_categoria,@descripcion,@img,@stock,@stockMin)
END

create procedure EliminarInsumo
	@id_insumos int
AS
BEGIN
DELETE FROM insumos
where id_insumos= @id_insumos
END

create procedure ModificarInsumo
	@id_insumos INT,
	@id_categoria INT,
	@descripcion VARCHAR(50),
	@img VARCHAR(150),
	@stock INT,
	@stockMin INT
AS
BEGIN
UPDATE insumos
SET
	id_insumos=@id_insumos,
	id_categoria=@id_categoria,
	descripcion=@descripcion,
	img=@img,
	stock=@stock,
	stockMin=@stockMin
where id_insumos= @id_insumos
END

---PROVEEDORES---
--Procedimiento almacenado para insertar un proveedor
create procedure InsertarProveedor
	@id_proveedor INT,
	@nombre VARCHAR(80),
	@cuit VARCHAR(10),
	@domicilio VARCHAR(120),
	@email VARCHAR(100),
	@telefono VARCHAR(15)
AS
BEGIN
INSERT INTO proveedores VALUES(@id_proveedor,@nombre,@cuit,@domicilio,@email,@telefono)
END

create procedure EliminarProveedor
	@id_proveedor int
AS
BEGIN
DELETE FROM proveedores
where id_proveedor= @id_proveedor
END

create procedure ModificarProveedor
	@id_proveedor INT,
	@nombre VARCHAR(80),
	@cuit VARCHAR(10),
	@domicilio VARCHAR(120),
	@email VARCHAR(100),
	@telefono VARCHAR(15)
AS
BEGIN
UPDATE proveedores
SET
	id_proveedor=@id_proveedor,
	nombre=@nombre,
	cuit=@cuit,
	domicilio=@domicilio,
	email=@email,
	telefono=@telefono
where id_proveedor= @id_proveedor
END


---INSUMO-PROVEEDOR---
--Procedimiento almacenado para insertar un insumo-proveedor
create procedure InsertarInsumoProveedor
	@id_insumo INT,
	@id_proveedor INT,
	@precio_Costo REAL,
	@precio_Venta REAL
AS
BEGIN
INSERT INTO insumos_proveedor VALUES(@id_insumo,@id_proveedor,@precio_Costo,@precio_Venta)
END

create procedure EliminarInsumoProveedor
	@id_insumo int,
	@id_proveedor int
AS
BEGIN
DELETE FROM insumos_proveedor
where id_insumo=@id_insumo and id_proveedor= @id_proveedor
END

create procedure ModificarInsumoProveedor
	@id_insumo INT,
	@id_proveedor INT,
	@precio_Costo REAL,
	@precio_Venta REAL
AS
BEGIN
UPDATE insumos_proveedor
SET
	id_insumo=@id_insumo,
	id_proveedor=@id_proveedor,
	precio_Costo=@precio_Costo,
	precio_Venta=@precio_Venta
where id_insumo= @id_insumo and id_proveedor= @id_proveedor
END



---VENTADETALLE---
--Procedimiento almacenado para insertar una ventadetalle
create procedure InsertarVentaDetalle
	@id_detalle INT,
	@id_insumo INT,
	@id_proveedor INT,
	@cantidad INT,
	@precio_unitario REAL
AS
BEGIN
INSERT INTO venta_detalle VALUES(@id_detalle,@id_insumo,@id_proveedor,@cantidad,@precio_unitario)
END

create procedure EliminarVentaDetalle
	@id_detalle INT
AS
BEGIN
DELETE FROM venta_detalle
where id_detalle=@id_detalle
END

create procedure ModificarVentaDetalle
	@id_detalle INT,
	@id_insumo INT,
	@id_proveedor INT,
	@cantidad INT,
	@precio_unitario REAL
AS
BEGIN
UPDATE venta_detalle
SET
	id_detalle=@id_detalle,
	id_insumo=@id_insumo,
	id_proveedor=@id_proveedor,
	cantidad=@cantidad,
	precio_unitario=@precio_unitario
where id_detalle=@id_detalle
END

---VENTACABECERA---
--Procedimiento almacenado para insertar una ventadetalle
create procedure InsertarVentaCabecera
	@id_cabecera INT,
	@id_detalle INT,
	@fecha_y_hora DATETIME,
	@total REAL,
	@id_usuario INT,
	@id_tipo_pago INT,
	@cuit INT
AS
BEGIN
INSERT INTO venta_cabecera VALUES(@id_cabecera,@id_detalle,@fecha_y_hora,@total,@id_usuario,@id_tipo_pago,@cuit)
END

create procedure EliminarVentaCabecera
	@id_cabecera INT
AS
BEGIN
DELETE FROM venta_cabecera
where id_cabecera=@id_cabecera
END

create procedure ModificarVentaCabecera
	@id_cabecera INT,
	@id_detalle INT,
	@fecha_y_hora DATETIME,
	@total REAL,
	@id_usuario INT,
	@id_tipo_pago INT,
	@cuit INT
AS
BEGIN
UPDATE venta_cabecera
SET
	id_cabecera=@id_cabecera,
	id_detalle=@id_detalle,
	fecha_y_hora=@fecha_y_hora,
	total=@total,
	id_usuario=@id_usuario,
	id_tipo_pago=@id_tipo_pago,
	cuit=@cuit
where id_cabecera=@id_cabecera
END


------Triggers----
--tabla en la que se almacenan los datos de la operacion que se hace sobre la tabla escribano y el usuario que la realizo junto con la fecha y hora----
create table actualizacioninsumos(
	id_insumos INT,
	id_categoria INT NOT NULL,
	descripcion VARCHAR(50) NOT NULL,
	img VARCHAR(150) NOT NULL,
	stock INT NOT NULL,
	stockMin INT NOT NULL,
	operacion varchar(30),
	usuario varchar(100),
	fecha datetime
)
select * from actualizacioninsumos

--Disparador que almacena datos cada vez que se hace un insert en la tabla Insumos
CREATE trigger insertInsumo
  on insumos
  after insert
 as 
 begin
	INSERT INTO actualizacioninsumos(id_insumos,id_categoria,descripcion,img,stock,stockMin,operacion,usuario,fecha)
   Select 
		i.id_insumos,
		i.id_categoria,
		i.descripcion,
		i.img,
		i.stock,
		i.stockMin,
		'Insert',
		system_user,
		GETDATE()
   from inserted i
end



--Disparador que almacena datos cada vez que se hace un update en la tabla Insumos
CREATE trigger updateInsumo
	on insumos
	after update 
as
 begin
	INSERT INTO actualizacioninsumos(id_insumos,id_categoria,descripcion,img,stock,stockMin,operacion,usuario,fecha)
   Select 
		d.id_insumos,
		d.id_categoria,
		d.descripcion,
		d.img,
		d.stock,
		d.stockMin,
		'update datos viejos',
		system_user,
		GETDATE()
   from deleted d
   INSERT INTO actualizacioninsumos(id_insumos,id_categoria,descripcion,img,stock,stockMin,operacion,usuario,fecha)
   Select 
		i.id_insumos,
		i.id_categoria,
		i.descripcion,
		i.img,
		i.stock,
		i.stockMin,
		'update datos nuevos',
		system_user,
		GETDATE()
   from inserted i
end

--Disparador que almacena datos cada vez que se hace un delete en la tabla Insumos
create trigger deleteInsumo
  on insumos
  after delete
 as 
 begin
	INSERT INTO actualizacioninsumos(id_insumos,id_categoria,descripcion,img,stock,stockMin,operacion,usuario,fecha)
   Select 
		d.id_insumos,
		d.id_categoria,
		d.descripcion,
		d.img,
		d.stock,
		d.stockMin,
		'delete',
		system_user,
		GETDATE()
   from deleted d
end


--Transacciones-- 
--Operacion de transaccion que realiza la modificacion de dos registros de diferentes tablas y el insert de otro

declare @Error int 
begin tran

exec ModificarUsuario 4,1,'Alcaraz Sonia','soniaalc','AlvaroGonzales@gmail.com','Barrio laguna soto sector 3 casa 13','374449276',2,1

exec ModificarLocalidad 2, 1, 'Madariaga'

exec InsertarInsumo 78,1,'Placa Madre Gigabyte Micro ATX Cromada','img',500,50

set @Error = @@Error 
--En caso de error, se hace un rollback para que los datos vuelvan a un estado anterior a las modificaciones 
if (@Error <> 0) 
begin 
rollback tran 
print 'Error en la transaccion' 
end 
else 
commit 
select * from insumos 
select * from localidades
select * from usuar
