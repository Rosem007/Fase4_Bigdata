# Fase4_Bigdata
Diseño de la base de datos 

En este proyecto se diseña una base de datos NoSQL en MongoDB para el almacenamiento y consulta de casos de COVID-19 reportados en Colombia.
Cada registro corresponde a un caso individual e incluye información:
•	Demográficas: edad y sexo.
•	Geográficas: departamento y municipio (códigos DIVIPOLA y nomenclaturas).
•	Clínicas: estado del caso reportado (leve, grave o fallecido), dónde se encontraba el paciente (casa, hospital o fallecido), forma de recuperación.
•	Temporales: las fechas de notificación, inicio de síntomas, diagnóstico, recuperación o muerte.
Se hace precisamente uso de MongoDB porque:
•	Permite trabajar documentos CSV con una estructura flexible.
•	Está dotado para soportar el almacenamiento de datos semiestructurados que provengan de archivos planos o de reportes oficiales;
•	Soporta consultar mediante filtros y/o agregaciones (por departamento, edad, tipo de contagio) de una forma sencilla y rápida.


Contexto del Problema: 
Los datos oficiales del COVID-19 contienen millones de registros con información diversa: características clínicas, ubicación geográfica, fechas clave, evolución del paciente, entre otros. Su volumen y variabilidad representan un reto para sistemas tradicionales basados en SQL.
Se requiere:

•	Un motor que soporte datos semiestructurados.
•	Alta velocidad en consultas analíticas.
•	Flexibilidad para cambios en el esquema.
•	Facilidad para cargar archivos masivos provenientes de CSV.

MongoDB resulta adecuado para este tipo de información porque no exige esquemas rígidos y permite procesar datos con estructuras variadas.

Diseño de la Solución
El proyecto se desarrolla sobre MongoDB Shell e incluye:
Estructura de la base de datos
Cada documento representa un caso positivo e incluye:
•	Datos demográficos: Edad, sexo.
•	Datos geográficos: Departamento y municipio (códigos DIVIPOLA).
•	Dimensión clínica: Estado del paciente (leve, grave, crítico, fallecido), tipo de recuperación, ubicación (casa, hospital, UCI).
•	Fechas relevantes: Notificación, inicio de síntomas, diagnóstico, recuperación o muerte.
Este diseño permite consultas transversales sin la necesidad de relaciones complejas.

Carga de datos
•	Instalación del shell de MongoDB.
•	Creación de carpeta data.
•	Configuración de variables de entorno.

•	Importación del archivo Casos_positivos_de_COVID-19_en_Colombia.csv a una colección llamada casos_covid.

 
#Administracion de la base de datos: 
#Se crea la base de datos y se carga el archivo de Casos_positivos_de_COVID-19_en_Colombia.csv.
 

#Consulta  de

{'Nombre departamento': "VALLE"}

 
#Consultar 
db.casos_covid.find({ "ID de caso": 1556980 })
 

# Consultas básicas

8.	Se Seleccionar todos los documentos con
   db.casos_covid.find()
 

# Seleccionar con condiciones (filtro)

#Ejecutamos la siguiente linea de comando para seleccionar con filtr o 

db.casos_covid.find({ "Sexo": "F" })
 
#Actualizamos el documento

db.casos_covid.updateOne({ _id: "691a9f2cf7744d8b02bd29a8" },  { $set: { Estado: "Crítico" } })
 

#Eliminar un caso

db.casos_covid.deleteOne({ "ID de caso": 1137450 })

 

#Consultas con filtros y operadores

#Mujeres mayores de 60 años en estado Leve
db.casos_covid.find({
  "Sexo": "F",
  "Edad": { $gt: 60 },
  "Estado": "Grave"
})
 

#Casos comunitarios en el departamento VALLE
 
#Consultas de agregación

#Total de casos por departamento: db.casos_covid.aggregate([
 { $group: { _id: "$Nombre departamento", total: { $sum: 1 } } },
  { $sort: { total: -1 } }
])
 
#Promedio de edad por tipo de contagio:  db.casos_covid.aggregate([
 { $group: { _id: "$Tipo de contagio", edad_promedio: { $avg: "$Edad" } } }
])
 

#Cantidad de casos por estado clínico: db.casos_covid.aggregate([
 { $group: { _id: "$Estado", cantidad: { $sum: 1 } } }
])
