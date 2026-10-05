# ADR 001: Modelo de Despliegue de Aplicaciones Mule

## Estado
**Propuesto**

Esta decisión será revisada y ratificada tras una evaluación detallada de las implicaciones de costos de licenciamiento (vCores) y los requisitos específicos de red del cliente.

---

## Contexto
Nuestra organización requiere definir el modelo de despliegue para las nuevas integraciones y APIs desarrolladas en MuleSoft. Durante el análisis de la arquitectura, se han identificado las siguientes restricciones y necesidades clave del cliente:

* **Falta de Infraestructura Local:** El cliente actualmente no cuenta con los recursos de hardware ni el software necesario para alojar servidores locales (on-premise).
* **Ausencia de Proveedor Cloud:** No existe un contrato vigente o suscripción con proveedores de computación en la nube pública (como Microsoft Azure, AWS o Google Cloud Platform).
* **Eficiencia Operativa:** Se requiere un modelo de despliegue que minimice la carga operativa, ya que no se cuenta con un equipo dedicado para la gestión y mantenimiento de infraestructura subyacente o clústeres de contenedores.
* **Seguridad y Escalabilidad:** La solución debe garantizar el aislamiento de las aplicaciones, proteger los datos sensibles y ser capaz de escalar dinámicamente según el volumen de transacciones.

Dado este escenario, se propone la utilización de CloudHub 2.0 como el modelo de despliegue estándar.

---

## Decisión
Desplegar las aplicaciones Mule utilizando **CloudHub 2.0**, la plataforma de integración como servicio (iPaaS) totalmente gestionada y en contenedores de MuleSoft.

### Justificación

* **Plataforma Totalmente Gestionada (Cero Infraestructura):**
  * Al ser un iPaaS totalmente gestionado por MuleSoft, elimina por completo la necesidad de que el cliente adquiera hardware local o contrate servicios de nube de terceros (como Azure o AWS). Esto resuelve directamente la limitación actual de recursos del cliente.

* **Arquitectura Basada en Contenedores y Aislamiento:**
  * CloudHub 2.0 despliega APIs e integraciones como contenedores ligeros en la nube.
  * Proporciona un límite de aislamiento estandarizado al ejecutar cada instancia de Mule y cada servicio como un contenedor separado, garantizando que los recursos no se compartan de manera insegura entre aplicaciones.

* **Escalabilidad Dinámica:**
  * La plataforma escala dinámicamente la infraestructura y los servicios integrados (hacia arriba o hacia abajo) para soportar volúmenes de transacciones elásticos, adaptándose a los picos de demanda sin intervención manual.

* **Seguridad Integrada y Cumplimiento:**
  * Incorpora políticas de seguridad robustas, protegiendo los servicios y datos sensibles con secretos encriptados, controles de firewall y acceso restringido al shell.
  * Encripta certificados, contraseñas y otros datos de configuración sensibles tanto en reposo como en tránsito dentro de la plataforma Anypoint.

* **Disponibilidad Global:**
  * Soporta despliegues en todas las regiones de MuleSoft, lo que permite ubicar las aplicaciones geográficamente cerca de los consumidores finales o cumplir con futuras normativas de residencia de datos si fuera necesario.

---

## Consecuencias

### Consecuencias Positivas
* **Time-to-Market Acelerado:** El equipo puede comenzar a desplegar aplicaciones inmediatamente sin esperar el aprovisionamiento de infraestructura.
* **Reducción del TCO (Costo Total de Propiedad):** Al no tener que mantener servidores, sistemas operativos ni plataformas de orquestación, se reducen drásticamente los costos operativos y de mantenimiento.
* **Seguridad Enterprise Out-of-the-Box:** Se heredan automáticamente las certificaciones y controles de seguridad gestionados por MuleSoft.

### Posibles Desventajas (Trade-offs)
* **Control Limitado de la Infraestructura:** Al ser una plataforma totalmente gestionada, el equipo técnico no tiene acceso a los niveles inferiores de la infraestructura (por ejemplo, el sistema operativo o el clúster de Kubernetes subyacente).
* **Dependencia del Ecosistema MuleSoft:** El modelo de despliegue y su topología de red (Ingress, VPCs compartidas/privadas) están sujetos a las capacidades y evolución del roadmap específico de CloudHub 2.0.

---

## Referencias
* [CloudHub 2.0 Overview - MuleSoft Documentation](https://docs.mulesoft.com/cloudhub-2/)