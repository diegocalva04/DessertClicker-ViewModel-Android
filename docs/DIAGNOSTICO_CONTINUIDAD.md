# Diagnóstico de continuidad entre prácticas

Fecha: 15 de octubre de 2026.

## Base seleccionada

La segunda práctica continúa desde el proyecto terminado en la primera práctica. Se creó una copia independiente que conserva el historial Git previo, sin modificar el repositorio original.

La base es compatible con el segundo codelab porque coincide con la rama `main` moderna del proyecto oficial en `app/src` y `app/build.gradle.kts`. Conserva además los siete registros del ciclo de vida agregados durante la primera práctica.

## Estado antes de incorporar ViewModel

- `DessertClickerApp()` administra cinco valores mediante `rememberSaveable`.
- La lógica de ventas y progresión de postres reside en `MainActivity.kt`.
- `Datasource` contiene la lista de postres.
- La interfaz usa Jetpack Compose y Material 3.
- La función de compartir usa un `ACTION_SEND`.
- El proyecto utiliza Kotlin DSL y Lifecycle 2.8.7.

## Verificaciones reales de la base

Se ejecutó `gradlew.bat clean assembleDebug` y la compilación terminó correctamente.

El APK se instaló en un teléfono Xiaomi 23100RN82L con Android 15, API 35. La actividad inició en frío y Logcat registró, en orden, `onCreate Called`, `onStart Called` y `onResume Called`.

La jerarquía de accesibilidad mostró los valores iniciales `Desserts sold: 0` y `Total Revenue: $0`. Después de diez pulsaciones reales sobre el postre, mostró `Desserts sold: 10` y `Total Revenue: $75`, lo que confirma que el contador, los ingresos y la progresión de precio funcionaban antes de la refactorización.

## Criterio de migración

Las cinco variables `rememberSaveable` se sustituirán por una única fuente de verdad administrada por `DessertViewModel`. Los métodos de ciclo de vida y la interfaz moderna se conservarán. La rama oficial `viewmodel`, basada en una versión antigua de Gradle y Material, se utilizará como referencia arquitectónica y no como reemplazo completo del proyecto actual.
