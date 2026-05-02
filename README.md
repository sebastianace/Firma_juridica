# Sistema de Gestión de Asuntos Legales — Firma Jurídica
Base de datos relacional para gestionar clientes, asuntos legales y procuradores de una firma jurídica.
## Interpretación del problema
Una firma jurídica necesita un sistema para gestionar los asuntos legales de sus clientes. El sistema debe registrar la información de los clientes, los asuntos legales que manejan y los procuradores encargados de llevarlos.
## Entidades y atributos identificados

**Cliente**

- DNI *(clave primaria)*
- nombre
- dirección
- fecha de nacimiento

**Asuntos Legales**

- numero_expediente *(clave primaria)*
- fecha de inicio
- fecha de finalización
- estado del caso

**Procuradores**

- DNI *(clave primaria)*
- nombre
- apellidos
- número de colegiado
- número de casos ganados
## Relaciones y cardinalidades

**Cliente — Asuntos Legales (1:N)**  
Un cliente puede tener varios asuntos legales, pero cada asunto pertenece a un único cliente. Por eso la relación es uno a muchos.

**Procuradores — Asuntos Legales (N:M)**  
Un procurador puede encargarse de varios asuntos y un mismo asunto puede ser llevado por varios procuradores
## Modelo conceptual (Diagrama ER)
<img width="1251" height="739" alt="Gemini_Generated_Image_wcmzb8wcmzb8wcmz (1)" src="https://github.com/user-attachments/assets/550073ac-c8f1-43b1-b6d4-0401b661d26d" />

## Transformación al modelo relacional

1. **Cada entidad → una tabla.**  
   `CLIENTE`, `ASUNTO` y `PROCURADOR` se convierten directamente en tablas, usando sus identificadores únicos como claves primarias.

2. **Relación 1:N (CLIENTE–ASUNTO) → clave foránea.**  
   La PK de `CLIENTE` (`DNI`) se añade como atributo foráneo en `ASUNTO` (`DNI_cliente`), indicando a qué cliente pertenece cada asunto.

3. **Relación N:M (ASUNTO–PROCURADOR) → tabla intermedia.**  
   Se crea la tabla `ASUNTO_PROCURADOR` con clave primaria compuesta por `num_expediente` (FK → `ASUNTO`) y `DNI_procurador` (FK → `PROCURADOR`). Esto evita duplicados y permite en el futuro añadir atributos propios de la relación, como una fecha de asignación.
## Modelo lógico relacional
<img width="910" height="698" alt="TABLA" src="https://github.com/user-attachments/assets/9508eb53-0302-407c-9846-3e64c90174db" />

## Decisiones tomadas

**¿Por qué una tabla intermedia?**  
La relación entre procuradores y asuntos legales es N:M. En bases de datos relacionales este tipo de relación no se puede representar directamente, ya que causaría duplicación de datos. La solución es crear la tabla `asuntos_procuradores` que actúa como puente entre ambas tablas, registrando cada combinación procurador-asunto sin repetir información.

**¿Por qué `numero_expediente` como identificador de asuntos legales?**  
El enunciado indica que cada asunto se identifica por un número de expediente único y El enunciado lo indica explícitamente; es un identificador natural único.
