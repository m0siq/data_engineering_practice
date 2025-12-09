🚀 Guía de Instalación y Ejecución
Esta guía explica cómo desplegar y ejecutar el pipeline ELT de Ventas Walmart desde cero en un entorno local.

📋 1. Requisitos Previos (Prerequisites)
Antes de empezar, asegúrate de tener instalado:

Docker Desktop (Debe estar abierto y corriendo).

Python 3.10+

Git

📥 2. Clonar el Repositorio
Abre tu terminal (PowerShell o CMD) y descarga el proyecto:

Bash

git clone https://github.com/m0siq/data_engineering_practice.git
cd data_engineering_practice
🐳 3. Levantar la Infraestructura (Docker)
El proyecto utiliza PostgreSQL en un contenedor Docker. Hemos configurado el puerto 5434 para evitar conflictos con otras bases de datos que puedas tener instaladas.

Ejecuta:

Bash

docker compose up -d
Verificación: Ejecuta docker ps. Deberías ver el contenedor mi_data_warehouse corriendo en 0.0.0.0:5434->5432/tcp.

🐍 4. Configurar el Entorno Python
Para evitar conflictos de librerías, crearemos un entorno virtual e instalaremos las dependencias necesarias (Pandas, SQLAlchemy, dbt).

En Windows:

PowerShell

# Crear entorno
python -m venv venv

# Activar entorno
.\venv\Scripts\activate

# Instalar librerías
pip install pandas sqlalchemy psycopg2-binary dbt-core dbt-postgres
En Mac/Linux:

Bash

python3 -m venv venv
source venv/bin/activate
pip install pandas sqlalchemy psycopg2-binary dbt-core dbt-postgres
🚚 5. Fase 1: Ingesta de Datos (Extract & Load)
Ejecutaremos el script ETL que lee el archivo data/Walmart_Sales.csv y carga los datos crudos en PostgreSQL.

Ejecuta desde la carpeta raíz:

Bash

python scripts/carga_inicial.py
✅ Resultado esperado: Verás un mensaje que dice 🎉 ¡ÉXITO TOTAL! Datos cargados en 'raw_ventas'.

⚙️ 6. Fase 2: Transformación (Transform con dbt)
Ahora que los datos están cargados, usaremos dbt para limpiar, enriquecer y agregar los datos.

Entra en la carpeta del proyecto dbt:

Bash

cd dbt_project
Ejecuta la construcción de modelos y tests:

Bash

dbt build
✅ Resultado esperado: Verás una lista de modelos (stg_sales, int_sales_metrics, mart_analisis_tiendas) todos en color verde con el mensaje PASS o Completed successfully.

📊 7. Visualizar la Documentación
Para ver el linaje de datos (el mapa visual del proyecto) y el diccionario de datos:

Bash

dbt docs generate
dbt docs serve --port 8001
(Usamos el puerto 8001 para evitar errores de permisos en Windows).

👉 Abre en tu navegador: http://localhost:8001

🏆 8. Verificación Final (Opcional)
Si quieres ver los datos finales calculados (Top 5 Tiendas) directamente en tu terminal:

Abre una nueva terminal (o cancela la documentación con Ctrl+C), vuelve a la raíz y ejecuta:

Bash

cd ..
python scripts/verificar_final.py
🛑 Cómo detener todo
Cuando termines de trabajar, puedes apagar la base de datos para ahorrar recursos:

Bash

docker compose down