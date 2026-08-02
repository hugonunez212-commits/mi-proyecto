---
name: n8n-workflow-patterns
description: Patrones de arquitectura de flujos de trabajo comprobados de n8n reales. Úsalo al crear nuevos workflows, diseñar estructura de workflows, planificar arquitecturas, o al preguntar sobre webhooks, integración de API HTTP, operaciones de base de datos, agentes de IA, procesamiento por lotes o tareas programadas.
---

# Patrones de Flujos de Trabajo en n8n

Patrones arquitectónicos comprobados para la construcción de flujos en n8n.

---

## Los 6 Patrones Principales

Basado en el análisis del uso real de flujos:

1. **[Procesamiento de Webhooks](webhook_processing.md)** (Más común)
   - Recibe solicitudes HTTP → Procesa → Emite salida
   - Patrón: Webhook → Validar → Transformar → Responder/Notificar

2. **[Integración con API HTTP](http_api_integration.md)**
   - Recupera datos de APIs REST → Transforma → Almacena/Usa
   - Patrón: Disparador → Petición HTTP → Transformar → Acción → Manejador de Errores

3. **[Operaciones de Base de Datos](database_operations.md)**
   - Lectura/Escritura/Sincronización de datos
   - Patrón: Programador → Consultar → Transformar → Escribir → Verificar

4. **[Flujo de Agente de IA](ai_agent_workflow.md)**
   - Agentes de IA con memoria y herramientas
   - Patrón: Disparador → Agente de IA (Modelo + Herramientas + Memoria) → Salida

5. **[Tareas Programadas](scheduled_tasks.md)**
   - Flujos de trabajo de automatización recurrentes
   - Patrón: Programador → Recuperar → Procesar → Entregar → Registrar (Log)

6. **Procesamiento por Lotes (Batch)** (ver abajo)
   - Procesa conjuntos de datos masivos en partes considerando los límites de APIs
   - Patrón: Preparar → Dividir en lotes → Procesar por lote → Acumular → Agregar

---

## Guía de Selección de Patrones

### Cuándo usar cada patrón:

**Procesamiento de Webhooks** - Úsalo cuando:
- Recibes datos de sistemas externos.
- Construyes integraciones (comandos de Slack, envíos de formularios, webhooks de GitHub).
- Necesitas respuestas instantáneas a eventos.
- Ejemplo: "Recibir pago de Stripe vía webhook → Actualizar BD → Enviar confirmación".

**Integración de API HTTP** - Úsalo cuando:
- Recuperas datos de APIs externas.
- Sincronizas servicios de terceros.
- Construyes pipelines de datos.
- Ejemplo: "Traer issues de GitHub → Transformar → Crear tickets en Jira".

**Operaciones de BD** - Úsalo cuando:
- Sincronizas entre bases de datos.
- Ejecutas consultas de bases de datos programadas.
- Haces flujos tipo ETL.
- Ejemplo: "Leer datos de Postgres → Transformar → Escribir en MySQL".

**Agentes de IA** - Úsalo cuando:
- Construyes IA conversacional.
- Necesitas una IA que acceda a herramientas externas.
- Haces tareas de razonamiento de múltiples pasos.

**Tareas Programadas** - Úsalo cuando:
- Generas reportes recurrentes o resúmenes.
- Recuperación periódica de datos.
- Tareas de mantenimiento.

**Procesamiento por Lotes (Batch)** - Úsalo cuando:
- Procesas volúmenes muy grandes de datos que superan límites de API.
- Necesitas procesar bucles anidados.
- Ejemplo: "Bajar productos de 4 mercados × 1000 por cada consulta → Agregar los resultados".

---

## Componentes Comunes de Flujo

### 1. Disparadores (Triggers)
- **Webhook** - URL / Punto final HTTP (instantáneo).
- **Schedule** - Basado en Cron (periódico).
- **Manual** - Clic para ejecutar (pruebas).

### 2. Fuentes de Datos
- **Petición HTTP** - APIs REST.
- **Bases de datos** - Postgres, MySQL, MongoDB.
- **Nodos de Servicios** - Slack, Google Sheets, etc.

### 3. Transformación
- **Set (Establecer)** - Mapear o transformar valores.
- **Code (Código)** - Lógica compleja.
- **IF / Switch** - Enrutamiento condicional.

### 4. Salidas
- **Petición HTTP** - Llamar APIs de salida.
- **Base de datos** - Almacenamiento.

### 5. Manejo de Errores
- **Error Trigger** - Captura la falla en otro flujo.
- **Stop and Error** - Forzar detención o falla.
- **Continue On Fail** - Continuar si hay un fallo local.

---

## Creación de un flujo paso a paso

1. **Planificar**: Identificar patrón, listar nodos necesarios.
2. **Implementar**: Configurar credenciales y conectar el árbol principal.
3. **Validar**: Probar los nodos y la lógica aisladamente (con datos estáticos / de muestra).
4. **Desplegar**: Activar y probar integralmente. Manejar errores.

## Consejos con Integraciones

### Google Sheets
- **Nunca uses opciones de 'anexar'** directamente si afecta fórmulas (usa la API V4 de Google pura si la hoja es compleja con un HTTP request).
- Muestra los números y variables correctas flotantes (Float) para evitar romper sumas de google sheets.

### Bucle 'Split in Batches'
- La salida `0` ('done') ocurre una vez cuando todo termina.
- La salida `1` ocurre iteración por iteración. Combina los resultados mediante la función `All()` del nodo interno antes de continuar el camino principal.

Para ver todos los patrones en profundidad, explora los documentos secundarios del repositorio.
