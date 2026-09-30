# PRIS Rules

## 8 Reglas de Plausibilidad Estructural de Sólidos Inorgánicos

**PRIS** es un protocolo de cribado fisicoquímico utilizado para filtrar estructuras cristalinas hipotéticas antes de su evaluación atomística de mayor costo computacional.

En este trabajo, PRIS se aplica después de la generación de estructuras con **MatterGen** y antes de la relajación y evaluación con **MatterSim**. Su objetivo es descartar configuraciones que, aun siendo numéricamente válidas, presentan inconsistencias geométricas o fisicoquímicas que justifican excluirlas del análisis posterior.

> **Importante:** las reglas PRIS utilizadas aquí constituyen criterios de *screening* definidos para este flujo de trabajo. No deben interpretarse como leyes universales de estabilidad o sintetizabilidad.

---

## PRIS-1 — Repulsión de corto alcance

**Objetivo:** evitar solapamientos atómicos severos e interacciones de corto alcance físicamente inaceptables.

### Criterio

```text
d_ij >= 0.65 * (r_i + r_j)
d_ij >= 0.90 Å
```

donde:

- `d_ij` es la distancia entre dos átomos `i` y `j`;
- `r_i` y `r_j` representan los radios atómicos asociados a ambos sitios.

### Interpretación

Una violación de esta regla indica que dos átomos se encuentran excesivamente próximos, lo que sugiere una fuerte repulsión de corto alcance y una geometría cristalina poco plausible.

---

## PRIS-2 — Colisión de contacto iónico estricto

**Objetivo:** controlar contactos excesivamente cortos entre especies tratadas mediante radios iónicos.

### Criterio

```text
d_ij >= 0.72 * (r_ion,i + r_ion,j)
```

donde:

- `d_ij` es la distancia interatómica;
- `r_ion,i` y `r_ion,j` son los radios iónicos de las especies involucradas.

### Interpretación

La regla penaliza configuraciones en las que dos iones aparecen a una distancia incompatible con sus tamaños iónicos efectivos.

---

## PRIS-3 — Eficiencia de empaquetamiento atómico

**Objetivo:** evitar estructuras excesivamente abiertas o celdas con densidades geométricas anormalmente altas.

### Criterio

```text
0.18 <= eta <= 0.78
```

donde `eta` es la fracción de empaquetamiento atómico de la estructura.

### Interpretación

- `eta < 0.18`: estructura excesivamente abierta o con grandes vacíos.
- `eta > 0.78`: empaquetamiento anormalmente denso para el criterio utilizado.

---

## PRIS-4 — Equilibrio electrostático global

**Objetivo:** comprobar la coherencia química global de la composición propuesta.

### Criterios considerados

- electroneutralidad formal de valencias;
- coherencia entre estados de oxidación plausibles;
- consistencia con diferencias de electronegatividad (`Delta chi`).

### Interpretación

Una estructura puede ser descartada si la combinación de especies y estados formales conduce a un desbalance electrostático o a una asignación química poco plausible.

---

## PRIS-5 — Densidad gravimétrica

**Objetivo:** excluir estructuras con densidades incompatibles con el dominio de sólidos inorgánicos considerado.

### Criterio

```text
1.8 <= rho <= 22.6 g/cm^3
```

donde `rho` es la densidad gravimétrica calculada de la estructura.

### Interpretación

Las estructuras fuera de este intervalo se consideran atípicas para el espacio de materiales evaluado y son descartadas en esta etapa del cribado.

---

## PRIS-6 — Coordinación local tridimensional

**Objetivo:** comprobar que los sitios atómicos se encuentren integrados en una red tridimensional razonablemente conectada.

### Criterio

```text
3 <= CN <= 14
```

para todos los sitios atómicos, donde `CN` corresponde al número de coordinación local.

### Interpretación

La regla busca detectar:

- átomos aislados;
- sitios subcoordinados;
- coordinaciones excesivamente altas;
- conectividades incompatibles con una red cristalina tridimensional plausible.

---

## PRIS-7 — Regularidad de la celda cristalina

**Objetivo:** descartar celdas con geometrías extremas o altamente deformadas.

### Criterios

```text
30° <= alpha, beta, gamma <= 150°
relación axial <= 5.0
```

### Interpretación

La regla limita:

- ángulos de celda excesivamente agudos u obtusos;
- relaciones entre parámetros de red demasiado extremas.

Su objetivo es evitar celdas geométricamente degeneradas o poco razonables para el dominio explorado.

---

## PRIS-8 — Separación entre cationes de alta valencia

**Objetivo:** reducir configuraciones con fuerte repulsión electrostática directa entre cationes altamente cargados.

### Criterio

```text
d_cation-cation >= 2.20 Å
```

para pares de cationes con:

```text
q >= +4
```

### Interpretación

Esta regla se inspira en restricciones electrostáticas asociadas a las reglas 3 y 4 de Pauling y busca evitar proximidades directas entre cationes refractarios de alta valencia sin suficiente separación estructural.

---

# Flujo de cribado

El flujo aplicado en este trabajo puede resumirse como:

```text
MatterGen
1,000 estructuras generadas
        |
        v
PRIS
8 reglas fisicoquímicas
        |
        v
62 estructuras viables
Retención = 6.2 %
        |
        v
MatterSim
Relajación estructural
        |
        +--> E(V) -> K
        |
        +--> Convex hull -> E_hull
        |
        v
29 candidatos con K >= 250 GPa
        |
        v
Ranking Top 10
```

---

# Interpretación de los descartes

En la ejecución reportada para el póster:

- **PRIS-1:** 533 casos — 56.8 %
- **PRIS-6:** 220 casos — 23.5 %
- **PRIS-3:** 136 casos — 14.5 %
- **PRIS-8:** 26 casos — 2.8 %
- **PRIS-4 + PRIS-5:** 23 casos — 2.4 %

En conjunto, PRIS-1, PRIS-6 y PRIS-3 explicaron aproximadamente el **94.8 %** de los descartes registrados.

---

# Alcance y limitaciones

PRIS es una etapa de **cribado previo**. Superar sus reglas no implica por sí solo:

- estabilidad termodinámica definitiva;
- sintetizabilidad experimental;
- estabilidad dinámica;
- estabilidad mecánica completa;
- superdureza;
- existencia experimental del compuesto.

Las estructuras que superan PRIS deben ser sometidas a etapas posteriores de validación, tales como:

1. relajación estructural;
2. evaluación energética de mayor fidelidad;
3. cálculo del tensor elástico;
4. estimación de dureza y tenacidad;
5. análisis de estabilidad dinámica;
6. evaluación de rutas de síntesis;
7. validación experimental.

---

# Uso en este repositorio

Las estructuras almacenadas en `structures/` corresponden a candidatos que superaron el cribado PRIS utilizado en este estudio.

Los valores de propiedades y rankings asociados deben interpretarse dentro del contexto del flujo computacional específico empleado en este trabajo.

---

## Trabajo asociado

**Acoplamiento de reglas PRIS y aprendizaje automático para el descubrimiento acelerado de materiales superduros**

Gabriel Epuyao Barrientos  
Doctorado en Ingeniería  
Facultad de Ingeniería y Ciencias  
Universidad de La Frontera, Chile
