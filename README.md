# Limpieza-de-datos-

por: David Alejandro Restrepo Tuberquia

Este documento detalla el proceso sistemático de limpieza y preparación aplicado al dataset IOT-temp.csv, el cual contiene 97,606 registros de mediciones de temperatura de dispositivos IoT. El objetivo principal fue corregir problemas de tipos de datos, inconsistencias de formato (especialmente en la columna de tiempo) y la presencia de datos faltantes, dejando el dataset en un estado óptimo para el análisis de series de tiempo.

1. Diagnóstico Inicial de la Calidad de Datos
El análisis inicial (df.info()) reveló que solo la columna de temperatura (temp) era numérica, mientras que las cuatro columnas clave para el análisis estaban definidas de manera ineficiente o incorrecta como tipo object.

Limpieza de datos
Este documento detalla el proceso sistemático de limpieza y preparación aplicado al dataset IOT-temp.csv, el cual contiene 97,606 registros de mediciones de temperatura de dispositivos IoT. El objetivo principal fue corregir problemas de tipos de datos, inconsistencias de formato (especialmente en la columna de tiempo) y la presencia de datos faltantes, dejando el dataset en un estado óptimo para el análisis de series de tiempo.

1. Diagnóstico Inicial de la Calidad de Datos
El análisis inicial (df.info()) reveló que solo la columna de temperatura (temp) era numérica, mientras que las cuatro columnas clave para el análisis estaban definidas de manera ineficiente o incorrecta como tipo object.

Columna	Tipo Inicial	Problemática
noted_date	object	CRÍTICA: Debe ser datetime64 para el análisis temporal.
temp	int64	Pérdida potencial de precisión decimal (debe ser float64).
room_id/id	object	Carácter especial / en el nombre y tipo de dato ineficiente.
out/in	object	Carácter especial / en el nombre y tipo de dato ineficiente.
2. Acciones y Resultados de la Limpieza
Se aplicaron los siguientes pasos de transformación, abordando cada problemática:

A. Renombrado y Optimización Estructural
Columnas Renombradas: room_id/id fue renombrada a room_id y out/in a location para eliminar los caracteres especiales y mejorar la sintaxis.

Duplicados Eliminados: Se detectó y eliminó 1 fila duplicada.

Conversión a float: La columna temp fue convertida a float64 para asegurar la precisión decimal.

Optimización de Memoria: Las columnas id, room_id, y location fueron convertidas a tipo category para un uso más eficiente de la memoria.

Estandarización Categórica: Los valores en location fueron convertidos a minúsculas (.str.lower()) para garantizar consistencia (ej. "In" vs "in").

B. Corrección Crítica de la Columna noted_date
La columna de fecha y hora fue el mayor desafío debido a la inconsistencia de formato:

Fallo Inicial: El intento inicial de conversión falló, identificando un error de formato (time data "30-11-2018 23:58" doesn't match format "%m-%d-%Y %H:%M").

Solución de Formato: Se especificó el formato correcto (DATE_FORMAT = '%d-%m-%Y %H:%M') y se usó errors='coerce' para convertir los datos que no coincidieran a NaT (Not a Time).

Tratamiento de Nulos: Esta conversión reveló que 47,662 filas contenían datos de fecha inválidos (NaT).

Limpieza Final: Las 47,662 filas con fechas inválidas fueron eliminadas con df.dropna(subset=['noted_date']), ya que no se podían utilizar en un análisis temporal.

3. Estado Final del Dataset
Después de la limpieza, el DataFrame queda con 49,943 entradas (una reducción de 47,663 filas debido a duplicados y datos inválidos)

Rango Temporal Final: El dataset depurado cubre mediciones desde el 2018-01-11 00:06:00 hasta el 2018-12-10 23:55:00
