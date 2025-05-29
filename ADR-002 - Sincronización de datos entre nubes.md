# ADR-002 - Sincronización de datos entre nubes
**Fecha:** 2025-05-29  
**Estado:** Propuesta

## Contexto

Una de las principales exigencias del sistema multinube es que los datos estén **disponibles y sincronizados** entre los diferentes proveedores de nube pública. Esto incluye:

- Bases de datos relacionales (RDS u otras)
- Bases de datos NoSQL (por ejemplo, DynamoDB, MongoDB)
- Almacenamiento en bloque (para archivos, multimedia, documentos)

La sincronización de datos es esencial no solo para mantener la integridad de la información, sino también para garantizar que la aplicación pueda reanudarse rápidamente en otra nube en caso de fallo sin pérdida de información.

## Decisión

Se implementará un esquema de **sincronización bidireccional y segura** entre las nubes públicas. Este mecanismo será **independiente del proveedor**, asegurando portabilidad, flexibilidad y compatibilidad.

Las herramientas evaluadas y seleccionadas incluyen:
- **Apache Kafka MirrorMaker**: para replicación de eventos y logs entre clústeres distribuidos.
- **Debezium**: para capturar cambios en bases de datos relacionales mediante CDC (Change Data Capture).
- **Rclone** o similares: para replicación eficiente de almacenamiento en bloque (archivos y objetos).

Además, se utilizarán canales de comunicación cifrados (TLS/HTTPS, VPN o canales dedicados) para proteger los datos en tránsito.

## Justificación

La sincronización de datos garantiza:
- Disponibilidad continua de la aplicación en múltiples entornos.
- Reducción del tiempo de recuperación en caso de desastre.
- Cumplimiento con estándares de seguridad y continuidad del negocio.

## Consecuencias

### Positivas
- Los datos estarán disponibles en todas las nubes de forma confiable y sin pérdida.
- Se mantiene la integridad y consistencia entre bases de datos distribuidas.
- Aumenta la capacidad de recuperación ante desastres (*Disaster Recovery*).

### Negativas
- Implica un **costo operativo adicional** debido al uso de herramientas de replicación y consumo de ancho de banda.
- Requiere **monitoreo y auditoría** constantes para validar el estado de sincronización.
- Puede introducir **latencia** si no se configura correctamente.

## Alternativas consideradas

- **Sin sincronización previa (replicación post-fallo):** Requiere mucho tiempo de recuperación y genera pérdida de datos.
- **Uso de servicios propietarios (como AWS DMS):** No compatible con la estrategia cloud-agnostic.

## Relación con otras decisiones

Esta sincronización es imprescindible para que funcione:
- [ADR-003 - Estrategia de conmutación por error (Failover)](./ADR-003%20-%20Estrategia%20de%20conmutación%20por%20error%20(Failover).md)

Y depende del enfoque definido en:
- [ADR-001 - Elección de arquitectura multi-nube](./ADR-001%20-%20Elección%20de%20arquitectura%20multi-nube.md)
