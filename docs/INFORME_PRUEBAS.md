# Informe de pruebas de Dessert Clicker con ViewModel

Fecha: 15 de octubre de 2026.

## Entorno de ejecución

- Dispositivo físico: Xiaomi 23100RN82L.
- Versión de Android: 15.
- Nivel de API: 35.
- Resolución física informada por Android: 720 x 1600 píxeles.
- Conexión: ADB autorizada mediante el identificador `H6LJ8TLJS8KR4TMR`.

## Resultados funcionales

| Prueba | Procedimiento ejecutado | Resultado real |
|---|---|---|
| Inicio | Instalación del APK, cierre forzado e inicio en frío de `MainActivity`. | Correcto. Se mostró el cupcake con 0 ventas y $0. |
| Venta | Diez pulsaciones ADB dentro de los límites del postre. | Correcto. La jerarquía mostró 10 ventas y $75. |
| Progresión | Se alcanzó el umbral del siguiente postre. | Correcto. La imagen cambió del cupcake inicial al donut. |
| Orientación | Se cambió la rotación del sistema de vertical a horizontal. | Correcto. Después de destruirse y recrearse la actividad permanecieron 10 ventas y $75, con el donut sincronizado. |
| Segundo plano | Se envió la aplicación a Inicio y se volvió a abrir. | Correcto. Permanecieron 10 ventas y $75. |
| Compartir | Se pulsó el botón Share y se inspeccionó el selector del sistema. | Correcto. `com.android.intentresolver` recibió el texto `I've clicked 10 Desserts for a total of $75 #AndroidDessertClicker`. Al regresar se conservó el estado. |
| Ciclo de vida | Se filtró Logcat con la etiqueta `MainActivity`. | Correcto. Los registros continuaron activos y mostraron las transiciones esperadas. |
| Compilación final | Se ejecutó `clean testDebugUnitTest lintDebug assembleDebug`. | Correcto. Gradle terminó con `BUILD SUCCESSFUL` y generó el APK. |

## Evidencia de cambio de configuración

La rotación produjo la siguiente secuencia real en Logcat:

```text
onPause Called
onStop Called
onDestroy Called
onCreate Called
onStart Called
onResume Called
```

Antes y después de esa secuencia, la jerarquía de la pantalla mostró 10 ventas y $75. La persistencia corresponde a la instancia de `DessertViewModel`, que sobrevive a la recreación de la actividad durante el cambio de configuración.

## Compilación y análisis estático

La validación final terminó correctamente. El APK `app-debug.apk` tiene 9.545.029 bytes. Android Lint informó 0 errores y 27 advertencias. Las advertencias corresponden principalmente a versiones y recursos conservados del proyecto oficial, además de la dependencia Lifecycle fijada en la misma versión 2.8.7 que utiliza el proyecto.

La tarea `testDebugUnitTest` informó `NO-SOURCE` porque el proyecto oficial no incluye pruebas unitarias locales. Las verificaciones funcionales se realizaron sobre el dispositivo físico.

## Comparación con la solución oficial

`DessertUiState.kt` coincide con el archivo de la rama oficial `viewmodel`. `DessertViewModel.kt` conserva las mismas propiedades, flujo de estado y lógica; las únicas diferencias son orden de imports y sangría.

`MainActivity.kt` aplica la misma integración mediante `viewModel()`, `collectAsState()` y `viewModel::onDessertClicked`. Se mantienen deliberadamente Material 3, Kotlin DSL, Gradle moderno, los recursos actuales y los registros del ciclo de vida de la primera práctica, porque la rama `viewmodel` oficial corresponde a una versión anterior del proyecto.

## Evidencias gráficas

- `01_aplicacion_inicial.png`: ejecución inicial en el teléfono.
- `02_ventas_y_progresion.png`: diez ventas, $75 y cambio a donut.
- `03_estado_conservado_horizontal.png`: los mismos valores e imagen después del giro.

No se guardó una captura del selector de compartir porque la pantalla mostraba contactos personales. La prueba se verificó mediante la jerarquía de accesibilidad y el texto exacto entregado al selector.
