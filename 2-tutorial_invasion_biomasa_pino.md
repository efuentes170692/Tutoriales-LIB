---
# **Tutorial 2: Efecto de la invasión de biomasa de pino sobre riqueza nativa y visitación de aves **

**Autor:** *Eduardo Fuentes-Lillo*  
**Fecha:** `r format(Sys.Date(), "%d %B %Y")`  

---
# Tutorial 2: Efecto de la invasión de biomasa de pino sobre riqueza nativa y visitación de aves  
### GLM Poisson → Diagnóstico → Negativa Binomial → ANOVA → Gráficos  
### Elaborado para estudiantes

Este tutorial enseña cómo evaluar el impacto de la **biomasa de pino** (invasión) sobre:

- **la riqueza de especies nativas**, y  
- **la visitación de aves**,  

utilizando modelos estadísticos adecuados según el tipo de variable respuesta.

---

# 1. Estructura correcta de los datos

Cada fila representa un sitio/ parcela con distinto nivel de invasión.

| biomasa_pino | riqueza_nativas | visitas_aves | humedad | cobertura | temperatura |
|--------------|-----------------|--------------|---------|-----------|-------------|
| 12.3         | 18              | 4            | 63.2    | 40        | 19.3        |
| 35.1         | 10              | 1            | 70.5    | 55        | 20.1        |
| 50.2         | 7               | 0            | 81.0    | 65        | 19.0        |
| 5.4          | 22              | 8            | 58.1    | 30        | 18.9        |
| 20.8         | 14              | 3            | 67.0    | 45        | 19.7        |

```r
datos <- read.csv("biomasa_polinizadores_aves.csv")
head(datos)
```

---

# 2. ¿LM o GLM? Selección del modelo correcto

Depende del tipo de variable:

- **Riqueza nativa** → conteo → **GLM Poisson o NegBin**  
- **Visitas de aves** → conteo → **GLM Poisson o NegBin**  
- **Biomasa de pino** → predictor continuo

---

# 3. Modelo 1: Biomasa de pino → Riqueza de nativas  
## 3.1 GLM Poisson inicial

```r
m_pois_riqueza <- glm(
  riqueza_nativas ~ biomasa_pino + humedad + cobertura + temperatura,
  family = poisson,
  data = datos
)

summary(m_pois_riqueza)
```

---

## 3.2 Diagnóstico de sobre-dispersión

```r
library(performance)
check_overdispersion(m_pois_riqueza)
```

Interpretación:

- ratio ≈ 1 → Poisson válido  
- ratio > 1.5 → sobre-dispersión  
- ratio > 2 → usar **Negativa Binomial**  

---

## 3.3 GLM Negativa Binomial si hay sobre-dispersión

```r
library(MASS)

m_nb_riqueza <- glm.nb(
  riqueza_nativas ~ biomasa_pino + humedad + cobertura + temperatura,
  data = datos
)

summary(m_nb_riqueza)
```

---

# 4. Modelo 2: Biomasa de pino → Visitación de aves

## 4.1 GLM Poisson

```r
m_pois_aves <- glm(
  visitas_aves ~ biomasa_pino + humedad + cobertura + temperatura,
  family = poisson,
  data = datos
)
```

## 4.2 Diagnóstico

```r
check_overdispersion(m_pois_aves)
```

## 4.3 GLM Negativa Binomial si corresponde

```r
m_nb_aves <- glm.nb(
  visitas_aves ~ biomasa_pino + humedad + cobertura + temperatura,
  data = datos
)
```

---

# 5. ANOVA del modelo: importancia de cada variable

```r
anova(m_nb_riqueza, test = "Chisq")
anova(m_nb_aves, test = "Chisq")
```

Interpretación:

- **Chi-square alto** → variable importante  
- **p < 0.05** → efecto significativo  

Ejemplo:

```
biomasa_pino   ***  
humedad         **  
cobertura        -  
temperatura      -  
```

---

# 6. Interpretación ecológica esperada

- Más **biomasa de pino → menos riqueza nativa**  
- Más **biomasa de pino → menos visitas de aves**  
- Humedad y cobertura pueden moderar los efectos  
- Temperatura usualmente tiene efecto secundario

---

# 7. Gráficos del modelo

## 7.1 Efecto de biomasa sobre riqueza nativa

```r
library(ggeffects)

eff_riqueza <- ggpredict(m_nb_riqueza, terms = "biomasa_pino")

ggplot(eff_riqueza, aes(x, predicted)) +
  geom_line(size = 1.2, color = "darkgreen") +
  geom_ribbon(aes(ymin = conf.low, ymax = conf.high), alpha = 0.2) +
  theme_bw() +
  labs(
    x = "Biomasa de pino",
    y = "Riqueza nativa predicha",
    title = "Efecto de la invasión de pino sobre la riqueza nativa"
  )
```

## 7.2 Efecto de biomasa sobre visitación de aves

```r
eff_aves <- ggpredict(m_nb_aves, terms = "biomasa_pino")

ggplot(eff_aves, aes(x, predicted)) +
  geom_line(size = 1.2, color = "darkblue") +
  geom_ribbon(aes(ymin = conf.low, ymax = conf.high), alpha = 0.2) +
  theme_bw() +
  labs(
    x = "Biomasa de pino",
    y = "Visitas de aves predichas",
    title = "Efecto de la invasión de pino sobre la visita de aves"
  )
```

---

# 8. Resumen final

1. Probar primero con **GLM Poisson**  
2. Evaluar sobre-dispersión  
3. Si existe → usar **GLM Negativa Binomial**  
4. Evaluar importancia de variables con **ANOVA Chi-square**  
5. Graficar efectos marginales  
6. Interpretar resultados ecológicamente  

Fin del tutorial.

