# Verificación y mejora del standalone
## Analista de Asistencia a Usuarios — Poder Judicial de Corrientes (Conv. 07/26)

**Archivo verificado:** `standalone_analista_asistencia_pj_corrientes.html` (483.716 bytes, 5.198 líneas)
**Material de referencia:** `prompt.md` + `01_Guia_Teorica.md`, `02_Banco_Preguntas.md`, `03_Simulacros_y_Entrevista.md`, `04_Plan_de_Estudio.md`, `OFICINA DE ASISTENCIA A USUARIOS.txt` y `progreso_analista_pjc_2026-11-21.json`

> Nota: el screenshot que mencionás no llegó en el mensaje (no vino ninguna imagen adjunta), así que generé capturas propias del archivo abierto en Chrome.

---

## 1. Veredicto en una línea

**El HTML está alineado con el pedido en lo sustancial** (las 10 secciones, los 500 ítems y los 4 archivos están completos y sin inventos), **pero tenía tres defectos que lo volvían poco usable en la práctica**: el progreso se perdía al recargar, importar un JSON borraba datos y varios íconos/diagramas rompían el diseño. Todo eso está corregido en la versión `v1.1.0` entregada.

---

## 2. Cómo lo verifiqué

| Qué | Cómo |
|---|---|
| Fidelidad del contenido | Extraje el JSON embebido (`<script id="contenido">`) y lo comparé carácter por carácter con los 4 `.md` |
| Integridad del banco | Parseo real en navegador: 500 preguntas, 4 opciones y clave en todas |
| Funcionamiento | Chrome headless vía DevTools Protocol: 10 rutas, consola, cronómetros, localStorage |
| Regresiones | 20 pruebas automáticas (conteos, práctica, simulacro, caso, entrevista, plan, export/import, persistencia, tema, consola) |
| Visual | Capturas a 1440×900 y 390×844, antes y después |
| Diagramas | `getBBox()` de los 17 SVG contra su `viewBox` para detectar texto recortado |

---

## 3. Alineación con los `.md` (contenido)

Los cuatro textos embebidos son **idénticos** a los originales una vez normalizados los saltos de línea (solo se quitaron los marcadores `\newpage` de LaTeX y se colapsaron líneas en blanco). **No se inventó, resumió ni omitió contenido.**

| Archivo | Original | Embebido | Resultado |
|---|---|---|---|
| `01_Guia_Teorica.md` | 53.582 B | 51.370 B | Idéntico tras normalizar |
| `02_Banco_Preguntas.md` | 76.560 B | 72.231 B | Idéntico tras normalizar |
| `03_Simulacros_y_Entrevista.md` | 51.661 B | 49.110 B | Idéntico tras normalizar |
| `04_Plan_de_Estudio.md` | 18.711 B | 17.469 B | Idéntico tras normalizar |

**Conteos verificados en ejecución:**

| Elemento | Pedido | En la app |
|---|---|---|
| Preguntas de banco | 500 | **500** (50+55+55+50+50+55+55+50+50 = 470, + 30 integradoras) |
| Preguntas con 4 opciones y clave | todas | **500/500**, sin faltantes |
| Simulacros | T4 ×5 (20) + T9 + T10 (40) | **7** (5×20 + 2×40) |
| Diagnóstico | 20 preguntas / 25 min | **20** / 1.500 s |
| Entrevista | 30 (bloques A–D) | **30** (5/12/6/4) |
| Casos prácticos | 20 con rúbrica | **20** |
| Plan | 28 días | **28** |
| Ejes | 9 × 7 subsecciones | **9 × 7** |
| Diagramas SVG | ≥ 9 | **17** |
| Animaciones paso a paso | ≥ 5 | **5** |

---

## 4. Alineación con `prompt.md` (22 criterios de aceptación)

| # | Criterio | Estado |
|---|---|---|
| 1 | Abre con doble clic en Chrome, Firefox y Edge | ⚠️ Verificado en Chrome/Chromium (headless). Sin APIs propietarias, pero **no lo probé en Firefox** |
| 2 | 100 % offline | ✅ No hay `src`/`href` externos ni CDN |
| 3 | 10 secciones navegables | ✅ 10 enlaces + router por hash |
| 4 | 500 preguntas con corrección automática | ✅ |
| 5 | Simulacros con cronómetro | ✅ (30:00 → 29:59 comprobado) |
| 6 | 20 casos con resolución y rúbrica | ✅ 6 criterios × 8 pts |
| 7 | 30 entrevistas con temporizador y rúbrica | ✅ 90 s, 6 × 1 pt |
| 8 | Diagnóstico de 20 | ✅ |
| 9 | Plan de 28 días con checklist | ✅ |
| 10 | Guarda y recupera progreso de `localStorage` | ❌→✅ **Estaba roto** (ver defecto 1) |
| 11 | Exporta e importa JSON | ❌→✅ **Estaba roto** (ver defecto 2) |
| 12 | Dashboard con índice y estado por eje | ✅ |
| 13 | Recomendaciones personalizadas | ✅ |
| 14 | ≥ 9 diagramas SVG | ✅ 17 |
| 15 | ≥ 5 animaciones | ✅ 5 |
| 16 | Responsive | ✅ (390 px sin desborde) |
| 17 | Accesibilidad básica | ✅ y mejorada (foco, `aria-label`, skip link) |
| 18 | Temas claro y oscuro | ✅ + alto contraste |
| 19 | Sin errores de consola | ✅ (0 excepciones en 10 rutas) |
| 20 | README embebido | ✅ con changelog |
| 21 | Marcadores `PARTE X/Y` | ✅ (`PARTE 1/9` … `9/9`) |
| 22 | JSON embebido + cómo editarlo | ✅ documentado |

**Extras del apartado «Lo que falta en el pedido original»:** buscador global ✅, marcadores ✅, notas por eje ✅, repaso espaciado ✅ (implementado ahora), estadísticas de tiempo ✅, comparación de intentos ✅, racha ✅, examen rápido ✅, solo errores ✅, informe `.md` ✅, impresión ✅, atajos ✅, ayuda ✅, modo lectura ✅, recordatorio de distractores ✅, enlace pregunta↔teoría ✅, versionado ✅, créditos ✅.

---

## 5. Defectos encontrados y corregidos

### Crítico 1 — El progreso se perdía en cada recarga

La función que fusiona el estado guardado recorría **las claves del destino**, no las de la fuente. Resultado: los objetos que nacen vacíos (`banco.respuestas`, `casos`, `entrevista`, `teoria.notas`) no recibían ninguna clave y se descartaban al cargar.

*Evidencia:* 8 respuestas y 1 caso guardados → recargar → 0 y 0.
*Impacto:* responder el banco, autoevaluar casos o escribir respuestas de entrevista no servía de nada al cerrar la pestaña.
*Corrección:* se recorre la fuente; en la raíz solo se aceptan claves conocidas y en los objetos anidados se copia todo lo que venga.

### Crítico 2 — Importar el JSON devolvía «ok» y borraba datos

Mismo bug de fusión, agravado porque el archivo `progreso_analista_pjc_2026-11-21.json` usa el esquema anterior (`usuario.diagnosticos`, `prefs`, `r/k/ok`, `crit[] + auto`, `actividad` como lista de fechas).

*Antes:* `importarOk: true`, pero diagnóstico `null`, 0 casos, 0 entrevistas, respuestas vacías, racha 0, preferencias ignoradas.
*Después:* diagnóstico 20/20 + historial, respuestas convertidas, 1 caso, 30 entrevistas, 2 simulacros, racha 1, preferencias aplicadas.
*Corrección:* detector de formato + conversión completa (`convertirLegado`) y aviso con el resumen de lo convertido.

### Alto 3 — Las fechas se adelantaban un día de noche

`hoyISO()` usaba `toISOString()` (UTC). En Argentina (UTC−3), **desde las 21:00 ya devolvía el día siguiente**: el «día actual» del plan, la racha y los registros se corrían.
*Corrección:* fecha local con `getFullYear/getMonth/getDate`.

### Alto 4 — Íconos gigantes que rompían el diseño

`.ic` no tenía tamaño base y la regla global `svg{ height:auto }` estiraba cada ícono al ancho del contenedor. En la barra superior, las tarjetas del plan y el feedback del quiz aparecían como círculos de 100–300 px.

| Antes | Después |
|---|---|
| ![Plan antes](capturas/15-plan.png) | ![Plan después](capturas5/15-plan.png) |

*Corrección:* `.ic{ width:1.15em; height:1.15em }`.

### Alto 5 — Diagramas con texto recortado y escala desmedida

Los SVG con `viewBox` se escalaban al ancho de la tarjeta (texto hasta 3×) y **5 de los 17 tenían contenido fuera del `viewBox`** (leyendas cortadas por izquierda, derecha o abajo).

| Antes | Después |
|---|---|
| ![Estudio antes](capturas/11-estudio.png) | ![Estudio después](capturas5/11-estudio.png) |

*Corrección:* tamaño derivado del `viewBox` (×1.4), centrado, y ajuste de los 5 diagramas. Auditoría final: **17/17 dentro de su `viewBox`**.

### Medio 6 — Justificación arbitraria en las respuestas

Se tomaba la primera línea de «Errores frecuentes» que contuviera la primera palabra del subtema. Con 119 preguntas de subtema «general» eso mostraba un texto sin relación.
*Corrección:* puntuación por palabras significativas compartidas con el enunciado; si no hay relación suficiente se muestra «(sin justificación disponible)». Además, ya no se repite la misma frase en el recordatorio.

### Medio 7 — Buscador global poco descubrible

Solo funcionaba con **Enter** (sin botón ni pista) y, si había un examen en curso, reemplazaba la vista y destruía la sesión.
*Corrección:* búsqueda en vivo desde 3 letras (350 ms de espera) y, con examen activo, resultados en un modal.

### Bajo 8 — Otros

- `id="dt"`/`id="tdd"` repetidos en todos los diagramas → ids únicos.
- Sin enlace «Saltar al contenido» → agregado (foco directo al `main`).
- Pie con la versión y los conteos escritos a mano → ahora derivados del contenido real.
- «1 preguntas respondidas», «1 días» → concordancia corregida.
- `correccionInmediata` declarada dos veces → unificada.
- El comentario prometía repaso espaciado pero no estaba implementado → implementado (las falladas se anteponen al resto).

---

## 6. Pruebas ejecutadas sobre la versión final

```
✓ 500 preguntas · 7 simulacros · 20 diagnóstico · 30 entrevista · 20 casos · 28 días · 9 ejes
✓ práctica registra respuestas           ✓ feedback visible al responder
✓ repaso espaciado antepone falladas     ✓ simulacro de 20 preguntas
✓ cronómetro corre (30:00 → 29:59)       ✓ caso guarda rúbrica
✓ entrevista arranca temporizador        ✓ plan marca día
✓ exportar/importar conserva datos       ✓ tema oscuro aplica
✓ persistencia tras recargar (v1.1.0)    ✓ sin excepciones / sin errores de consola
✓ importación del JSON viejo completa     ✓ 17/17 diagramas dentro de su viewBox
```

**20/20 pruebas OK.** Tamaño final: **480,8 KB** (el pedido admite 2–3 MB).

---

## 7. Qué cambió en la entrega

- **Un solo archivo**, mismo nombre, autocontenido, offline, sin frameworks (se mantiene el requisito).
- El **contenido de los 4 `.md` no se tocó**: se verificó que es idéntico.
- La versión del esquema de progreso pasa a **1.1.0**; los JSON viejos se convierten al importar (misma versión mayor).
- El **archivo original de `Downloads` quedó intacto**; la versión mejorada está en el workspace.

---

## 8. Recomendaciones (no bloqueantes)

1. **Probar en Firefox y Edge reales** antes de dar por cerrado el criterio 1.
2. **Cargar tu progreso**: el JSON del 21/11 se importa y conserva todo; conviene reexportarlo desde Ajustes para dejarlo en el formato nuevo.
3. Si querés conservar el score de los casos del formato viejo, revisá que la rúbrica reconstruida coincida con lo que recordás (se conserva el puntaje total; la distribución por criterio es la mejor reconstrucción posible).
4. Los datos institucionales marcados «requiere verificación» en el material siguen marcados: no se inventó normativa ni nombres de sistemas.
