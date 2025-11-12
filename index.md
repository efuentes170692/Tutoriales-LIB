# 🌿 Tutoriales del Curso — Laboratorio de Invasiones Biológicas (LIB)

Bienvenido al sitio de tutoriales del curso.  
# 🌿 Tutoriales LIB

Bienvenido/a a los tutoriales del curso de análisis espacial y modelado ecológico en R.

## 📘 Lista de tutoriales

- [Tutorial 1 – ANOVA entre parcelas invadidas y no invadidas](tutorial_1_ANOVA.md)
- (próximamente: Tutorial 2 – Distancia a caminos)
- (próximamente: Tutorial 3 – Downscaling de variables)
- (próximamente: Tutorial 4 – Modelado de distribución con GLM, GAM y RF)Aquí encontrarás guías paso a paso para realizar análisis en **R** aplicados a ecología y biodiversidad.  
Cada tutorial incluye código, interpretación de resultados y productos finales (gráficos, mapas, etc.).

---

## 📊 Análisis Estadístico

- [**Tutorial 1:** ANOVA entre parcelas invadidas y no invadidas](tutorial_1_ANOVA.html)  
  _Comparación de la temperatura del suelo según el nivel de invasión, incluyendo supuestos, ANOVA, Tukey y barras de error._

- [**Tutorial 2:** ANOVA multifactorial (próximamente)](#)  
  _Evaluación de interacciones entre factores ecológicos._

- [**Tutorial 3:** Regresión lineal simple (próximamente)](#)  
  _Análisis de relaciones lineales entre variables continuas._

---

## 🗺️ Modelado de Distribución de Especies

- [**Tutorial 4:** Modelado GLM, GAM y Random Forest (próximamente)](#)  
  _Uso de variables ambientales, bioclimáticas y topográficas para predecir la distribución potencial de especies._

- [**Tutorial 5:** Proyección espacial y ensamble de modelos (próximamente)](#)  
  _Creación de mapas de idoneidad, comparación de algoritmos y análisis de conectividad._

---

## 📁 Información General

**Formato:** `.md` y `.html`  
**Lenguaje:** R  
**Autor:** Dr. Eduardo Fuentes Lillo  
**Institución:** Laboratorio de Invasiones Biológicas – IEB  
**Año:** 2025  

---

_© 2025 Laboratorio de Invasiones Biológicas (LIB). Todos los derechos reservados._
<script>
document.addEventListener('DOMContentLoaded', function() {
  document.querySelectorAll('pre > code').forEach(function(codeBlock) {
    const button = document.createElement('button');
    button.textContent = 'Copiar';
    button.style = 'float:right; margin:4px; font-size:0.8em;';
    button.addEventListener('click', function() {
      navigator.clipboard.writeText(codeBlock.textContent);
      button.textContent = '¡Copiado!';
      setTimeout(() => button.textContent = 'Copiar', 1500);
    });
    codeBlock.parentNode.insertBefore(button, codeBlock);
  });
});
</script>
