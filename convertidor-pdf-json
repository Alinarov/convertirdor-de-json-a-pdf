import json
from reportlab.lib.pagesizes import letter
from reportlab.pdfgen import canvas
from reportlab.lib.colors import black

def crear_pdf_desde_json(json_file, output_pdf):
    with open(json_file, "r", encoding='utf-8') as file:
        data = json.load(file)

    c = canvas.Canvas(output_pdf, pagesize=letter)
    y_position = 750  # Posición vertical inicial
    page_height = letter[1]

    # Ordenar fechas de carga
    fechas_ordenadas = sorted(data.keys())

    # Título en la primera página
    c.setFont("Helvetica-Bold", 14)
    c.drawString(100, y_position, "Registros de las cargas de los empleados hechas en la fecha")
    y_position -= 30  # Espacio después del título

    first_page = True

    for fecha in fechas_ordenadas:
        empleados = data[fecha]

        for dni in sorted(empleados.keys()):
            detalles = empleados[dni]

            # Datos del empleado
            empleado_info = [
                f"FECHA: {fecha}",
                f"NOMBRES: {detalles['nombres_apellidos']}",
                f"TOTAL DE CARGA EN EL DIA: {detalles['total_carga']} Kg",
                f"DNI: {dni}",
                f"N CARGAS: {detalles['n_cargas']}"
            ]

            # Verificar espacio necesario para los datos del empleado
            espacio_necesario = (len(empleado_info) + 2) * 20 + 100
            if not first_page and y_position - espacio_necesario < 80:
                c.showPage()
                y_position = page_height - 120  # Reiniciar posición vertical

            # Escribir datos del empleado con tamaño de letra 11
            c.setFont("Helvetica", 11)
            for line in empleado_info:
                c.drawString(100, y_position, line)
                y_position -= 18  # Espacio de línea para tamaño de letra 11

            # Encabezados de la tabla con tamaño de letra 11
            c.setFont("Helvetica-Bold", 11)
            c.drawString(100, y_position - 15, "PESO DE CARGA")
            c.drawString(250, y_position - 15, "HORA DE REGISTRO")
            y_position -= 10

            # Líneas horizontales del encabezado de la tabla
            c.setLineWidth(0.5)
            c.line(80, y_position + 10, 500, y_position + 10)  # Línea superior
            c.line(80, y_position - 10, 500, y_position - 10)  # Línea inferior

            y_position -= 20

            # Escribir datos de la tabla con tamaño de letra 11
            c.setFont("Helvetica", 11)
            for carga in detalles["cargas"]:
                if y_position < 100:
                    c.showPage()
                    y_position = page_height - 120  # Reiniciar posición vertical

                c.drawString(100, y_position - 5, str(carga[0]))  # Peso de carga
                c.drawString(250, y_position - 5, carga[1])  # Hora de registro

                # Líneas horizontales de la tabla
                c.line(80, y_position - 10, 500, y_position - 10)

                y_position -= 18  # Espacio de línea para tamaño de letra 11

            y_position -= 20  # Espacio después de la tabla

            # Indicador de separación de registros de empleados
            if not first_page and y_position < 100:
                c.showPage()
                y_position = page_height - 120  # Reiniciar posición vertical

            draw_fancy_line(c, y_position + 10)
            y_position -= 20  # Espacio después del indicador

            first_page = False

    c.save()

def draw_fancy_line(canvas, y_position):
    # Dibujar línea decorativa
    canvas.setStrokeColor(black)
    canvas.setLineWidth(1)
    canvas.line(80, y_position, 100, y_position)  # Línea izquierda
    canvas.drawString(100, y_position, "] ------ [")
    text_width = canvas.stringWidth("] ------ [", "Helvetica", 10)
    canvas.line(100 + text_width, y_position, 500, y_position)  # Línea derecha

# Uso de la función
json_file = "temp.json"
output_pdf = "registro_carga.pdf"
crear_pdf_desde_json(json_file, output_pdf)
