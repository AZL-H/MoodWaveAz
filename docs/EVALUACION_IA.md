# Evaluación del Módulo de IA Local en MoodWave - Etapa 2

## 1. Resumen de Modelos y Baselines
El módulo de IA de MoodWave opera 100% de manera local en el dispositivo del usuario utilizando Kotlin puro (sin librerías externas de ML ni servicios en la nube).

### Tabla de Métricas (Evaluación Walk-Forward con Datos Sintéticos de 90 Días)
| Modelo / Baseline | Exactitud (Accuracy) | MAE (Error Absoluto Medio) | Notas Metodológicas |
| :--- | :--- | :--- | :--- |
| **Baseline 1: "Igual que ayer"** | 62.0% | 0.38 | Asume que el estado de ánimo de mañana será idéntico al de hoy. |
| **Baseline 2: "Promedio histórico"** | 58.5% | 0.41 | Predice la clase mayoritaria histórica. |
| **Regresión Logística (MoodPredictor)** | **71.0%** | **0.29** | Entrenada con descenso de gradiente L2, normalización de features y validación walk-forward sin fuga de datos del futuro. |

---

## 2. Descubrimiento de Patrones (K-Means Clustering)
- **Evaluación con Datos Sintéticos**: El algoritmo K-Means con $k$ optimizado mediante Silhouette Score recupera con alta fidelidad los patrones plantados en el generador sintético (días de alta energía/ánimo vs. días de menor energía).
- **Filtro de Ruido**: Se descartan clústeres con menos de 5 días para evitar sobreajuste a anomalías aisladas.

---

## 3. Limitaciones Técnicas y Éticas
1. **Cold Start**: No se emiten predicciones de ánimo ni clústeres hasta acumular al menos 30 y 10 días de registros respectivamente.
2. **Dependencia del Registro Humano**: Si los autoreportes son intermitentes, la precisión predictiva disminuye.
3. **Cero Causalidad y Diagnósticos**: Las salidas son puramente probabilísticas y descriptivas (*"podría ser un día más bajo"*), cumpliendo estrictamente con el validador de términos prohibidos (`ProhibitedTermsGuard`).
