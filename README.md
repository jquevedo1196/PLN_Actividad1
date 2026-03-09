# CONTEXTO - Actividad 1: Etiquetado Morfosintáctico con HMM

## Resumen General
Implementar en Python un **etiquetador morfosintáctico** basado en **Modelos Ocultos de Markov (HMM) bigrama** y el **algoritmo de Viterbi** para etiquetar la oración:

> **«Habla con el enfermo grave de trasplantes.»**

---

## Estructura de la Actividad (3 Partes)

### Parte 1: Construcción de tablas de probabilidades
- **Entrada:** Corpus `Corpus-tagged` (formato Wikicorpus/FreeLing, español)
- **Formato del corpus:** XML con `<doc>`, frases separadas por línea en blanco, cada palabra en una línea con: `token | lema | POS_tag | sentido` → **Solo usar columnas 1 (token) y 3 (POS tag)**
- **Etiquetas:** Formato EAGLES de FreeLing (ej: `VSIP3S0` = Verbo Semiauxiliar Indicativo Presente 3ª persona Singular sin género). Referencia: https://freeling-user-manual.readthedocs.io/en/v4.1/tagsets/tagset-es/
- **Calcular:**
  1. **Probabilidades de emisión:** P(palabra | etiqueta)
  2. **Probabilidades de transición:** P(etiqueta_i | etiqueta_{i-1})
- **Entregable:** Tablas de emisión y transición en archivo **Excel (.xlsx)**

### Parte 2: Etiquetado con Viterbi
- **Oración a etiquetar:** `Habla con el enfermo grave de trasplantes.`
- **Pasos:**
  1. Construir la **matriz de Viterbi** (observaciones × estados relevantes)
  2. Considerar la transición al **estado final** (el punto `.`)
  3. Trazar la **ruta inversa** (backtracking) para obtener la mejor secuencia de etiquetas
  4. Presentar la oración etiquetada
- **Simplificación permitida:** Eliminar de la matriz estados/etiquetas que no apliquen a las palabras de la oración
- **Entregable:** Matriz de Viterbi en archivo **Excel (.xlsx)**

### Parte 3: Análisis y Reflexión
Responder razonadamente:
1. ¿Es correcto el etiquetado obtenido para `«Habla con el enfermo grave de trasplantes.»`? Justificar.
2. Etiquetar también `«El enfermo grave habla de trasplantes.»` → ¿Es correcto? Justificar.
3. ¿Limitaciones del etiquetador creado?
4. ¿Posibles mejoras?

---

## Restricciones Críticas
- **Resolver en el Jupyter Notebook proporcionado** (obligatorio, si no → 0 puntos)
- **Usar etiquetas EAGLES del corpus** (si se usan otras → 0 puntos)
- **NO usar librerías de NLP** (NLTK, spaCy, etc.) → implementar todo manualmente (si no → 0 puntos)
- Código **bien comentado** (penalización si no)
- Mostrar **todos los resultados** generados por el código
- No plagio ni copia entre alumnos

## Entregables
1. **Jupyter Notebook** con código Python comentado + resultados + respuestas a preguntas
2. **Excel (.xlsx)** con tablas de emisión y transición (Parte 1)
3. **Excel (.xlsx)** con matriz de Viterbi (Parte 2)

---

## Datos Técnicos del Corpus
- **Fuente:** Wikicorpus (Wikipedia en español, anotado con FreeLing)
- **Formato:** Cada línea = `token  lema  POS_tag  sentido` (separados por espacios/tabs)
- **Contracciones:** "del" → dos líneas: "de" + "el"
- **Nombres compuestos:** "Luis Buñuel" → un token: "luis_buñuel"
- **Separador de frases:** Línea en blanco
- **Documentos:** Delimitados por tags `<doc id="...">`

---

## Estructura del Notebook (Actividad1.ipynb)

El notebook viene **pre-estructurado** con clases y métodos ya definidos. El alumno debe completar los bloques marcados con `########## Aquí debes incluir tu código ##########`.

### Mapa de Celdas

| Celdas | Contenido |
|--------|-----------|
| 0-3 | Markdown: encabezado UNIR, datos alumno, descripción actividad |
| 4-10 | **Carga del corpus**: clase `Palabra` + lectura de `Corpus-tagged.txt` |
| 11-25 | **Parte 1**: clase `HMMBigrama` + cálculo de probabilidades + exportación Excel |
| 26-40 | **Parte 2**: clase `Viterbi` + matriz Viterbi + decodificación + exportación Excel |
| 41-49 | **Parte 3**: preguntas de análisis (celdas markdown para respuestas) |

### Clase `Palabra` (Celda 5) — COMPLETA, no requiere cambios
- `__init__(token, tag)` → almacena token y etiqueta
- `Token()` → devuelve el token
- `Tag()` → devuelve la etiqueta POS

### Clase `HMMBigrama` (Celda 12) — **6 HUECOS por completar**
Atributos: `_corpus`, `_estados`, `_tokens`, `_q0='q0'`, `_qF='qF'`, `_prob_trans`, `_prob_obs`

**Métodos implementados (no tocar):**
- `_ProcesarCorpus()` → cuenta ocurrencias de estados y tokens
- `Estados(incluir_inicial, incluir_final)` → devuelve dict de estados con conteos
- `Tokens()` → devuelve dict de tokens con conteos

**Métodos con HUECOS:**

1. **`ProbabilidadesDeTransicion()`** — 2 huecos:
   - **Hueco 1:** Dentro del loop `for it in range(0, len(oracion) - 1)` → contar transiciones entre palabras consecutivas de la oración (patrón: estado[it] → estado[it+1])
   - **Hueco 2:** Calcular `prob` = `C(T-1, T) / C(T-1)` usando `contador_transiciones[qt_1][qt]` / `estados_totales[qt_1]`

2. **`ProbabilidadesDeEmision()`** — 2 huecos:
   - **Hueco 1:** Inicializar `contador_observaciones[etiqueta][token] = 0`
   - **Hueco 2:** Calcular `prob` = `C(Ti, Wi) / C(Ti)` usando `contador_observaciones[Ti][Wi]` / `estados[Ti]`

### Clase `Viterbi` (Celda 29) — **3 HUECOS por completar**
Atributos: `_hmmbigrama`, `_oracion`, `_estados_relevantes`, `_prob_viterbi`, `_estado_max_anterior`

**Método `_CalculoEstadosRelevantes()`** — completo: busca en el corpus qué etiquetas tienen las palabras de la oración

**Método `Probabilidades()`** — 2 huecos:
- **Hueco 1:** Calcular `prob_qOrigen` = valor Viterbi anterior del estado origen × probabilidad de transición del origen al destino. Es decir: `matriz_viterbi[qOrigen][token_anterior] * prob_trans[qDestino][qOrigen]`
- **Hueco 2:** Si `prob_qOrigen > prob_max` → actualizar `prob_max` y guardar `qOrigen` en `self._estado_max_anterior[qDestino][token]`
- Luego (ya implementado): `matriz_viterbi[qDestino][token] = prob_max * prob_obs[token][qDestino]`

**Método `DecodificacionSecuenciaOptima()`** — 1 hueco:
- **Hueco 1:** En el loop de backtracking, usar `self._estado_max_anterior` para recuperar la etiqueta del estado anterior que maximizó la probabilidad. Buscar `etiqueta = self._estado_max_anterior[etiqueta_anterior][palabra_anterior]` y agregar a `oracion_etiquetada`

### Flujo de Ejecución
```
1. Cargar corpus → lista de listas de Palabra
2. hmmbigrama = HMMBigrama(corpus)
3. prob_transicion = hmmbigrama.ProbabilidadesDeTransicion()  → exportar .xlsx
4. prob_emision = hmmbigrama.ProbabilidadesDeEmision()        → exportar .xlsx
5. viterbi = Viterbi(hmmbigrama, 'Habla con el enfermo grave de trasplantes .')
6. matriz_viterbi = viterbi.Probabilidades()                  → exportar .xlsx
7. oracion_etiquetada = viterbi.DecodificacionSecuenciaOptima()
8. Imprimir resultado: token / tag
```

### Nota Importante sobre la Oración
La oración se pasa con el **punto separado**: `'Habla con el enfermo grave de trasplantes .'` (espacio antes del punto), y todos los tokens se convierten a **lowercase** con `x.lower()` antes de procesar.

### Archivos Excel de Salida
- `mia07_t3_tra_resultados_trans.xlsx` → probabilidades de transición
- `mia07_t3_tra_resultados_emision.xlsx` → probabilidades de emisión  
- `mia07_t3_tra_resultados_viterbi.xlsx` → matriz de Viterbi