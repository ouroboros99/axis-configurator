# Contribuir a AXIS CONFIGURATOR

Gracias por querer mejorar esta herramienta. Está construida para la comunidad, por la comunidad.

---

## Filosofía del proyecto

- **Zero friction**: cualquier cambio debe reducir, no aumentar, la complejidad para el usuario final
- **Zero backend**: todo debe funcionar abriendo el archivo HTML localmente, sin servidor
- **Multi-plataforma**: Windows, macOS y Linux son ciudadanos de primera clase desde v5
- **Documentado**: el código JS es modular y comentado — nuevas funciones deben seguir el mismo patrón
- **Español**: la interfaz y los mensajes de error son en español para la comunidad latinoamericana

---

## Cómo contribuir

### Opción 1: Reportar un error o sugerir una mejora

Abrir un Issue en GitHub con:
- **Título claro**: `[ERR] descripción del problema` o `[FEATURE] idea de mejora`
- **Contexto**: OS (Windows/macOS/Linux), versión de TidalCycles, SC, Max
- **Reproducción**: qué pasos llevan al problema
- **Comportamiento esperado vs actual**

### Opción 2: Pull Request

1. Fork del repositorio
2. Crear una rama descriptiva: `git checkout -b fix/bom-verification` o `git checkout -b feature/jack-support`
3. Hacer los cambios
4. Verificar que el archivo HTML sigue siendo un solo archivo (todas las dependencias son CDN)
5. Abrir el PR con descripción de qué cambia y por qué

---

## Áreas donde se necesita ayuda

### Alta prioridad
- [ ] **Soporte Jack/PipeWire en Linux**: el generador de SC boot para Linux actualmente usa un comentario genérico — necesitamos detección más fina del backend de audio (JACK, PipeWire, ALSA puro)
- [ ] **Más targets OSC**: soporte para Pure Data, OSCQuery, otros receptores además de Max
- [ ] **Más errores de diagnóstico**: ERR-06 en adelante — otros errores frecuentes no documentados
- [ ] **Versiones de SC/Tidal**: dropdown para seleccionar versiones y adaptar el código generado

### Media prioridad
- [ ] **Traducciones**: versión en inglés para la comunidad global (el sistema de OS detection ya adapta los scripts; faltaría la UI)
- [ ] **Historial de configuraciones**: guardar múltiples configs en localStorage con nombres
- [ ] **Modo offline total**: cachear Google Fonts y Tailwind localmente para uso sin internet
- [ ] **Validación de rutas ampliada**: verificar que la ruta existe (si el navegador lo permite) o detectar patrones más específicos por OS

### Baja prioridad / mejoras de UX
- [ ] **Más pasos en el wizard**: expandir el tour con capturas o animaciones
- [ ] **Tema claro**: variante light mode además del dim mode actual
- [ ] **Exportar SOP como PDF**: imprimir el checklist como referencia física

---

## Estructura del código

El archivo `index.html` está organizado en secciones claramente delimitadas:

```
<style>
  /* === VARIABLES === */    ← Colores y tokens de diseño
  /* === COMPONENTS === */   ← Estilos de cada componente UI
</style>

<body>
  <!-- TOUR OVERLAY -->      ← Wizard de bienvenida (4 pasos)
  <!-- CHANGELOG MODAL -->   ← Modal de historial de cambios
  <!-- HEADER -->            ← Título, OS badge, dim toggle, tour/changelog buttons
  <!-- NAV -->               ← Navegación de 4 tabs
  <!-- SECTION 01 -->        ← Generador
  <!-- SECTION 02 -->        ← SOP Checklist
  <!-- SECTION 03 -->        ← Diagnóstico
  <!-- SECTION 04 -->        ← Max Blueprint
  <!-- FOOTER -->            ← CC BY-NC-SA 4.0, links, atribución
</body>

<script>
  // OS DETECTION          ← detectOS() → constante global OS
  // NAVIGATION            ← showSection()
  // DIM MODE              ← toggleDim()
  // TOUR WIZARD           ← openTour(), closeTour(), tourStep()
  // CHANGELOG MODAL       ← openChangelog(), closeChangelog()
  // ACCORDION             ← toggleAcc()
  // CLIPBOARD             ← copyCode(), copyAllResults()
  // TOGGLE MAX PORT       ← toggleMaxPort()
  // PATH VALIDATION       ← validatePath()
  // SOP CHECKLIST         ← toggleStep(), togglePhase(), updateSOPProgress(), etc.
  // JSON EXPORT/IMPORT    ← exportConfig(), importConfig()
  // PATH UTILITIES        ← toForwardSlash(), toBackslash(), toUnix(), etc.
  // GENERATORS            ← buildBootTidalLines(), buildPowerShell(), buildBashInstall(),
  //                          buildVerifyWindows(), buildVerifyUnix(),
  //                          buildSCBoot(), buildVSCodeSettings(), buildSession(),
  //                          buildPowerManagement()
  // ZIP DOWNLOAD          ← downloadZip() (usa JSZip CDN)
  // MAIN GENERATE         ← generateConfigs()
  // INIT                  ← (function init() { ... })()
</script>
```

### Para agregar soporte de un nuevo OS / audio backend

1. Agregar el OS en la constante `OS_LABELS`
2. Modificar `buildSCBoot()` para agregar el bloque de hardware del nuevo backend
3. Modificar `buildPowerManagement()` para el script correspondiente
4. Actualizar la validación de rutas en `validatePath()` si aplica
5. Actualizar el `README-axis.txt` dentro de `downloadZip()`

### Para agregar un nuevo generador de código

1. Agregar la función `buildXxx()` en la sección `GENERATORS`
2. Agregar un `<div class="code-wrap">` en el HTML con un `id` único para el `<pre>`
3. Llamar a `buildXxx()` dentro de `generateConfigs()` y asignar al `textContent` del `<pre>`
4. Agregar el nuevo `<pre>` al array `ids` dentro de `copyAllResults()`
5. Agregar el archivo correspondiente en `downloadZip()`

---

## Convenciones de código

- **Funciones**: camelCase, con comentario JSDoc de una línea
- **IDs HTML**: kebab-case, descriptivos (`ps-out`, `verify-out`, `sop-fill`)
- **CSS**: variables CSS para colores, clases componentes bien nombradas
- **No frameworks JS**: vanilla únicamente — sin React, Vue, jQuery
- **ARIA**: todos los controles interactivos deben tener `aria-label`, `aria-expanded` o `role` apropiado

---

## Preguntas

Abrir un Issue con el tag `[QUESTION]`.

*Gracias por ayudar a que más personas puedan hacer live coding sin perder la paciencia en el setup.*
