# AXIS CONFIGURATOR v5.2
### Herramienta de configuración para TidalCycles + SuperCollider + Max for Live

[![License: CC BY-NC-SA 4.0](https://img.shields.io/badge/License-CC%20BY--NC--SA%204.0-lightgrey.svg)](./LICENSE)
![Stack](https://img.shields.io/badge/Stack-TidalCycles%201.10.1%20%C2%B7%20SC%203.14%20%C2%B7%20Max%209-blue)
![Zero Backend](https://img.shields.io/badge/Zero%20Backend-100%25%20Client--Side-orange)
![OS](https://img.shields.io/badge/OS-Windows%2011%20%C2%B7%20macOS%20%C2%B7%20Linux-purple)
[![Live Demo](https://img.shields.io/badge/Demo-GitHub%20Pages-brightgreen)](https://ouroboros99.github.io/axis-configurator/)

**[→ Abrir herramienta (GitHub Pages)](https://ouroboros99.github.io/axis-configurator/)**

---

## ¿Qué problema resuelve?

Instalar **TidalCycles + SuperCollider + Ableton Live con Max for Live** es un campo minado. El 90% de los principiantes abandona antes de escuchar su primer sonido — no porque la herramienta sea mala, sino porque los archivos de configuración son frágiles, los errores son crípticos y la documentación oficial no cubre los casos de falla más comunes.

**AXIS CONFIGURATOR** es un generador de configuración **100% client-side** (un solo archivo HTML, zero backend) que:

1. **Detecta tu OS automáticamente** (Windows / macOS / Linux) y adapta todos los scripts
2. **Genera todos los archivos de configuración correctos** para tu máquina con un clic
3. **Descarga un ZIP completo** con BootTidal.hs, startup.scd, scripts de instalación y verificación
4. **Incluye un SOP interactivo** de 17 pasos en 5 fases para el proceso completo de setup
5. **Diagnostica los 5 errores** que causan el 90% de los abandonos
6. **Explica la arquitectura Max for Live** con diagramas de flujo

---

## Errores que resuelve

Si llegaste aquí buscando uno de estos mensajes de error, este es el lugar correcto.

### `lexical error at character '\65279'`

Causa: El archivo `BootTidal.hs` fue guardado con BOM (Byte Order Mark) por el editor de texto — un carácter invisible que GHCi no puede procesar.

Frecuencia: Muy alta, especialmente en Windows con Notepad o VSCode sin configuración.

Solución: AXIS genera el archivo sin BOM e incluye un script de verificación automática.

### `Variable not in scope: d1` / `Variable not in scope: s`

Causa: La API de TidalCycles cambió en la versión 1.10. La función `d1` ya no existe como antes — se usa `p "1"` o se debe importar el módulo correcto en BootTidal.hs.

Frecuencia: Muy alta — la mayoría de los tutoriales en YouTube son anteriores a v1.10 y ya no funcionan.

Solución: AXIS genera un BootTidal.hs compatible con la API actual (TidalCycles ≥ 1.10).

### Silencio total sin errores — TidalCycles conecta pero no suena nada

Causa: El driver de audio está bloqueado por otra aplicación (Zoom, Discord, Spotify) o hay un proceso zombie de SuperCollider de una sesión anterior.

Frecuencia: Alta.

Solución: El SOP incluye pasos de limpieza de procesos y el startup.scd generado usa configuración explícita del device de audio.

### `exceeded number of interconnect buffers` en SuperCollider

Causa: La memoria asignada a SuperCollider es insuficiente para SuperDirt con el número de canales por defecto.

Frecuencia: Media — más común en máquinas con menos de 8GB RAM.

Solución: El startup.scd generado aumenta la memoria de SC a valores seguros con `s.options.memSize`.

### `flonum de Max no se mueve` / OSC desde Tidal no llega a Max for Live

Causa: La cadena de objetos Max está mal conectada o el puerto OSC en BootTidal.hs no coincide con el configurado en el patch de Max.

Frecuencia: Media.

Solución: El Max Blueprint incluye el diagrama exacto de la cadena de objetos y el test de integración.

---

## Demo

**[→ Abrir AXIS CONFIGURATOR (GitHub Pages)](https://ouroboros99.github.io/axis-configurator/)**

No requiere instalación. No requiere servidor. Funciona offline después de la primera carga.

---

## Uso local

```bash
git clone https://github.com/ouroboros99/axis-configurator.git
cd axis-configurator
# Abrir index.html en cualquier navegador moderno
```

O descargar directamente: [index.html](./index.html) → abrir en el navegador.

---

## Funcionalidades v5.2

### 01 — Generador de Configuración
- Detección automática de OS (Windows / macOS / Linux) — scripts adaptados por plataforma
- Formulario con validación avanzada de rutas e indicadores inline
- Toggle para activar/desactivar integración Max for Live con puerto OSC configurable
- Export/Import de configuración en JSON
- **ZIP descargable** con todos los archivos listos:
  - `BootTidal.hs` — sin BOM, API 1.10 compatible
  - `startup.scd` — boot SC con memoria aumentada y device explícito
  - `sesion.tidal` — sesión de inicio
  - `settings.json` — VSCode con extensión TidalCycles
  - Script de instalación (`.ps1` Windows / `.sh` macOS-Linux)
  - Script de verificación BOM
  - Script anti-suspensión

### 02 — SOP Checklist Interactivo
- 17 pasos en 5 fases colapsables con progreso guardado en localStorage
- Keyboard navigation (Enter/Espacio para marcar pasos)
- Notas de compatibilidad específicas por OS

### 03 — Diagnóstico de Errores
- 5 errores con causa raíz + solución step-by-step
- Notas específicas por OS (Windows, macOS, Linux)
- Snippets copiables con un clic

### 04 — Max for Live Blueprint
- Diagrama de flujo de señal OSC completo
- Guía de construcción del patch en 5 pasos
- Test de integración

### UX
- Wizard de bienvenida (4 pasos, primer uso)
- Interfaz en español e inglés (toggle en el header)
- Dim mode con preferencia guardada
- ARIA labels y keyboard navigation completos
- Responsive en móvil y tablet

---

## Stack técnico

- **HTML5** — un solo archivo, semántica correcta, ARIA completo
- **CSS puro** con variables CSS para theming
- **JavaScript Vanilla** — sin frameworks, sin dependencias de runtime
- **JSZip CDN** — únicamente para la función de descarga ZIP
- **Google Fonts** — JetBrains Mono + VT323

Zero dependencias de runtime propias. Zero backend. Funciona abriendo el archivo.

---

## Configuración compatible

| Componente | Versión testeada |
|---|---|
| TidalCycles | 1.10.1 |
| GHCup / GHCi | ≥ 9.4 |
| SuperCollider | 3.14 |
| SuperDirt | ≥ 1.7 |
| Max for Live | 9 |
| Ableton Live | 12 |
| ASIO4ALL | v2 (Windows) |
| Windows | 11 |
| macOS | Ventura / Sonoma |
| Linux | Ubuntu 22.04+ |
| VSCode + TidalCycles ext. | Última |

---

## Contribuir

Ver [CONTRIBUTING.md](./CONTRIBUTING.md) para guía de contribución, áreas de ayuda prioritarias y convenciones de código.

---

## Licencia

**CC BY-NC-SA 4.0** — libre para usar, adaptar y redistribuir bajo la misma licencia.  
Uso comercial, venta o inclusión en productos pagos **está prohibido**.  
Desarrollado por **Axis Opus** ([ouroboros99](https://github.com/ouroboros99)).  
Ver [LICENSE](./LICENSE) para el texto completo.

---

## Changelog

### v5.2 — Abril 2026
- Licencia migrada de MIT a CC BY-NC-SA 4.0
- Eliminado Tailwind CDN — CSS propio cubre todos los utilitarios de layout
- Footer y ZIP actualizados con atribución correcta (Axis Opus / ouroboros99)

### v4.0 — Abril 2026
- Detección automática de OS — PowerShell para Windows, bash para macOS/Linux
- Botón ZIP con JSZip
- Generadores para macOS (Core Audio) y Linux (ALSA/JACK)
- Wizard de bienvenida, modal de Changelog
- Botón "Copiar todos los resultados"
- Tooltips, validación avanzada de rutas, ARIA completo

### v3.0 — Marzo 2026
- Export/Import de configuración JSON
- Sección Max Blueprint
- Generador BootTidal.hs para API TidalCycles 1.10.1

### v2.0 — Enero 2026
- SOP Checklist interactivo (17 pasos, 5 fases)
- Diagnóstico de errores (5 errores)

---

*Hecho con frustración convertida en herramienta — para la comunidad de live coding.*
