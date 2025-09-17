# ReparticionCurules

# Calculadora de Diputaciones por Representación Proporcional (MX)

Aplicación (alojada en Streamlit) para asignar curules de **Representación Proporcional (RP)** por circunscripción con base en votos por partido, considerando reglas comunes del sistema mexicano: **umbral nacional (3%)**, **cociente natural**, **tope de sobrerrepresentación (+8%)**, y límites por circunscripción (**40 curules**).

> Pensada para explorar escenarios y generar tablas de asignación por partido y circunscripción.

---

## ✨ Características

* **Entrada**: votos por partido y **5 circunscripciones**; curules de **Mayoría Relativa (MR)** por partido.
* **Método de asignación**: **cociente natural + restos mayores** para 200 curules RP (modificable).
* **Umbral nacional**: filtra partidos con **≥ 3%** del voto válido nacional.
* **Tope de sobrerrepresentación**: limita la diferencia entre % curules y % votos a **+8%**.
* **Capacidad por región**: máximo **40** curules de RP por circunscripción.
* **Reportes**:
* **Presets** de datos (2024) y carga/edición en la UI.

---

## 📦 Requisitos

* Python 3.10+
* Paquetes:
  * `streamlit`
  * `pandas`
  * `numpy`
  * (Opcional) `matplotlib` para gráficos

Instalación rápida:

```bash
pip install streamlit pandas numpy matplotlib
```

---

## ▶️ Cómo ejecutar

```bash
streamlit run app.py
```
Puedes cambiar el nombre al archivo Python, en ese caso, modificalo la orden de ejecución.

La app abrirá en tu navegador (por defecto `http://localhost:8501`).

---

## 🧩 Parámetros de entrada

1. **Votos por circunscripción** (`votos_region`):

```python
{
  "PRIMERA": {"PAN": ..., "PRI": ..., "PT": ..., "PVEM": ..., "MC": ..., "MORENA": ...},
  "SEGUNDA": {...},
  "TERCERA": {...},
  "CUARTA":  {...},
  "QUINTA":  {...}
}
```

2. **Distritos de MR ganados** (`distritos_mr`):

```python
{"PAN": ..., "PRI": ..., "PT": ..., "PVEM": ..., "MC": ..., "MORENA": ...}
```

3. **Parámetros globales** (configurables en la UI o vía `Config`):

* `regiones = ["PRIMERA","SEGUNDA","TERCERA","CUARTA","QUINTA"]`
* `asientos_por_region = 40` (RP)
* `total_camara = 500` (300 MR + 200 RP)
* `umbral_nacional = 0.03`
* `aplicar_tope_sobrerrep = True`
* `margen_tope = 0.08`

---

## 🧮 Resumen del algoritmo

1. **Umbral (3%)**

   * Se suman votos nacionales por partido.
   * Se filtran partidos con ≥ 3% de los votos válidos.

2. **Cociente natural**

   * Para RP totales:

     $$
     Q = \frac{\text{votos válidos de partidos con derecho}}{Curules RP}
     $$
   * Asignación inicial por circunscripción y partido (si implementado por región) o a nivel nacional y luego distribución:

     $$
     \text{curules} = \left\lfloor \frac{\text{votos}}{Q} \right\rfloor
     $$

3. **Restos mayores**

   * Se calculan **restos** = votos − curules·Q.
   * Se reparten curules **restantes** en orden de restos, respetando **máximo 40 por circunscripción**.
   * Si una circunscripción llega a 40, se salta a la siguiente mejor opción del mismo partido.

4. **Tope de sobrerrepresentación (+8%)** (si activo)

   * Se calcula % curules totales por partido (MR + RP) vs % votos nacionales.
   * Si un partido excede **+8%**, se ajusta reduciendo RP y redistribuyendo sin romper:

     * el tope,
     * el límite de **40 por circunscripción**,
     * y el total de 200 RP.

---

## 🧾 Salidas esperadas

* **Tabla RP por circunscripción**: matriz regiones × partidos.
* **Totales por partido**:

  * `RP_total_partido`, `MR_total_partido`, `Total_cámara`.
* **Diagnóstico**:

  * Cociente natural, restos, orden de asignación de los últimos asientos.
  * Indicador de cumplimiento de tope por partido.

---

## 🧰 Uso en la interfaz

1. **Cargar preset** (2018/2021/2024) o editar manualmente los campos de votos por región.
2. **Ingresar MR** por partido.
3. (Opcional) Activar/Desactivar tope de sobrerrepresentación.
4. **Ejecutar**: la app mostrará tablas y (si habilitado) gráficos de verificación.
5. **Exportar**: puedes añadir un botón para descargar CSVs de resultados (opcional).

---

## ✅ Reglas de negocio verificadas

* Umbral nacional del **3%** aplicado antes del reparto.
* **200** curules RP en total.
* **Máximo 40** RP por circunscripción (se salta cuando se alcanza).
* **Tope +8%** (si se enciende la opción).
* Sumatoria **MR + RP = 500** curules (consistencia global).

---

## 🧪 Pruebas rápidas

* Caso base con partidos que superan el 3% y distribución pareja.
* Caso con un partido sobrerrepresentado para verificar el tope.
* Caso límite donde varias circunscripciones alcanzan 40 RP.

> Puedes integrar pruebas con `pytest` para la lógica pura (cálculo de Q, restos, asignación incremental, tope).

---

## 🔧 Notas de implementación

* El algoritmo de reparto y el tope están desacoplados de la UI (si separaste en `core/`), lo que permite:

  * pruebas unitarias,
  * reutilización en otros frontends,
  * y escenarios con datos reales (CSV).

* Si vas a cargar **CSV** de votos:

  * Limpia encabezados, normaliza nombres de partidos/regiones,
  * verifica totales y NaNs antes del reparto.

---

## 📄 Licencia

MIT (o la que prefieras).

---

## 👤 Autor

* Jhan Luis Ramírez Suárez
* (Agrega correo o sitio si quieres)

---

¿Quieres que lo adapte a la estructura exacta de tu repo (nombres de archivos/clases) o que añada un botón de **descarga de resultados** en CSV desde la app?
