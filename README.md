# Component-2: Spring Boot + GraphQL Microservice

Este microservicio implementa la gestión de **Profesores**, **Asignaturas** y **Calificaciones** mediante un API GraphQL y MongoDB.

## 📋 Funcionalidades

### Profesor

* **Query**:

  * `profesores`: Lista todos los profesores.
  * `profesorPorId(id: ID!)`: Obtiene un profesor por su ID.

  **Ejemplos**:

  ```graphql
  query {
    profesores {
      id
      nombre
      documento
      area
    }
  }

  query {
    profesorPorId(id: "<ID_PROF>") {
      id
      nombre
      documento
      area
    }
  }
  ```

* **Mutation**:

  * `crearProfesor(nombre: String!, documento: String!, area: String!): Profesor!`
  * `actualizarProfesor(id: ID!, nombre: String, area: String): Profesor!`
  * `eliminarProfesor(id: ID!): Boolean!`

  **Ejemplos**:

  ```graphql
  mutation {
    crearProfesor(
      nombre: "Juan Pérez",
      documento: "CC999999",
      area: "Física"
    ) {
      id nombre documento area
    }
  }

  mutation {
    actualizarProfesor(
      id: "<ID_PROF>",
      area: "Química"
    ) {
      id nombre area
    }
  }

  mutation {
    eliminarProfesor(id: "<ID_PROF>")
  }
  ```

### Asignatura

* **Query**:

  * `asignaturas`: Lista todas las asignaturas.

  **Ejemplo**:

  ```graphql
  query {
    asignaturas {
      id
      nombre
      profesorIds
    }
  }
  ```

* **Mutation**:

  * `crearAsignatura(nombre: String!): Asignatura!`
  * `actualizarAsignatura(id: ID!, nombre: String): Asignatura!`
  * `eliminarAsignatura(id: ID!): Boolean!`
  * `asignarProfesorAAsignatura(profesorId: ID!, asignaturaId: ID!): Asignatura!`
  * `desasignarProfesorDeAsignatura(profesorId: ID!, asignaturaId: ID!): Asignatura!`

  **Ejemplos**:

  ```graphql
  mutation {
    crearAsignatura(nombre: "Historia") {
      id nombre profesorIds
    }
  }

  mutation {
    actualizarAsignatura(
      id: "<ID_ASIG>",
      nombre: "Biología"
    ) {
      id nombre
    }
  }

  mutation {
    eliminarAsignatura(id: "<ID_ASIG>")
  }

  mutation {
    asignarProfesorAAsignatura(
      profesorId: "<ID_PROF>",
      asignaturaId: "<ID_ASIG>"
    ) {
      id profesorIds
    }
  }

  mutation {
    desasignarProfesorDeAsignatura(
      profesorId: "<ID_PROF>",
      asignaturaId: "<ID_ASIG>"
    ) {
      id profesorIds
    }
  }
  ```

### Calificación

* **Query**:

  * `calificaciones(estudianteId: ID, asignaturaId: ID, cursoId: ID, periodo: String): [Calificacion!]!`

  **Ejemplo**:

  ```graphql
  query {
    calificaciones(periodo: "2025-1") {
      id
      estudianteId
      asignaturaId
      cursoId
      periodo
      nota
      observaciones
    }
  }
  ```

* **Mutation**:

  * `registrarCalificacion(estudianteId: ID!, asignaturaId: ID!, cursoId: ID!, periodo: String!, nota: Float!, observaciones: String): Calificacion!`
  * `actualizarCalificacion(id: ID!, nota: Float, observaciones: String): Calificacion!`
  * `eliminarCalificacion(id: ID!): Boolean!`

  **Ejemplos**:

  ```graphql
  mutation {
    registrarCalificacion(
      estudianteId: "est-101",
      asignaturaId: "<ID_ASIG>",
      cursoId: "11-A",
      periodo: "2025-1",
      nota: 4.2,
      observaciones: "Buen desempeño"
    ) {
      id nota observaciones
    }
  }

  mutation {
    actualizarCalificacion(
      id: "<ID_CALIF>",
      nota: 4.8
    ) {
      id nota observaciones
    }
  }

  mutation {
    eliminarCalificacion(id: "<ID_CALIF>")
  }
  ```

## 🚀 Ejecución con Docker Compose con Docker Compose

1. **Clona el repositorio** y navega al directorio:

   ```bash
   git clone <URL_DEL_REPO>
   cd component-2
   ```

2. **Asegúrate** de no tener MongoDB local escuchando en el puerto 27017, o ajusta el puerto en `docker-compose.yml`.

3. **Levanta los servicios**:

   ```bash
   docker compose up --build
   ```

   * El servicio **mongoDB** correrá en el contenedor `mongoDB` y se mapea al puerto 27018 (ajustable).
   * El servicio **api** correrá en `http://localhost:8080`.

4. **Probar GraphQL**:

   * Abre GraphiQL en: `http://localhost:8080/graphiql`
   * Ejecuta consultas y mutaciones según las funcionalidades descritas.
