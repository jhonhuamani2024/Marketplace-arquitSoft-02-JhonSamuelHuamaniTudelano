# Restricciones

## Marketplace de productos para mascotas

Las restricciones son condiciones, reglas o limitaciones que deben respetarse durante el desarrollo del sistema.

| ID   | Restricción          | Descripción                                                                                                                      |
| ---- | -------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| RC01 | Aplicación web       | El sistema debe desarrollarse como una aplicación accesible mediante un navegador web.                                           |
| RC02 | Control de versiones | El código fuente debe gestionarse utilizando Git y mantenerse en un repositorio compartido.                                      |
| RC03 | API REST             | La comunicación entre el frontend y los servicios del sistema debe realizarse mediante una API REST.                             |
| RC04 | Pasarela de pago     | El sistema debe integrarse con una pasarela de pago externa para procesar las operaciones de pago.                               |
| RC05 | Servicio de envío    | El sistema debe integrarse con un servicio externo de envío para gestionar la información relacionada con la entrega de pedidos. |

## Consideraciones

Las restricciones anteriores condicionan las decisiones de diseño y arquitectura del marketplace.

En particular:

* El sistema será accesible mediante una aplicación web.
* El código será gestionado mediante Git.
* La comunicación entre frontend y backend utilizará una API REST.
* Los pagos dependerán de una pasarela externa.
* Los envíos dependerán de un servicio externo.
