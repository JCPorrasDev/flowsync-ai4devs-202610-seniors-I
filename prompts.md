# Prompts

Aquí van **todos los prompts que lanzaste** para hacer el ejercicio, en el orden en que los
lanzaste, con el modelo y la herramienta de cada uno.

Esto no es papeleo. Lo que se revisa es **cómo pediste las cosas**, no solo lo que salió: un
resultado flojo con un prompt bueno y un resultado flojo con un prompt vago necesitan feedback
distinto, y sin este archivo no se distinguen.

## Cómo rellenarlo

- Un apartado `## Prompt N` por cada prompt.
- **Pega el prompt tal cual lo lanzaste**, dentro del bloque de código, aunque ocupe diez líneas
  y aunque tenga faltas. No lo reescribas para que quede bien: el que arreglaste mentalmente
  después no es el que lanzaste.
- Incluye también los que **no funcionaron**. Suelen ser los más útiles de leer.
- `Modelo` y `Herramienta` en todos. Si cambiaste de una a otra a mitad, se nota aquí.

---

## Prompt 1

**Modelo:** Claude Opus 5.5 (`claude-opus-5-5`)
**Herramienta:** Claude Code

```
vas a trabajar solo sobre el requisito «Lo que cada tarea muestra de su responsable» que está en openspec/specs/tasks/spec.md, el spec tiene otros requisitos y quedan por fuera del alcance del ejercicio, el objetivo es construir una matriz de trazabilidad: revisar, scenario a scenario, si cada uno tiene un test que lo cubre, y escribir los que falten. La matriz de trazabilidad y las tres líneas van juntas en docs/verificacion/jcpv.md
```

**Qué salió:** matriz (0 de 3 cubiertos), tres tests nuevos en `backend/tests/functional/tasks/` (uno en rojo: la lista expone el email del responsable) y las tres líneas, con la primera cifra pendiente de rellenar a mano.
