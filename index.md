# 🌿 Tutoriales del Curso — Laboratorio de Invasiones Biológicas (LIB)

Bienvenido al sitio de tutoriales del curso.  

## 📘 Lista de tutoriales

- [Tutorial 1 – ANOVA entre parcelas invadidas y no invadidas](tutorial_1_ANOVA.md)

---

## 📊 Análisis Estadístico

- [**Tutorial 1:** ANOVA entre parcelas invadidas y no invadidas](tutorial_1_ANOVA.html)  
  _Comparación de la temperatura del suelo según el nivel de invasión, incluyendo supuestos, ANOVA, Tukey y barras de error._

- [**Tutorial 2:** ANOVA multifactorial (próximamente)](#)  
  _Evaluación de interacciones entre factores ecológicos._

- [**Tutorial 3:** Regresión lineal simple (próximamente)](#)  
  _Análisis de relaciones lineales entre variables continuas._


---

## 📁 Información General

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
