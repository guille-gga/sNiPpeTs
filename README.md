# sNiPpeTs

```
{
	"Examen Schema": {
		"prefix": "db-schema",
		"body": [
			"CREATE DATABASE IF NOT EXISTS alquiler_vehiculos;",
			"USE alquiler_vehiculos;",
			"",
			"CREATE TABLE marcas (id INT AUTO_INCREMENT PRIMARY KEY, nombre VARCHAR(50) NOT NULL UNIQUE);",
			"",
			"CREATE TABLE modelos (",
			"  id INT AUTO_INCREMENT PRIMARY KEY,",
			"  marca_id INT,",
			"  nombre VARCHAR(50) NOT NULL,",
			"  precio_dia DECIMAL(10, 2) NOT NULL,",
			"  FOREIGN KEY (marca_id) REFERENCES marcas(id)",
			");",
			"",
			"CREATE TABLE centros (",
			"  id INT AUTO_INCREMENT PRIMARY KEY,",
			"  ciudad VARCHAR(50) NOT NULL,",
			"  direccion VARCHAR(100) NOT NULL",
			");",
			"",
			"CREATE TABLE vehiculos (",
			"  id INT AUTO_INCREMENT PRIMARY KEY,",
			"  modelo_id INT, centro_id INT,",
			"  kilometros INT DEFAULT 4000,",
			"  FOREIGN KEY (modelo_id) REFERENCES modelos(id),",
			"  FOREIGN KEY (centro_id) REFERENCES centros(id)",
			");",
			"",
			"INSERT INTO marcas (nombre) VALUES ('Toyota'), ('Ford'), ('Volkswagen'), ('BMW'), ('Mercedes');",
			"",
			"INSERT INTO modelos (marca_id, nombre, precio_dia) VALUES ",
			"((SELECT id FROM marcas WHERE nombre='Toyota'), 'Corolla', 50.00),",
			"((SELECT id FROM marcas WHERE nombre='Ford'), 'Focus', 55.00),",
			"((SELECT id FROM marcas WHERE nombre='Volkswagen'), 'Golf', 50.00),",
			"((SELECT id FROM marcas WHERE nombre='BMW'), 'X3', 95.00),",
			"((SELECT id FROM marcas WHERE nombre='Mercedes'), 'C-Class', 120.00);",
			"",
			"INSERT INTO centros (ciudad, direccion) VALUES ",
			"('Granada', 'Avenida Andaluces 18'),",
			"('Malaga', 'Avenida Comandante García 12'),",
			"('Malaga', 'Centro Comercial Vialia'),",
			"('Sevilla', 'Estación Santa Justa P2');",
			"",
			//docker-compose up -d --build
			//docker exec -it mysql_mycli_server mycli -u uuussseeerrr -p pppaaasss alquiler_vehiculos
			//docker logs mysql_mycli_server
			"INSERT INTO vehiculos (modelo_id, centro_id, kilometros)",
			"SELECT m.id, c.id, 4000 FROM modelos m CROSS JOIN centros c;"
		],
		"description": "Genera el esquema completo del examen"
	},
	"Examen Joins": {
		"prefix": "db-joins",
		"body": [
			"-- Join 1: Vehículos y precios",
			"SELECT ma.nombre AS Marca, mo.nombre AS Modelo, mo.precio_dia, v.kilometros ",
			"FROM vehiculos v JOIN modelos mo ON v.modelo_id = mo.id JOIN marcas ma ON mo.marca_id = ma.id;",
			"",
			"-- Join 2: Ubicación completa",
			"SELECT c.ciudad, c.direccion, mo.nombre, ma.nombre, v.kilometros ",
			"FROM vehiculos v JOIN modelos mo ON v.modelo_id = mo.id ",
			"JOIN marcas ma ON mo.marca_id = ma.id JOIN centros c ON v.centro_id = c.id;"
		]
	}
}
