# RPG Hibernate API

Proyecto Java con Spring MVC + Hibernate para gestionar jugadores de un RPG.
Expone una API REST y sirve una vista web simple con Thymeleaf.

## Tecnologias

- Java 8
- Maven
- Spring Web MVC 5
- Hibernate
- MySQL 8
- P6Spy (log de consultas SQL)
- Thymeleaf

## Estructura principal

- `src/main/java/com/game/controller`: endpoints REST
- `src/main/java/com/game/service`: logica de negocio
- `src/main/java/com/game/repository`: acceso a datos (Hibernate)
- `src/main/java/com/game/entity`: entidades y enums
- `src/main/resources/init.sql`: datos de ejemplo

## Requisitos previos

1. Tener instalado Java 8 y Maven.
2. Tener un servidor MySQL ejecutandose en `localhost:3306`.
3. Crear la base de datos:

```sql
CREATE DATABASE rpg;
```

4. Ajustar credenciales de BD en:
   - `src/main/java/com/game/repository/PlayerRepositoryDB.java`
   - Por defecto:
     - usuario: `root`
     - password: `Testing`

## Carga de datos inicial

Este proyecto incluye datos de ejemplo en `src/main/resources/init.sql`.
Puedes ejecutarlo manualmente en MySQL para poblar la tabla `player`.

> Nota: Hibernate esta configurado con `hbm2ddl.auto=update`, por lo que la tabla se sincroniza automaticamente segun la entidad `Player`.

## Compilar

```bash
mvn clean package
```

Esto genera el WAR en `target/rpg-hibernate-1.0.war`.

## Ejecutar

Al ser un proyecto `war`, debes desplegarlo en un contenedor Servlet (por ejemplo, Tomcat).
Tambien puedes usar la configuracion de Smart Tomcat desde IntelliJ.

## Endpoints REST

Base URL:

`/rest/players`

### Obtener jugadores (paginado)

- `GET /rest/players?pageNumber=0&pageSize=3`
- Parametros opcionales:
  - `pageNumber` (default: `0`)
  - `pageSize` (default: `3`)

### Contar jugadores

- `GET /rest/players/count`

### Crear jugador

- `POST /rest/players`
- Body JSON (ejemplo):

```json
{
  "name": "Arin",
  "title": "Shadow Blade",
  "race": "HUMAN",
  "profession": "WARRIOR",
  "birthday": 1276037080000,
  "banned": false,
  "level": 10
}
```

### Actualizar jugador

- `POST /rest/players/{ID}`
- Solo actualiza campos enviados en el body.

### Eliminar jugador

- `DELETE /rest/players/{ID}`

## Validaciones implementadas

- `id` debe ser mayor a 0 para actualizar/eliminar.
- `name`: obligatorio en creacion, no vacio, maximo 12 caracteres.
- `title`: obligatorio en creacion, maximo 30 caracteres.
- `race` y `profession`: obligatorios en creacion.
- `birthday`: obligatoria en creacion, no negativa, anio entre 2000 y 3000.
- `banned`: en creacion se considera `false` si no se envia.

## Valores permitidos de enums

- `race`: `HUMAN`, `DWARF`, `ELF`, `GIANT`, `ORC`, `TROLL`, `HOBBIT`
- `profession`: `WARRIOR`, `ROGUE`, `SORCERER`, `CLERIC`, `PALADIN`, `NAZGUL`, `WARLOCK`, `DRUID`

## Vista web

La ruta `/` renderiza la plantilla `my.html` ubicada en `src/main/webapp/html`.

