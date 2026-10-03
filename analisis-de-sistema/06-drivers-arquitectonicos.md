# 4. Drivers arquitectónicos

Un driver arquitectónico es un requisito, atributo de calidad o restricción que
influye de forma significativa en la estructura del sistema. Los drivers de este
proyecto salen de los atributos de calidad (AC) y de las restricciones (R) del
marketplace de productos para mascotas.

## 4.1 Drivers priorizados

| Prioridad | ID | Driver | Origen | Problema que plantea | Decisión que responde |
|---|---|---|---|---|---|
| 1 | DA06 | Mantenibilidad / evolución modular | AC05 | Los cambios en una funcionalidad no deben afectar innecesariamente a otros módulos | Modularidad + Clean Architecture |
| 2 | DA03 | Seguridad | AC01 | El sistema maneja datos personales y de pago | Autenticación y autorización |
| 3 | DA02 | Rendimiento | AC02 | Habrá alta concurrencia en búsquedas y catálogo | Caché y optimización de comunicación/procesamiento |
| 4 | DA01 | Escalabilidad | AC04 | Aumentarán los usuarios durante campañas | Monolito modular con posibilidad de escalamiento horizontal |
| 5 | DA04 | Pago externo | R06 | Hay que comunicarse con una pasarela de pagos de terceros | Integración mediante API y adaptadores |
| 6 | DA05 | API REST | R03 | Frontend y backend deben comunicarse mediante REST | Separar interfaz y backend mediante API REST |

## 4.2 Descripción de cada driver

### DA06 — Mantenibilidad / evolución modular
- **Origen:** AC05 (Mantenibilidad), RNF05.
- **Descripción:** El sistema debe permitir modificar funcionalidades sin afectar
  innecesariamente otros módulos.
- **Escenario:** Cuando se cambia la pasarela de pagos, solo se modifica el módulo
  de Pagos; Catálogo y Carrito no se tocan.
- **Por qué influye:** Define la separación de responsabilidades, la modularidad y
  las dependencias internas.

### DA03 — Seguridad
- **Origen:** AC01 (Seguridad), RNF02.
- **Descripción:** Solo usuarios autenticados pueden comprar, y cada rol (cliente,
  seller, administrador) accede solo a lo que le corresponde.
- **Escenario:** Cuando un usuario sin sesión intenta registrar un pedido, el
  sistema rechaza la solicitud.
- **Por qué influye:** Obliga a incluir autenticación, autorización por roles y
  comunicación cifrada (HTTPS) desde el diseño.

### DA02 — Rendimiento
- **Origen:** AC02 (Rendimiento), RNF01.
- **Descripción:** Las búsquedas y el catálogo deben responder rápido aun con muchos
  usuarios simultáneos.
- **Escenario:** Con 500 usuarios concurrentes, el catálogo responde en menos de 2
  segundos.
- **Por qué influye:** Obliga a incorporar caché para la información de consulta
  frecuente y a optimizar las consultas.

### DA01 — Escalabilidad
- **Origen:** AC04 (Escalabilidad), RNF04.
- **Descripción:** El sistema debe soportar el aumento de usuarios en campañas.
- **Escenario:** Cuando aumenta la demanda, se pueden replicar instancias de la
  aplicación sin rediseñarla.
- **Por qué influye:** Condiciona que el backend no guarde estado en el servidor
  y que se pueda desplegar en varias instancias.

### DA04 — Pago externo
- **Origen:** R06 (restricción de integración).
- **Descripción:** El pago se procesa con una pasarela externa que el equipo no
  controla y que podría cambiar.
- **Escenario:** Al cambiar de proveedor de pagos, los casos de uso no se modifican;
  solo se reemplaza el adaptador.
- **Por qué influye:** Obliga a definir una interfaz de pagos (puerto) y un
  adaptador por proveedor.

### DA05 — API REST
- **Origen:** R03 (restricción técnica).
- **Descripción:** El frontend y el backend se comunican mediante una API REST con
  intercambio en JSON.
- **Escenario:** El frontend consume los endpoints `/api/v1/*` sin conocer los
  detalles internos del backend.
- **Por qué influye:** Define el límite entre interfaz y backend, y permite evolucionar
  ambos por separado.

## 4.3 Justificación del orden de prioridad

Mantenibilidad va primero porque es el driver que más condiciona la estructura
interna del sistema (módulos, capas y dependencias). Seguridad va segunda porque el
marketplace maneja datos personales y de pago, y una falla tiene un costo alto.
Rendimiento y escalabilidad siguen porque las campañas generan picos de carga.
Pago externo y API REST son restricciones ya impuestas, que se resuelven con
patrones conocidos (adaptadores y separación frontend/backend).

## 4.4 Trazabilidad: del requisito a la decisión

| Requisito | Atributo | Driver | Decisión (ADR) |
|---|---|---|---|
| RNF05 | AC05 Mantenibilidad | DA06 | ADR-001 Monolito modular, ADR-002 Clean Architecture |
| RF01, RNF02 | AC01 Seguridad | DA03 | ADR-006 Autenticación con JWT y roles |
| RNF01 | AC02 Rendimiento | DA02 | ADR-003 Estrategia de caché |
| RNF03, RNF04 | AC03, AC04 Escalabilidad | DA01 | ADR-001 Monolito modular |
| RF05, R06 | — | DA04 | ADR-004 Integración de pagos con interfaces y adaptadores |
| R03 | — | DA05 | ADR-005 API REST entre frontend y backend |

## 4.5 Verificación de cobertura

- [x] Cada driver tiene un origen (AC o R) definido.
- [x] Cada driver tiene al menos una decisión (ADR) que lo responde.
- [x] Cada driver tiene un escenario concreto.