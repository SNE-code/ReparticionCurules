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

3. **Restos mayores**

   * Se reparten curules **restantes** en orden de restos mayores, respetando **máximo 40 por circunscripción**.
   * Si una circunscripción llega a 40, se salta a la siguiente mejor opción del mismo partido.

4. **Tope de sobrerrepresentación (+8%)**

   * Se calcula % curules totales por partido (MR + RP) vs % votos nacionales.
   * Si un partido excede **+8%**, se ajusta reduciendo RP y redistribuyendo sin romper:
     * el tope,
     * el límite de **40 por circunscripción**,
     * y el total de curules RP.

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

1. **Cargar preset** (2024) o editar manualmente los campos de votos por región.
2. **Ingresar MR** por partido.
4. **Ejecutar**: la app mostrará tablas y gráficos de verificación.
5. **Exportar**: puedes añadir un botón para descargar CSVs de resultados.

---

## 👤 Autor

* Jhan Luis Ramírez Suárez para SNE.
* luis.ramirez@cen.pan.org.mx
