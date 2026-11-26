<div align="center">
  <img width="250" height="250" alt="logoRedondoSF" src="https://github.com/user-attachments/assets/136ba8fd-979e-472a-b830-1d36f3df09f3" />
 
  <p><strong>Juega – Aprende – Supérate</strong></p>
</div>

---

Aplicación mobile nativa para **Android** desarrollada en **Kotlin** con **Jetpack Compose** y **Material Design 3**, diseñada para facilitar la iniciación y consolidación de los fundamentos de Kotlin mediante **gamificación**, **microaprendizaje** y retroalimentación educativa inmediata.


## Planteamiento y objetivos

Aprender a programar desde cero suele generar frustración debido a entornos complejos, documentación densa o tutoriales pasivos. **K-Reto** aborda las tres principales barreras iniciales:

* **Sobrecarga cognitiva:** Introduce los conceptos de forma gradual, sin saturar al estudiante con herramientas complejas desde el primer minuto.
* **Falta de hábito y constancia:** Propone sesiones cortas y dinámicas con mecánicas de progreso, desbloqueo de niveles y ranking para mantener el ritmo diario.
* **Dificultad para entender los errores:** Ofrece explicaciones claras y breves ante cada fallo para transformar el error en aprendizaje activo.

### Objetivos específicos
* Diseñar una curva de aprendizaje progresiva desde tipos básicos hasta programación orientada a objetos elemental y conceptos avanzados de Kotlin.
* Implementar mecánicas de gamificación (niveles, puntuación, progreso en vivo y ranking) para fomentar la constancia.
* Construir una interfaz clara, accesible y legible para código en pantallas móviles con **Jetpack Compose** y **Material 3**.
* Garantizar retroalimentación inmediata tras cada respuesta.

---

## Mecánica del juego y niveles

### Estructura en 3 niveles progresivos
* **Nivel 1 — Básico:** Sintaxis esencial, `val` y `var`, tipos básicos, condicionales (`if` como expresión, `when`), bucles, rangos y funciones sencillas.
* **Nivel 2 — Intermedio:** *Null-safety*, colecciones, clases especiales (`data class`, `sealed class`, `object`) y Programación Orientada a Objetos (POO).
* **Nivel 3 — Avanzado:** Funciones de orden superior, *lambdas*, funciones de alcance (`let`, `apply`, `also`, `run`, `with`), propiedades delegadas (`by lazy`) y corrutinas (`launch`, `async`, `await`).

Los niveles superiores pueden permanecer bloqueados hasta alcanzar la puntuación mínima requerida en el nivel anterior.

### Tipos de retos (3 opciones de respuesta)
Cada ronda combina distintos formatos de tarjeta para mantener el dinamismo:
1. **Pregunta conceptual:** Cuestiones teóricas sobre reglas o comportamientos de Kotlin.
2. **Lectura o completar código:** Fragmentos en formato monoespaciado para predecir salidas, detectar errores o completar expresiones.
3. **Reto rápido / tarjeta exprés:** Preguntas ágiles de concepto o sintaxis.

### Flujo de partida y puntuación
1. **Jugador y nivel:** Selecciona o registra el perfil y elige un nivel desbloqueado desde el menú principal (`Home`).
2. **Resolución:** Lee la tarjeta y pulsa una de las **3 respuestas**; el resto de botones se bloquean al instante para evitar dobles pulsaciones.
3. **Acierto (`+10 puntos`):** Vista de confirmación en tono verde, suma de **10 puntos** y avance al siguiente reto.
4. **Fallo (`-5 puntos` + explicación):** Vista en tono rojo, resta de **5 puntos** y explicación breve de la respuesta correcta antes de continuar.
5. **Progreso en vivo:** Cabecera con marcador acumulado y progreso de tarjetas actualizado en tiempo real.
6. **Cierre y Ranking Top 10:** Resumen final de aciertos, fallos y puntuación total, registro de resultados y tabla **Top 10** ordenada de mayor a menor con el **Top 3** destacado.

---

## Stack tecnológico

| Capa | Tecnología | Función en el proyecto |
| :--- | :--- | :--- |
| **Lenguaje** | **Kotlin** | Lenguaje principal, seguro frente a nulos y estándar oficial en Android. |
| **UI** | **Jetpack Compose + Material 3** | Pantallas declarativas y reactivas, tarjetas de código, botones y barras de progreso. |
| **Arquitectura** | **MVVM** | Separación entre vista, lógica de negocio y datos con `StateFlow` y **Corrutinas**. |
| **Autenticación** | **Firebase Authentication** | Registro, inicio de sesión, gestión segura de credenciales y control de nombres duplicados. |
| **Base de datos** | **Cloud Firestore (NoSQL)** | Perfiles, puntuación (`XP`), progreso de niveles y ranking en tiempo real. |
| **Contenido local** | **JSON en `assets`** | Banco de preguntas y explicaciones de lectura rápida sin consumo adicional de datos. |
| **IDE y build** | **Android Studio (Gradle)** | Desarrollo, compilación y pruebas en emulador o dispositivo físico. |


---

## Arquitectura MVVM, persistencia y estructura de carpetas

### Diagrama de arquitectura (MVVM + Firebase + JSON local)

```text
+-----------------------------------------------------------------------+
|                           UI (Jetpack Compose)                        |
|  Pantallas: Home, Niveles, Juego (Retos 3 opciones), Acierto/Fallo,   |
|             Resumen de Ronda, Ranking Top 10 (Material Design 3)      |
+-----------------------------------^-----------------------------------+
                                    | Observa StateFlow / Eventos UI
                                    v
+-----------------------------------------------------------------------+
|                       ViewModel (Estado y Lógica)                     |
|  - Gestión de ronda, temporizador/progreso y evaluación de respuestas |
|  - Cálculo de puntuación (+10 / -5) y desbloqueo de niveles           |
|  - Corrutinas para llamadas asíncronas y actualización reactiva       |
+-----------------------------------^-----------------------------------+
                                    | Llama a casos de uso / repositorios
                                    v
+-----------------------------------------------------------------------+
|                        Repositorios y Fuentes de Datos                |
|                                                                       |
|   +-------------------------------+   +-----------------------------+ |
|   | Firebase Auth + Cloud         |   | JSON Local (assets)         | |
|   | Firestore (NoSQL)             |   | Banco de preguntas y        | |
|   | - Perfiles y XP/puntuación    |   | explicaciones por nivel y   | |
|   | - Estado de desbloqueo        |   | formato (lectura rápida)    | |
|   | - Ranking Top 10 en tiempo    |   +-----------------------------+ |
|   |   real                        |                                   |
|   +-------------------------------+                                   |
+-----------------------------------------------------------------------+
```
### Estructura de carpetas del proyecto (Android / Kotlin-Jetpack Compose)

```text
k-reto/
├── app/
│   ├── google-services.json
│   ├── build.gradle.kts
│   └── src/
│       └── main/
│           ├── assets/
│           │   ├── challenges_basic.json
│           │   ├── challenges_intermediate.json
│           │   └── challenges_advanced.json
│           ├── res/
│           │   ├── drawable/          # Logo tetera pixel-art, iconos (candado, podio)
│           │   └── values/            # Colores (verde acierto, rojo fallo), temas M3
│           └── java/com/kret/app/
│               ├── MainActivity.kt
│               ├── di/                # Inyección de dependencias (si aplica)
│               ├── model/             # Data classes: Player, Level, Challenge, RoundResult, RankingEntry
│               ├── data/
│               │   ├── local/         # Lectura y parseo del JSON en assets
│               │   ├── remote/        # Firebase Auth y Cloud Firestore
│               │   └── repository/    # Repositorios (Auth, Challenges, Ranking, Progress)
│               ├── viewmodel/         # ViewModels con StateFlow y Corrutinas
│               └── ui/
│                   ├── navigation/    # Rutas y navegación entre pantallas
│                   ├── screens/       # Player, Home, Levels, Game, Result, Ranking
│                   ├── components/    # Tarjeta reto, 3 botones, barra progreso, feedback
│                   └── theme/         # Tipografía (incl. código monoespaciado) y Material 3
└── README.md
```
### Gestión del proyecto y planificación
* Control de versiones: Git y GitHub con historial de commits descriptivos.

* Metodología: Adaptación individual de Scrum (Solo Scrum) en 4 Sprints iterativos,apoyada en un tablero de Trello (Backlog, Análisis, Diseño UI/BBDD, Desarrollo, Pruebas, Despliegue, Finalizado).

* Prototipado: Diseño de pantallas y flujos en Figma.

### Instalación y ejecución
1. Clona el repositorio:

```Bash
git clone [https://github.com/](https://github.com/)<usuario>/<repositorio>.git

```
2. Abre el proyecto en Android Studio.

3. Configura el proyecto de Firebase (google-services.json en app/) si utilizas autenticación y Cloud Firestore en tu entorno.

4. Espera la sincronización de Gradle y ejecuta en un emulador o dispositivo Android (Run > Run 'app').

---
<div align="center">

###  **Proyecto Intermodular - Grado Superior DAM (Desarrollo de Aplicaciones Multiplataforma)**

**Autora:** Miriam Mendieta.-  

<p align="center">
  <img src="https://github.com/user-attachments/assets/aa00116b-82be-4a59-9d8b-49728fc7a95b" alt="LogoMiriam" width="100" height="100" />
</p>
</div>
