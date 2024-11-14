# Contenedor SonarQube con Docker

Este proyecto configura un entorno de SonarQube en contenedores Docker, con una base de datos PostgreSQL como almacenamiento. El archivo `docker-compose.yml` configura ambos servicios: SonarQube y PostgreSQL, y los conecta a través de una red Docker privada.

## Estructura del Proyecto

- `docker-compose.yml`: Define los servicios de SonarQube y PostgreSQL, configurando redes y variables de entorno necesarias para la ejecución.

## Contenido del `docker-compose.yml`

El archivo `docker-compose.yml` define dos servicios principales:

### Servicios

- **sonarqube**:
  - Usa la imagen `sonarqube:latest` para configurar el servidor de SonarQube.
  - Expone el puerto `9000` para acceder a la interfaz de SonarQube.
  - Configura variables de entorno relacionadas con la conexión a la base de datos PostgreSQL.
  - La configuración incluye opciones de memoria para SonarQube y Elasticsearch a través de `SONARQUBE_ES_JAVA_OPTS` y `SONARQUBE_JAVA_OPTS`.
  - Depende del servicio `db` (PostgreSQL), por lo que espera que la base de datos esté en funcionamiento antes de iniciar.
  
- **db**:
  - Usa la imagen `postgres:12` para configurar la base de datos PostgreSQL.
  - Establece las credenciales de la base de datos (`POSTGRES_USER`, `POSTGRES_PASSWORD`, `POSTGRES_DB`) para la conexión desde SonarQube.
  - Está conectado a la misma red `sonarnet` que el servicio SonarQube para permitir la comunicación entre ambos servicios.

### Redes

- **sonarnet**: Define una red interna para que los contenedores de SonarQube y PostgreSQL puedan comunicarse de manera segura.

## Instrucciones para Ejecutar el Proyecto con Docker Compose

### Prerequisitos

Asegúrate de tener Docker y Docker Compose instalados en tu sistema.

### Pasos para Ejecutar

1. **Clona el Repositorio**:
   Si aún no has clonado el proyecto, usa el siguiente comando:
   ```bash
   git clone <URL-del-repositorio>
   cd Contenedor_Sonarqube
   ```

2. **Construye y Levanta los Contenedores**:
   Ejecuta el siguiente comando para construir y levantar los servicios definidos en el archivo `docker-compose.yml`:
   ```bash
   docker-compose up --build
   ```

   Esto descargará las imágenes necesarias (si no están ya disponibles localmente) y levantará los contenedores de SonarQube y PostgreSQL.

3. **Accede a SonarQube**:
   Una vez que los contenedores estén en ejecución, abre tu navegador e ingresa a la siguiente URL para acceder a la interfaz web de SonarQube:
   ```bash
   http://localhost:9000
   ```

   - El nombre de usuario predeterminado es `admin` y la contraseña es `admin`.

4. **Verifica los Contenedores en Ejecución**:
   Puedes verificar que los contenedores estén en ejecución con:
   ```bash
   docker ps
   ```

   Verás dos contenedores en ejecución: uno para `sonarqube` y otro para `postgres`.

5. **Detener los Contenedores**:
   Para detener los servicios de SonarQube y PostgreSQL, ejecuta:
   ```bash
   docker-compose down
   ```

   Esto detendrá y eliminará los contenedores, pero preservará los datos de SonarQube almacenados en el volumen de la base de datos.

### Variables de Entorno

- **SONARQUBE_JDBC_URL**: La URL de conexión a la base de datos PostgreSQL (en este caso, `jdbc:postgresql://db:5432/sonarqube`).
- **SONARQUBE_JDBC_USERNAME**: El nombre de usuario para la base de datos de SonarQube (en este caso, `sonar`).
- **SONARQUBE_JDBC_PASSWORD**: La contraseña para el usuario de la base de datos de SonarQube (en este caso, `sonar`).
- **SONARQUBE_ES_JAVA_OPTS**: Configuración de memoria para Elasticsearch (en este caso, `-Xms512m -Xmx512m`).
- **SONARQUBE_JAVA_OPTS**: Configuración de memoria para Java (en este caso, `-Xms512m -Xmx512m`).
- **POSTGRES_USER**: El nombre de usuario para la base de datos PostgreSQL (en este caso, `sonar`).
- **POSTGRES_PASSWORD**: La contraseña para el usuario de la base de datos PostgreSQL (en este caso, `sonar`).
- **POSTGRES_DB**: El nombre de la base de datos PostgreSQL (en este caso, `sonarqube`).

## Notas

- SonarQube requiere al menos 2 GB de memoria para ejecutarse de manera eficiente. Si experimentas problemas de rendimiento, puedes ajustar las opciones de memoria en el archivo `docker-compose.yml` bajo las variables `SONARQUBE_ES_JAVA_OPTS` y `SONARQUBE_JAVA_OPTS`.
- Asegúrate de tener suficiente espacio en disco, ya que SonarQube puede generar archivos grandes dependiendo del tamaño de los proyectos analizados.

## Contribuciones

Si encuentras problemas o tienes sugerencias para mejorar este proyecto, no dudes en abrir un _issue_ o enviar una _pull request_.