# Marco Ético y Lenguaje Responsable de MoodWave - Etapa 2

## 1. Principios Fundamentales
1. **Sin Diagnósticos**: MoodWave es un diario de registro personal y bienestar emocional. No es un dispositivo médico ni reemplaza la atención profesional.
2. **Sin Lenguaje Causal**: No se afirma que una actividad o hábito "cause" o "provoque" un estado de ánimo. Se comunica honestamente como una *asociación observada en los datos*.
3. **Procesamiento 100% Local**: Todo el procesamiento analítico se ejecuta en el dispositivo del usuario. Ninguna información emocional se envía a servidores ni a la nube.
4. **Transparencia Muestral**: Se muestra el número de registros ($n$) en cada afirmación.

---

## 2. Guardián de Términos Prohibidos (`ProhibitedTermsGuard`)
Todas las frases generadas automáticamente son validadas por un filtro estricto antes de mostrarse en pantalla.

### Lista de Términos Prohibidos
- **Causalidad**: `"causa"`, `"provoca"`, `"hace que"`, `"debido a"`, `"gracias a"`.
- **Imperativos o Prescripciones**: `"deberías"`, `"debes hacer"`, `"te deprime"`.
- **Términos Clínicos / Diagnósticos**: `"depresión"`, `"ansiedad clínica"`, `"trastorno"`, `"diagnóstico"`, `"patología"`, `"tratamiento médico"`.

---

## 3. Autenticación y Privacidad
1. **Qué se guarda**: Nombre de perfil local, hash de contraseña derivado con sal aleatoria y credenciales de cifrado. Los diarios y registros emocionales se almacenan en la base de datos local cifrada de Room.
2. **Dónde se guarda**: En el almacenamiento interno protegido del dispositivo, gestionado mediante `EncryptedSharedPreferences` (respaldado por Android Keystore TEE/StrongBox).
3. **Cómo se protege**: Las contraseñas o PINs **nunca se guardan en texto plano**. Se utiliza el algoritmo estándar de la industria **PBKDF2WithHmacSHA256** con 310,000 iteraciones y sal aleatoria de 16 bytes, realizando comparaciones en tiempo constante contra ataques de timing.
4. **Límites de Protección**: La seguridad descansa en la integridad del sistema operativo del dispositivo. En dispositivos con acceso *root* comprometido o sin bloqueo general de pantalla activo en el sistema, la protección local puede verse reducida.

---

## 4. Detección Local de Señales de Riesgo (`RiskSignalDetector`)

### Diseño del Detector
El detector analiza localmente los registros en busca de dos escenarios:
1. **Expresiones de Desesperanza en Notas**: Búsqueda de frases clave en español mexicano (ej. *"ya no quiero vivir"*, *"sin salida"*, *"autolesion"*), normalizando acentos, puntuación y mayúsculas.
2. **Malestar Sostenido**: Registro contiguo de ánimo bajo ($\le 3$ puntos) durante $k \ge 3$ días consecutivos.

### Límites y Manejo de Falsos Positivos / Negativos
- **Falsos Positivos**: El usuario podría escribir una nota reflexiva usando una frase de la lista en tono literario o hipotético. Para evitar alarmismo innecesario, la alerta se presenta de forma cálida, privada y respetuosa, sin bloquear la aplicación.
- **Falsos Negativos**: El análisis de lenguaje es heurístico y local. No sustituye la auto-evaluación consciente del usuario ni la consulta profesional.

### Recursos de Apoyo
Ante cualquier señal de riesgo detectada, la aplicación ofrece accesos directos a líneas de apoyo telefónico gratuitas y confidenciales (ej. *Línea de la Vida en México: 800 911 2000*).
