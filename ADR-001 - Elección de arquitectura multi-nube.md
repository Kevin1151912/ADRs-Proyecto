# ADR-001 - Elección de arquitectura multi-nube
**Fecha:** 2025-05-29  
**Estado:** Propuesta

## Contexto

Actualmente, la aplicación está alojada en un único proveedor de nube pública. Esta decisión inicial, aunque funcional, representa una limitación considerable frente a los objetivos de continuidad del negocio y tolerancia a fallos.

El uso de un solo proveedor introduce los siguientes riesgos:
- Pérdida total o parcial del servicio ante fallos en el proveedor.
- Riesgo de **vendor lock-in**, lo que complica migraciones o cambios futuros.
- Dificultad para responder a cambios regulatorios o geopolíticos (por ejemplo, adquisición del proveedor por parte de un país con restricciones).

La empresa ha expresado su rechazo a adoptar nubes privadas, comunitarias o híbridas. En consecuencia, se requiere una solución basada exclusivamente en **múltiples nubes públicas**, que funcione de forma independiente del proveedor.

## Decisión

Se adoptará una arquitectura **cloud-agnostic** (agnóstica de la nube), la cual permite desplegar y ejecutar los servicios de la aplicación en diferentes proveedores de nube pública sin necesidad de realizar cambios en el código o en las configuraciones internas.

Las decisiones clave incluyen:
- Evitar el uso de servicios propietarios o específicos de un proveedor.
- Utilizar herramientas y estándares abiertos, como contenedores Docker, Kubernetes y bases de datos compatibles.
- Mantener la misma experiencia de usuario y comportamiento del sistema, sin importar la nube subyacente.

## Justificación

Esta elección garantiza:
- Mayor **resiliencia** y disponibilidad ante fallos catastróficos en una nube.
- **Flexibilidad** para migrar cargas de trabajo sin rediseñar la arquitectura.
- Reducción de la dependencia de cualquier proveedor (mitigando el vendor lock-in).
- Cumplimiento con requisitos de gobernanza y estrategia empresarial.

## Consecuencias

### Positivas
- Se incrementa la tolerancia a fallos a nivel de infraestructura.
- La arquitectura puede escalar a diferentes nubes según demanda o costo.
- Permite cumplir con normativas que requieran independencia de proveedor.

### Negativas
- Aumenta la complejidad en el diseño e implementación del sistema.
- Algunas funcionalidades avanzadas (como servicios de IA gestionados o funciones sin servidor propietarias) podrían no utilizarse para preservar la portabilidad.

## Alternativas consideradas

- **Usar múltiples regiones del mismo proveedor:** No resuelve el riesgo del vendor lock-in.
- **Implementar una nube híbrida:** Rechazada explícitamente por la empresa.

## Relación con otras decisiones

Esta decisión es la base fundamental para los siguientes ADRs:
- [ADR-002 - Sincronización de datos entre nubes](./ADR-002%20-%20Sincronización%20de%20datos%20entre%20nubes.md): Mantiene los datos consistentes entre nubes.
- [ADR-003 - Estrategia de conmutación por error (Failover)](./ADR-003%20-%20Estrategia%20de%20conmutación%20por%20error%20(Failover).md): Permite cambiar automáticamente entre proveedores ante fallos.
