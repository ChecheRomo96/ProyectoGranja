# ProyectoGranja

# Conclusiones del proyecto

## 1. Objetivo del proyecto
El objetivo de este proyecto fue desarrollar un modelo de **Machine Learning capaz de predecir la producción diaria de leche (`produccion_kg`)** utilizando información histórica de producción, variables de alimentación, características de la dieta, condiciones climáticas y variables temporales.

Para lograrlo se construyó un pipeline que incluyó:

- limpieza y validación de datos  
- ingeniería de características (feature engineering)  
- control de **data leakage**  
- entrenamiento y evaluación de modelos de regresión  

---

# 2. Importancia de la ingeniería de características

Una parte crítica del proyecto fue la construcción de variables que capturaran dinámicas reales del sistema productivo.

Se incorporaron tres grupos principales de variables.

---

## Historia de producción

Variables como:

- `produccion_roll_mean_3`
- `produccion_roll_std_3`
- `delta_produccion_1d`
- `delta_produccion_3d`

permiten capturar:

- tendencia reciente de producción  
- variabilidad productiva  
- cambios abruptos en la producción  

Estas variables resultaron ser **las más predictivas**, lo cual es consistente con el comportamiento biológico de la producción lechera, que suele presentar **alta autocorrelación temporal**.

---

## Variables de alimentación

Se incluyeron variables relacionadas con el manejo nutricional:

- `consumo`
- `sobrante`
- `rechazo`
- `kg_am`
- `kg_pm`
- `kg_totales`
- `ratio_consumo_oferta`
- `ratio_sobrante_consumo`
- `ratio_sobrante_oferta`

Estas variables reflejan:

- oferta de alimento  
- consumo real  
- eficiencia del sistema de alimentación  

El modelo muestra que la alimentación tiene una **influencia significativa en la producción diaria**, como se espera en sistemas lecheros.

---

## Variables de dieta

También se incorporaron indicadores de ingredientes presentes en la dieta:

- `usa_oro_balance`
- `usa_silo_avena`
- `usa_triticale`
- `n_ingredientes`

Estas variables ayudan a capturar **variaciones en la formulación nutricional**, que pueden impactar directamente la producción.

---

# 3. Variables de salud animal

Se incluyó la variable binaria:

`ubre`

El análisis exploratorio mostró que:

- Producción promedio con `ubre = 0` → **38.27 kg**
- Producción promedio con `ubre = 1` → **34.29 kg**

Esto indica una **reducción aproximada de 4 kg de leche** cuando se registra la condición `ubre = 1`, lo que sugiere que esta variable está asociada con **problemas sanitarios de la ubre**, como mastitis u otras anomalías.

La inclusión de esta variable permite al modelo capturar **factores de salud animal que afectan la producción**.

---

# 4. Variables temporales y estacionales

Se incluyeron variables cíclicas para capturar patrones temporales:

- `mes_sin`
- `mes_cos`
- `dia_semana_sin`
- `dia_semana_cos`
- `dia_del_anio`

Estas variables permiten representar la **estacionalidad de la producción**, evitando discontinuidades artificiales entre meses o días del calendario.

---

# 5. Control de data leakage

Durante el desarrollo del proyecto se identificó y corrigió un problema potencial de **data leakage**, que ocurría cuando algunas variables derivadas de producción podían contener información directa del objetivo.

Para evitar esto se verificó que:

- las variables de tipo rolling usaran **solo información pasada**
- las variables derivadas no permitieran reconstruir directamente el target
- el split entre entrenamiento y prueba fuera **temporal**, no aleatorio

Esto asegura que el modelo evalúe su desempeño en un escenario **realista de predicción futura**.

---

# 6. Evaluación del modelo

El modelo final (Gradient Boosting Regressor) muestra una relación clara entre:

**producción real vs producción predicha**

El gráfico de predicción muestra:

- buena alineación con la diagonal ideal  
- dispersión moderada en valores extremos  
- ausencia de predicciones artificialmente perfectas  

Esto indica que el modelo captura **las principales relaciones del sistema**, sin depender de fuga de información.

---

# 7. Interpretación general

Los resultados sugieren que la producción diaria de leche está principalmente influenciada por:

1. **historia reciente de producción**
2. **manejo nutricional**
3. **estado sanitario de la vaca**
4. **variaciones temporales y estacionales**

Esto coincide con el conocimiento técnico del manejo de sistemas lecheros.

---

# 8. Limitaciones del modelo

Aunque el modelo muestra buen desempeño, existen algunas limitaciones:

- no se incluyen variables fisiológicas clave como  
  - días en lactancia (DIM)  
  - número de parto  
  - etapa de lactación  

- las variables climáticas son limitadas (solo presión atmosférica)

- algunos efectos complejos del manejo nutricional pueden no estar completamente capturados.

---

# 9. Trabajo futuro

El modelo podría mejorarse incorporando:

- variables fisiológicas de la vaca  
- temperatura y humedad para calcular **THI (Temperature-Humidity Index)**  
- modelos específicos por grupo de vacas o etapa de lactación  
- validación temporal más extensa con **TimeSeriesSplit**

---

# 10. Conclusión general

El proyecto demuestra que es posible predecir la producción diaria de leche utilizando un conjunto de variables relacionadas con:

- historial productivo
- alimentación
- composición de la dieta
- condiciones sanitarias
- factores temporales

El modelo final captura de forma efectiva las dinámicas principales del sistema productivo y constituye una base sólida para **aplicaciones de analítica predictiva en sistemas lecheros**.


## Configuración del Entorno Virtual

```bash
# Crear el entorno virtual
python -m venv venv

# Activar el entorno virtual
# En Windows:
venv\Scripts\activate
# En Linux/Mac:
source venv/bin/activate
```

## Instalación de Dependencias

```bash
pip install -r requirements.txt
```

## Estructura del Proyecto

```
ProyectoGranja/
├── venv/
├── src/
├── tests/
├── requirements.txt
└── README.md
```

## Ejecutar la Aplicación

```bash
python -m src.main
```

## Pipeline CI/CD

### 1. Pruebas Unitarias
```bash
pytest tests/ -v
```

### 2. Linting
```bash
flake8 src/
```

### 3. Formateo de Código
```bash
black src/
```

### 4. Type Checking
```bash
mypy src/
```

## Ejecutar Pipeline Completo

```bash
# Instalar herramientas de desarrollo
pip install pytest flake8 black mypy

# Ejecutar todo
pytest && flake8 src/ && black src/ && mypy src/
```

## Requisitos

- Python 3.8+
- pip
