# Metodología Estadística de MoodWave - Etapa 2: "Descubre tus patrones"

## 1. Naturaleza Ordinal de la Escala Emocional
El estado de ánimo se registra en una escala ordinal de 8 puntos (desde *1: Enojado* hasta *8: Radiante*). 
- **Expresión de Cambios**: Todas las variaciones se expresan estrictamente en **puntos** ($\Delta$ puntos), nunca en porcentajes.
- **Justificación**: Decir "tu ánimo mejoró un 20%" en una escala ordinal sin origen cero absoluto carece de validez matemática. Expresar "se observa un incremento de +1.2 puntos" es métricamente exacto y comprensible.

---

## 2. Comparación Intra-Sujeto ($\bar{x}_{con} - \bar{x}_{sin}$)
Para cada actividad $A$, se compara el promedio de ánimo de los días **CON** la actividad ($\bar{x}_{con}$) contra el promedio de los días **SIN** la actividad ($\bar{x}_{sin}$).
- **Por qué no usar la media global**: Comparar días activos contra la media global incluye el propio grupo activo dentro de la media global, diluyendo la diferencia e ignorando tendencias temporales. La comparación intra-sujeto ($\bar{x}_{con} - \bar{x}_{sin}$) aísla el comportamiento específico del usuario.

---

## 3. Medición del Tamaño del Efecto: Delta de Cliff ($d$)
Dado que la escala de ánimo es ordinal y no asume distribución normal:
- Se utiliza el **Delta de Cliff** ($d$), una medida no paramétrica que cuantifica la frecuencia con la que las puntuaciones del grupo con la actividad superan a las del grupo sin la actividad.
- **Umbrales Clasificados**:
  - $|d| < 0.147$: Insignificante
  - $0.147 \le |d| < 0.33$: Pequeño
  - $0.33 \le |d| < 0.474$: Moderado
  - $|d| \ge 0.474$: Relevante

---

## 4. Intervalos de Confianza por Bootstrap
Para estimar la incertidumbre del cambio medio sin asumir una distribución gaussiana:
- Se ejecuta un **Bootstrap de 1,000 iteraciones** con reemplazo utilizando el método de percentiles ($2.5\%$ a $97.5\%$).
- Para garantizar reproducibilidad determinista en las pruebas unitarias, se utiliza una semilla fija (`Random(42L)`).

---

## 5. Correlación de Rangos de Spearman ($\rho$)
Para variables ordinales/continuas (Nivel de Energía vs Ánimo, Horas de Sueño vs Ánimo, Pasos vs Ánimo), se aplica el coeficiente de **Spearman**, resistente a valores atípicos y no dependiente de la linealidad estricta.

---

## 6. Corrección Múltiple: Benjamini-Hochberg (FDR) vs Bonferroni
Al evaluar múltiples actividades simultáneamente (ej. 15 actividades), existe el riesgo de detectar asociaciones por mero azar (error Tipo I).
- **Por qué NO usar Bonferroni**: Ajustar por Bonferroni ($\alpha / m$) es excesivamente conservador. Asume independencia entre variables y genera una elevadísima tasa de falsos negativos (error Tipo II), ocultando patrones cotidianos reales del usuario.
- **Elección de Benjamini-Hochberg**: Controla la Tasa de Falsos Descubrimientos (FDR a $q = 0.05$). Mantiene bajo control los descubrimientos falsos sin destruir el poder estadístico del análisis.

---

## 7. Transparencia en el Tamaño de Muestra ($n$)
Todos los resultados comunicados en la interfaz de usuario **deben incluir explícitamente el tamaño de muestra $n$** (ej. $n = 23$ días). Ningún patrón se muestra si el número de observaciones es inferior al umbral definido por la `DataSufficiencyPolicy` ($n < 5$ registros por actividad o $n < 14$ días totales).
