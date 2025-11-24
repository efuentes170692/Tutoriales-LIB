---
# **Tutorial 3: Modelos para analizar visitas y riqueza de polinizadores**

**Autor:** *Eduardo Fuentes-Lillo*  
**Fecha:** `r format(Sys.Date(), "%d %B %Y")`  

---
# Tutorial 3: Modelos para analizar visitas y riqueza de polinizadores  
### GLM Poisson → Diagnóstico → Negativa Binomial → ANOVA del Modelo → Visualización  
### Elaborado para estudiantes

Este tutorial explica paso a paso cómo analizar datos de polinizadores usando modelos lineales generalizados (GLMs). Incluye:

- Estructura correcta de los datos  
- GLM Poisson como modelo base  
- Diagnóstico de sobre-dispersión  
- GLM Negativa Binomial cuando corresponde  
- ANOVA para estimar la importancia de cada variable  
- Gráficos de efectos del modelo  

---

# 1. Estructura correcta de los datos

Cada fila debe corresponder a una unidad muestreada (por planta, parcela o cuadrante). Variables necesarias:

| visitas | riqueza | area_basal | humedad | cobertura | temperatura |
|---------|---------|------------|----------|-----------|-------------|
| 12      | 4       | 25.3       | 63.2     | 40        | 19.3        |
| 5       | 2       | 10.1       | 70.5     | 55        | 20.1        |
| 18      | 5       | 32.8       | 58.0     | 35        | 18.8        |
| 0       | 1       | 5.4        | 81.2     | 70        | 21.0        |
| 7       | 3       | 18.6       | 66.3     | 45        | 19.9        |
| 3       | 2       | 12.0       | 72.0     | 60        | 20.4        |
| 15      | 4       | 28.4       | 60.1     | 38        | 19.0        |

```r
datos <- read.csv("mis_datos_polinizadores.csv")
head(datos)
```

---

# 2. GLM Poisson: punto de partida

```r
m_pois_visitas <- glm(
  visitas ~ area_basal + humedad + cobertura + temperatura,
  family = poisson(link = "log"),
  data = datos
)
summary(m_pois_visitas)
```

Para riqueza:

```r
m_pois_riqueza <- glm(
  riqueza ~ area_basal + humedad + cobertura + temperatura,
  family = poisson,
  data = datos
)
```

---

# 3. Diagnóstico de sobre-dispersión

```r
library(performance)

check_overdispersion(m_pois_visitas)
check_overdispersion(m_pois_riqueza)
```

Interpretación:

- ratio ≈ 1 → Poisson válido  
- ratio > 1.5 → sobre-dispersión  
- ratio > 2 → usar Negativa Binomial  

---

# 4. Ajuste de modelo Negativa Binomial (glm.nb)

```r
library(MASS)

m_nb_visitas <- glm.nb(
  visitas ~ area_basal + humedad + cobertura + temperatura,
  data = datos
)
summary(m_nb_visitas)
```

Para riqueza:

```r
m_nb_riqueza <- glm.nb(
  riqueza ~ area_basal + humedad + cobertura + temperatura,
  data = datos
)
```

---

# 5. ANOVA del modelo: importancia de cada variable  
(📌 **Opción B solicitada por el profesor**)

Este análisis compara el efecto de eliminar cada variable del modelo.  
Mide qué predictor explica más variación en visitas o riqueza.

```r
anova(m_nb_visitas, test = "Chisq")
```

El resultado muestra:

- Chi-square grande → la variable explica mucha varianza  
- p-value < 0.05 → variable significativa  

Ejemplo de interpretación:

```
                Df Deviance Resid. Df Resid. Dev  Pr(>Chi)    
area_basal       1   18.32        92     240.1   0.00002 ***
humedad          1    9.41        91     230.7   0.0021  **
cobertura        1    1.11        90     229.6   0.29
temperatura      1    0.36        89     229.2   0.55
```

Interpretación ecológica:

- **Área basal = predictor más importante**
- **Humedad = importante, pero secundaria**
- Cobertura y temperatura = poco efecto

---

# 6. Gráficos de resultados del modelo

## 6.1. Efecto parcial de un predictor (curva del modelo)

```r
library(ggplot2)

ggplot(datos, aes(area_basal, visitas)) +
  geom_point(alpha = 0.5) +
  geom_smooth(
    method = "glm",
    method.args = list(family = "poisson"),
    color = "blue",
    fill = "lightblue"
  ) +
  theme_bw() +
  labs(
    x = "Área basal",
    y = "Número de visitas",
    title = "Relación entre área basal y visitas de polinizadores"
  )
```

## 6.2. Efectos parciales del modelo NegBin

```r
library(ggeffects)

eff <- ggpredict(m_nb_visitas, terms = "area_basal")

ggplot(eff, aes(x, predicted)) +
  geom_line(size = 1.2, color = "darkgreen") +
  geom_ribbon(aes(ymin = conf.low, ymax = conf.high), alpha = 0.2) +
  theme_bw() +
  labs(
    x = "Área basal",
    y = "Visitas predichas",
    title = "Efecto marginal de área basal sobre visitas (NegBin)"
  )
```

---

# 7. Resumen para estudiantes

1. Preparar la tabla con variables bien definidas.  
2. Ajustar un **modelo Poisson** primero.  
3. Evaluar **sobre-dispersión**.  
4. Si existe → ajustar **Negativa Binomial**.  
5. Usar **ANOVA (Chi-square)** para ver qué variable explica más.  
6. Graficar efectos marginales del modelo.  
7. Interpretar ecológicamente los resultados.

Este flujo es el estándar profesional para analizar datos ecológicos de conteo.

---

Fin del tutorial.