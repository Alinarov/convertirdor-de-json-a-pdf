import json
from decimal import Decimal

import tkinter
import time 
from app import IndicadorCarga
import customtkinter as ctk
from comunicacion import SQLServerConnector

con = SQLServerConnector()
con.connect()


respuesta_sql = con.execute_query("""		SELECT e.nombre_empleado, e.apellidos_empleado, e.dni_empleado, c.peso_carga, c.id_fecha_carga, c.hora_entrega
		FROM empleado em
		INNER JOIN registro_empleados e ON em.id_registro_personal = e.id_registro_personal
		INNER JOIN cargas c ON em.id_empleado = c.id_empleado
		INNER JOIN fecha_carga fc ON c.id_fecha_carga = fc.id_fecha_carga
		WHERE c.id_fecha_carga = '2024-03-20'
		ORDER BY e.nombre_empleado""")


# Estructura de datos para almacenar la información
data = {}

# Procesar cada fila de la respuesta SQL
for nombre, apellidos, dni, peso_carga, id_fecha_carga, hora_entrega in respuesta_sql:
    if id_fecha_carga not in data:
        data[id_fecha_carga] = {}
    
    if dni not in data[id_fecha_carga]:
        data[id_fecha_carga][dni] = {
            "nombres_apellidos": f"{nombre} {apellidos}",
            "total_carga": 0,
            "dni": dni,
            "n_cargas": 0,
            "cargas": []
        }
    
    # Actualizar el total de carga y el número de cargas
    data[id_fecha_carga][dni]["total_carga"] += float(peso_carga)
    data[id_fecha_carga][dni]["n_cargas"] += 1
    
    # Agregar carga a la lista de cargas
    data[id_fecha_carga][dni]["cargas"].append([float(peso_carga), hora_entrega])

# Convertir a formato JSON
json_data = json.dumps(data, indent=4)

# Imprimir el JSON resultante
print(json_data)
