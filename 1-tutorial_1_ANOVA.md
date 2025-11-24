---
# **Tutorial 1: ANOVA — Comparación de temperatura del suelo entre parcelas invadidas y no invadidas**

**Autor:** *Eduardo Fuentes-Lillo*  
**Fecha:** `r format(Sys.Date(), "%d %B %Y")`  

---

# **Objetivo**

Aplicar un **Análisis de Varianza (ANOVA)** para evaluar si la **temperatura del suelo** difiere significativamente entre **parcelas invadidas y no invadidas** en función de la **biomasa de pino**.

Este tutorial incluye la verificación de supuestos de normalidad y homogeneidad, la comparación de medias mediante ANOVA y la visualización de resultados con barras de error.

---

## **1. Cargar datos desde Excel**

Se asume que el archivo contiene al menos las siguientes columnas:

- `Invasion`: con valores *Invadida* o *No invadida*.
- `Biomasa`: valores numéricos de biomasa de pino por parcela.
- `Temp_Suelo`: valores de temperatura promedio del suelo (°C).

Ejemplo de estructura:

| Invasion    | Biomasa | Temp_Suelo |
|--------------|----------|-------------|
| Invadida     | 320      | 21.5        |
| No invadida  | 180      | 17.2        |

```r
library(readxl)

# Cargar datos desde Excel
Datos <- read_excel("Datos_Temperatura_Suelo.xlsx")

# Revisar estructura
str(Datos)
head(Datos)
```

---

## **2. Exploración inicial de los datos**

```r
library(dplyr)

Datos %>% group_by(Invasion) %>% summarise(
  Media_Temp = mean(Temp_Suelo, na.rm = TRUE),
  SD_Temp = sd(Temp_Suelo, na.rm = TRUE),
  n = n()
)
```

---

## **3. Verificación de supuestos**

### **3.1 Normalidad (Shapiro-Wilk)**

```r
by(Datos$Temp_Suelo, Datos$Invasion, shapiro.test)
```
> Si p > 0.05 → los datos no difieren de una distribución normal.

### **3.2 Homogeneidad de varianzas (Levene)**

```r
library(car)
leveneTest(Temp_Suelo ~ Invasion, data = Datos)
```
> Si p > 0.05 → las varianzas son homogéneas.

---

## **4. ANOVA de un factor (Invasión)**

Evaluamos si la temperatura del suelo difiere significativamente entre parcelas invadidas y no invadidas.

```r
anova_mod <- aov(Temp_Suelo ~ Invasion, data = Datos)
summary(anova_mod)
```

> Si el valor p < 0.05 indica diferencias significativas entre ambos tipos de parcelas.

---

## **5. Comparación post-hoc (Tukey HSD)**

```r
TukeyHSD(anova_mod)
```

> Si bien hay solo dos grupos, esta prueba confirma la dirección y magnitud de la diferencia.

---

## **6. Gráfico de medias con barras de error**

```r
library(ggplot2)
library(Rmisc)

# Estadísticas resumidas
resumen <- summarySE(Datos, measurevar = "Temp_Suelo", groupvars = c("Invasion"))

# Gráfico

ggplot(resumen, aes(x = Invasion, y = Temp_Suelo, fill = Invasion)) +
  geom_bar(stat = "identity", color = "black", width = 0.6) +
  geom_errorbar(aes(ymin = Temp_Suelo - se, ymax = Temp_Suelo + se), width = 0.2) +
  scale_fill_manual(values = c("#56B4E9", "#E69F00")) +
  labs(
    title = "Temperatura del suelo en parcelas invadidas y no invadidas",
    x = "Tipo de parcela",
    y = "Temperatura del suelo (°C)",
    fill = "Invasión"
  ) +
  theme_minimal(base_size = 13)
```

---

## **7. Interpretación de resultados**

- **Supuestos:** si ambas pruebas (Shapiro y Levene) son no significativas, el ANOVA es válido.  
- **ANOVA:** si p < 0.05 → existen diferencias en temperatura del suelo entre parcelas invadidas y no invadidas.  
- **Gráfico:** muestra las medias y errores estándar, facilitando la interpretación visual.  

---

## **8. Conclusión esperada**

En este análisis se espera observar que las **parcelas invadidas presentan temperaturas de suelo más altas** en comparación con las no invadidas, lo que puede estar asociado a cambios microclimáticos generados por la invasión.