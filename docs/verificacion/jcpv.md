# Verificación · «Lo que cada tarea muestra de su responsable»

Requisito de `openspec/specs/tasks/spec.md` (líneas 97–114). Solo este requisito: el resto de la
capability `tasks` queda fuera.

- **Scenarios del requisito:** 3
- **Cubiertos por la suite existente:** 0

## A.1 · Matriz de trazabilidad

Estado de la suite **antes** de añadir nada (rama `s4/start`). Para cada fila se abrió la suite
entera: `backend/tests/functional/` solo tenía `auth/`, y ningún test hace una petición a
`/api/v1/tasks`.

| Scenario (una línea) | Prueba que lo cubre | Estado | Si «no lo sé», qué faltó |
|---|---|---|---|
| Al obtener una tarea de "Ada Lovelace", su `assignee` trae nombre "Ada Lovelace" e iniciales | — | No cubierto | — |
| Al obtener cualquier tarea (suelta o en la lista), su `assignee` no trae email ni otros datos de acceso | — | No cubierto | — |
| Si el responsable se registró sin nombre, `assignee.fullName` llega nulo y las iniciales siguen llegando | — (lo más parecido: `Auth \| iniciales › sin nombre, las iniciales salen del email`, que mira las iniciales en el **login**, no en el `assignee` de una tarea) | No cubierto | — |

No hay ningún «no lo sé» en la cobertura: con cero tests sobre tareas no había nada ambiguo que
leer. La duda real apareció en la spec, no en la suite (ver línea 2).

## A.2 · Tests escritos

`backend/tests/functional/tasks/assignee.spec.ts`, grupo `Tasks | responsable`, uno por scenario.
Cada uno lee la tarea por las dos vías (`GET /api/v1/tasks/:id?today=…` y `GET /api/v1/tasks`) y
con una sesión distinta de la del responsable.

| Scenario | Test | Resultado |
|---|---|---|
| Responsable identificable | `el responsable llega con su nombre y sus iniciales` | ✅ verde |
| La tarea no filtra datos de cuenta | `el responsable de una tarea no trae el email ni otros datos de la cuenta` | ❌ **rojo** |
| Responsable sin nombre | `un responsable sin nombre llega con el nombre nulo y con iniciales` | ✅ verde |

**Por qué está rojo** (no se ha tocado el código ni aflojado el test): la tarea suelta pasa, pero
la **lista** devuelve el `assignee` con `email`, `createdAt` y `updatedAt`.
`TaskDetailTransformer` usa `TaskAssigneeTransformer` (`id`, `fullName`, `initials`), mientras que
`TaskTransformer`, el de la lista, sigue usando `UserTransformer`, que incluye el email. Fallo
literal: `la lista expone el email del responsable: expected { id: 10, …(5) } to not have nested
property 'email'`.

Fuera del alcance de los tests, pero visto al leer el código: `POST /tasks` y
`PATCH /tasks/:id/status` también responden con `TaskTransformer`, así que también exponen el email.

## B · Las tres líneas

1. **Cubiertos antes de mirar → cubiertos de verdad:** _[por rellenar por jcpv: la estimación a
   ojo de antes del primer prompt]_ → 0 de 3.

2. **Hueco de test o hueco de spec: «La tarea no filtra datos de cuenta», por el `id`.** El
   requisito dice que no se expone «ningún otro dato de esa cuenta» aparte de nombre e iniciales,
   el scenario solo prohíbe el email y los «datos de acceso», y el `TaskAssigneeTransformer` manda
   el `id` a propósito: si el `id` incumple la regla, lo que falla es la spec (o el código); si no
   la incumple, esa excepción no está escrita en ningún sitio y la estaba poniendo yo.

3. **Lo que decidí yo al escribir los tests:** dejé pasar el `id` en el `assignee` (lista
   permitida `id`, `fullName`, `initials`); para el responsable sin nombre solo exigí unas iniciales
   no vacías, sin fijar su valor, aunque hoy salen del email (`ada@example.com` → `AE`), lo que
   choca con el «sin recurrir a su email» del scenario; y entendí «obtener una tarea» como las dos
   lecturas (lista y tarea suelta), no como las respuestas de crear o cambiar el estado, que
   también la devuelven.
