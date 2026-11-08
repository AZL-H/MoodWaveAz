# MoodWaveAz
Proyecto educativo app que analiza los datos del usuario, descubre patrones  y los comunica de forma honesta: sin diagnosticar y sin confundir correlación con causalidad.

MoodWave no realiza diagnósticos médicos ni sustituye atención profesional.


# MoodWave 🌊 - Diario Emocional y Descubrimiento de Patrones

MoodWave es una aplicación nativa para Android desarrollada con **Kotlin, Jetpack Compose, Room (SQLite), Navigation Compose, DataStore y BiometricPrompt**. Su propósito es ofrecer un espacio íntimo, privado y científicamente transparente para el registro diario del estado de ánimo y el descubrimiento autónomo de patrones de bienestar.

---

## 🏛️ Arquitectura del Sistema
El proyecto sigue estrictamente el patrón **MVVM + Clean Architecture**:
```
UI (Jetpack Compose) ──> ViewModel (StateFlow) ──> Casos de Uso (domain/usecase) ──> Repositorios (data/repository) ──> Room DB (SQLite)
```
- **Motor Estadístico Puro**: Todo el análisis estadístico reside en el paquete `com.example.moodwave.analytics` como código Kotlin puro (sin dependencias de `android.*`), permitiendo pruebas unitarias instantáneas en la JVM (`src/test`).
- **Ejecución en Segundo Plano**: Los cálculos pesados se ejecutan fuera del hilo principal (`Dispatchers.Default` / `IO`) y se cachean en `StateFlow` para evitar re-cálculos en recomposiciones de UI.

---

## 🔬 Decisiones Técnicas
1. **Método Estadístico**:
   - Escala ordinal de ánimo (1-8), expresando variaciones en puntos ($\Delta$ pts) y nunca en porcentajes.
   - Comparación intra-sujeto ($\bar{x}_{con} - \bar{x}_{sin}$) para aislar el impacto de las actividades.
   - **Delta de Cliff ($d$)** como medida no paramétrica de tamaño de efecto ordinal.
   - **Bootstrap (95% CI)** con 1,000 iteraciones y semilla fija para intervalos de confianza deterministas.
   - **Benjamini-Hochberg (FDR $q=0.05$)** para corregir comparaciones múltiples, evitando la alta tasa de falsos negativos de Bonferroni.
2. **Migración de Base de Datos**:
   - Migración explícita `MIGRATION_1_2` con sentencias SQL seguras (`ALTER TABLE` y `CREATE TABLE IF NOT EXISTS`) que garantizan la preservación del 100% de los datos de la Etapa 1.
3. **Cifrado y Respaldo**:
   - Cifrado robusto con **PBKDF2WithHmacSHA256** (310,000 iteraciones, salt aleatorio) + **AES-256-GCM** (IV aleatorio) en formato binario estructurado, con importación atómica en transacciones de Room.
4. **Sincronización y Recordatorios**:
   - **WorkManager** para persistencia en segundo plano (sobrevive a reinicios, respeta Doze mode). Estrategia de resolución de conflictos *Last-Write-Wins* basada en `updatedAt`.

---

## 🌸 Cómo Cargar Datos de Prueba
1. Abre la aplicación (en configuración `debug`).
2. Toca el ícono de engranaje **Configuración ⚙️** en la barra superior de la pantalla principal (Home).
3. En la sección **Datos de Prueba Sintéticos**, presiona el botón **"Cargar Datos"** para inyectar 90 días de registros con patrones plantados conocidos.
4. Explora las secciones de *Tendencias*, *Actividades*, *Insights* y *Reportes PDF*.

---

## 📚 Enlaces a Documentación Técnica
- [Metodología Estadística](docs/METODOLOGIA.md)
- [Ética y Lenguaje Responsable](docs/ETICA.md)
- [Evaluación del Módulo de IA](docs/EVALUACION_IA.md)
- [Guion de Video (3 Minutos)](docs/GUION_VIDEO_IA.md)
- [Seguimiento de Progreso](docs/PROGRESO.md)

---

## 📸 Lista de Capturas de Pantalla por Tomar
1. **Pantalla Principal (Home)** con saludo dinámico, widget semanal y tarjeta de racha (Tema Verde Naturaleza).
2. **Pantalla de Registro** (Stepper vertical con cuadrícula 4x2 de ánimos y selector de energía).
3. **Pantalla de Análisis y Patrones** (Resumen general con $n$ y tarjetas de insights destacados).
4. **Pantalla de Tendencias** (Gráfica de media móvil con eje Y de 1 a 8 y mapa de calor semanal).
5. **Modal "¿Por qué veo esto?"** (Desglose científico de confianza, $n$, fechas y tamaño de efecto).
6. **Alerta de Señal de Riesgo** (Mensaje empático con contacto a la Línea de la Vida).
7. **Configuración de Privacidad y Respaldo** (Exportación JSON y opciones de cifrado).
