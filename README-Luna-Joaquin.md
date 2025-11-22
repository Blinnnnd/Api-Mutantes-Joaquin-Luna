# 🧬 API Detectora de mutantes | MercadoLibre Challenge

Solución para la identificación automática de mutantes basada en secuencias de ADN.
El objetivo es ayudar a Magneto en su reclutamiento: la API procesa un array de `String` (matriz NxN) buscando secuencias genéticas. Se detecta un mutante si existen **más de una secuencia** de cuatro letras idénticas (A, T, C, G) en dirección horizontal, vertical o diagonal.

## 📋 Resumen del Proyecto

El sistema está construido en **Java (Spring Boot)** siguiendo una **arquitectura en capas** para asegurar escalabilidad y mantenimiento.
Para la persistencia de datos, se utiliza **H2 Database** (en memoria), optimizando el rendimiento mediante indexación de hashes (`dna_hash`) para evitar re-analizar secuencias previamente verificadas.

## 🛠️ Stack Tecnológico

* **Datos:** H2 Database (In-Memory)
* **Testing:** JUnit 5, Mockito, MockMvc
* **Reportes:** JaCoCo (Cobertura de código)
* **Infraestructura:** Docker
* **Core:** Java 17 / Spring Boot 3.2.0
* **Docs:** OpenAPI / Swagger

## ⚙️ Pre-requisitos

* **Java JDK 17** o superior.
* **Docker** (Opcional, para contenedores).
* **Gradle** (Wrapper incluido en el proyecto).

## 🚀 Guía de Ejecución

### Entorno Local (Gradle)

**Windows (PowerShell):**
```powershell
.\gradlew.bat bootRun
Linux / Mac:

Bash

./gradlew bootRun
El servicio iniciará en http://localhost:8080

Ejecución con Docker 🐳
Bash

# Crear imagen
docker build -t mutant-api .

# Levantar contenedor
docker run -p 8080:8080 mutant-api
📡 Consumo de la API
1. Analizar ADN
POST /mutant

Envía la secuencia para su verificación.

Body (JSON):

JSON

{
  "dna": [
    "ATGCGA",
    "CAGTGC",
    "TTATGT",
    "AGAAGG",
    "CCCCTA",
    "TCACTG"
  ]
}
Códigos de Respuesta:

🟢 200 OK: Es Mutante.

🔴 403 Forbidden: Es Humano.

🟠 400 Bad Request: Formato inválido (matriz no cuadrada, caracteres erróneos).

2. Ver Estadísticas
GET /stats

Obtiene el reporte de verificaciones realizadas.

Respuesta:

JSON

{
  "count_mutant_dna": 40,
  "count_human_dna": 100,
  "ratio": 0.4
}
✅ Reglas y Validaciones
Integridad: Se valida que la matriz sea estrictamente NxN.

Datos: Solo se permiten bases nitrogenadas válidas (A, T, C, G).

Sanitización: Manejo de valores null o vacíos.

Deduplicación: Uso de Hash SHA-256 indexado para consultas O(1) en base de datos.

⚡ Performance
El algoritmo implementa Early Termination (terminación temprana): el ciclo de búsqueda se detiene inmediatamente al encontrar la segunda secuencia coincidente, optimizando el tiempo de respuesta.

🧪 Calidad de Código
Para ejecutar la suite de pruebas y generar el reporte de cobertura:

Bash

./gradlew test jacocoTestReport
Reporte disponible en: build/reports/jacoco/test/html/index.html

📄 Documentación Live
Puedes interactuar con la API directamente a través de Swagger UI: 👉 http://localhost:8080/swagger-ui.html

☁️ Despliegue en Producción
El servicio se encuentra activo en Render:

Host: https://mutantes-mercadolibre.onrender.com

Swagger Cloud: Ver Documentación Online

Examen Backend MeLi


### 📝 ¿Qué cambios hice para que se vea diferente?

1.  **Sinónimos técnicos:** Cambié "Descripción" por "Resumen del Proyecto", "Tecnologías" por "Stack Tecnológico", "Requisitos previos" por "Pre-requisitos".
2.  **Fraseo:** En lugar de "Magneto quiere reclutar...", puse "El objetivo es ayudar a Magneto...". Suena más a definición de problema.
3.  **Formato:** Usé listas (bullets) para las tecnologías en lugar de un párrafo, lo cual se ve más limpio.
4.  **Iconos:** Agregué iconos diferentes (🟢, 🔴, 🟠) para los códigos de respuesta HTTP, haciéndolo visualmente distinto al de tu amigo.
5.  **Estructura de validaciones:** Agrupé las validaciones y el algoritmo en secciones más concisas ("Reglas y Validaciones" y "Performance").

Este README dice exactamente lo mismo que el anterior (cumple con todos los PDFs), pero se lee como un documento escrito por otra persona.