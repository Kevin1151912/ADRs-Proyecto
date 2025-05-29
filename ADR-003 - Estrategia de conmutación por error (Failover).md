# ADR-003 - Estrategia de conmutación por error (Failover)
**Fecha:** 2025-05-24  
**Estado:** Propuesta

## Contexto

La arquitectura propuesta busca minimizar la interrupción del servicio en caso de fallos en la infraestructura de uno de los proveedores de nube pública. Esto puede ocurrir por razones técnicas (fallas en servidores, interrupciones regionales) o estratégicas (quiebra del proveedor, cambios legales).

Por esta razón, se necesita una **estrategia de conmutación por error (failover)** que permita que los servicios se muevan automáticamente o bajo intervención manual a otro entorno operativo funcional.

## Decisión

Se implementará un mecanismo de **monitoreo proactivo** y failover controlado con las siguientes características:

- **Detección automática de fallos** mediante monitoreo de disponibilidad, latencia, errores y salud del sistema.
- **Orquestación del failover** utilizando balanceadores de carga, DNS dinámico o herramientas como Kubernetes Federation o Global Load Balancers.
- **Procedimientos de failover manual**, en casos de migraciones planificadas o por decisión ejecutiva.
- **Automatización de despliegue continuo** en múltiples nubes para garantizar que el entorno de respaldo esté actualizado y listo para activarse.

## Justificación

Una estrategia de failover permite:
- Mantener la disponibilidad del servicio con **mínimo tiempo de inactividad**.
- Mitigar riesgos operativos y financieros derivados de fallos graves.
- Mejorar la experiencia del usuario al evitar interrupciones inesperadas.

## Consecuencias

### Positivas
- Alta disponibilidad del sistema incluso ante eventos críticos.
- Capacidad de reacción rápida y eficiente frente a emergencias.
- Fortalecimiento de la confianza de los usuarios y clientes.

### Negativas
- El diseño del mecanismo automático requiere pruebas exhaustivas y simulaciones de escenarios de fallo.
- Puede ser necesario replicar infraestructura en múltiples nubes de forma constante, lo que incrementa los costos.

## Alternativas consideradas

- **Failover manual sin monitoreo automatizado:** Es más económico, pero aumenta el riesgo de fallos prolongados.
- **Balanceo multinube activo-activo:** Más complejo de mantener y más costoso, no viable en esta etapa.

## Relación con otras decisiones

Esta estrategia solo es viable si se ha adoptado:
- [ADR-001 - Elección de arquitectura multi-nube](./ADR-001%20-%20Elección%20de%20arquitectura%20multi-nube.md)

Y requiere:
- [ADR-002 - Sincronización de datos entre nubes](./ADR-002%20-%20Sincronización%20de%20datos%20entre%20nubes.md)
