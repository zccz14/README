# Informe de Análisis de Estilo Objetivo y Neutral

**Fecha de análisis por IA**: 3 de febrero de 2026
**Nota**: Este informe ha sido generado por IA y su contenido es solo para referencia.

---

## Resumen

El propietario de este repositorio es CZ (ID de GitHub: zccz14), cuyo ámbito profesional es el trading cuantitativo y la tecnología financiera (FinTech). CZ es el fundador de la organización No Trade No Life (NTNL), su lengua materna es el chino, puede comunicarse en inglés y japonés, y está aprendiendo español.

El contenido del repositorio abarca un período desde el 5 de enero de 2026 hasta el 3 de febrero de 2026, aproximadamente un mes. El repositorio contiene principalmente los siguientes proyectos:

- **CZON**: Una herramienta AI-Native para creadores de contenido, que admite escritura en lengua materna, traducción automática y distribución multiplataforma.
- **EA (Earnby.AI)**: Proyecto de fondo de prioridad desplegado en la cadena BSC, que ofrece servicios de gestión de patrimonio con protección de capital en stablecoins.
- **Guerra Prolongada del Capital**: Marco estratégico para el crecimiento de la riqueza de inversores individuales.
- **LegionMind**: Marco de gestión y colaboración de proyectos basado en Agentes de IA (discusiones relacionadas).

---

## Introducción a los Proyectos

### CZON

- **Origen**: Proyecto iniciado y desarrollado por CZ, originalmente llamado ZEN y posteriormente renombrado a CZON. La C representa Content Oriented (el contenido es el rey), la Z representa Zero Configuration (configuración cero), la O representa Organic AI-Native (AI-Native orgánico) y la N representa N-shaped Content Creation and Distribution Energy Curve (Curva de energía en forma de N para la creación y distribución de contenido).
- **Funcionalidades principales**:
  - Escritura de contenido en lengua materna, con traducción automática a múltiples idiomas.
  - Generación automática de enlaces permanentes por IA.
  - Extracción automática de resúmenes y metadatos por IA.
  - Soporte para temas JSX personalizados.
  - Generación automática de contenido optimizado para SEO.
  - Soporte para modo oscuro.
  - Generación de mapas del sitio y reglas para rastreadores web (robots.txt).
  - Soporte para referencias a recursos estáticos.
- **Iteraciones de versión**:
  - 0.4.2: Se añadió la generación de mapa del sitio y robots.txt.
  - 0.4.3: Se añadió soporte para modo oscuro.
  - 0.5.0-0.5.2: Integración de OpenCode para tareas de traducción, posteriormente revertida por problemas de rendimiento.
  - 0.6.0: Refactorización de la estructura de directorios, usando rutas en lugar de hashes como identificadores de archivo.
  - 0.6.3: Soporte para extracción automática de TOC y funcionalidad de anclas para títulos.
- **Archivos relacionados**: [De la creación a la distribución: construyendo un motor de contenido AI-Native](../INSIGHTS/4.md), [Interpretación del significado de la N en CZON](../LOGS/11.md), [Interpretación completa del significado de CZON](../LOGS/12.md)

### EA (Earnby.AI)

- **Origen**: El proyecto EA se lanzó en noviembre de 2025, siendo el sucesor de un fondo privado en funcionamiento desde marzo de 2024. El proyecto está desplegado en BSC (BNB Smart Chain), utilizando USDC como moneda de liquidación.
- **Funcionalidades principales**:
  - Gestión de patrimonio con protección de capital en stablecoins; el capital prioritario disfruta de protección del principal.
  - Rendimiento variable, actualmente del 12% anualizado.
  - Rescate disponible en cualquier momento; rescate ordinario desbloqueado tras 168 horas, rescate instantáneo con comisión del 0.5% y disponibilidad inmediata.
- **Introducción a las estrategias**:
  - Estrategia de cartera direccional: Trading de selección de momentos impulsado por modelos de aprendizaje automático, con apalancamiento del 0-200%.
  - Estrategia Delta neutral: Arbitraje y cobertura entre mercados, con apalancamiento del 0-300%.
- **Rendimiento histórico** (marzo 2024 - noviembre 2025):
  - Rentabilidad acumulada: +39.22%
  - Rentabilidad anualizada: ~22%
  - Rendimiento mensual promedio: +1.50%
  - Porcentaje de meses rentables: 95.0% (19/20)
  - Máximo retroceso: 1.12%
- **Archivos relacionados**: [Introducción al proyecto EA](../QUANT/EA/EA.md)

### Guerra Prolongada del Capital

- **Origen**: Marco formado a mediados de enero de 2026, derivado de una reflexión sistemática sobre el problema de los inversores individuales para superar barreras de clase. La idea central es utilizar pérdidas controladas para buscar victorias decisivas, en lugar de perseguir ganancias estables o apostar todo a la riqueza súbita.
- **Funcionalidades principales**:
  - Definir límites de riesgo claros: Establecer líneas de control de riesgos, definiendo la velocidad máxima de pérdida.
  - Flujo de caja continuo: Invertir continuamente a una tasa fija, asegurando estar siempre presente en el mercado.
  - Gestión dinámica de posiciones: Aumentar la inversión en ganancias, reducirla en pérdidas.
  - Estrategia de aumento de posición con ganancias flotantes: Utilizar ganancias flotantes para amplificar la ventaja.
- **Marco matemático**:
  - La curva de capital F(t) debe satisfacer: F(t) ≥ -C × (t - T₀), donde C es la velocidad de pérdida tolerable.
  - Definir ganancia P(t) = F(t) - C × (t - T₀)
  - Cuando P(t) alcanza L, utilizar todas las ganancias flotantes para la siguiente ronda de inversión.
- **Archivos relacionados**: [Guerra Prolongada del Capital](../INSIGHTS/6.md), [Guerra Prolongada del Capital: Reafirmación de conceptos y discusión](../LOGS/35.md), [Diseño experimental de la Guerra Prolongada del Capital](../LOGS/39.md)

### Análisis de Espectro Completo (FSA)

- **Origen**: Formado el 10 de agosto de 2025, derivado de la investigación sobre la extensión de la aplicación de la Fórmula de Kelly en el ámbito de la inversión y el trading.
- **Funcionalidades principales**:
  - Definir el espacio de resultados X, que incluye rentabilidad determinista y probabilidad determinista.
  - Calcular la rentabilidad compuesta: C(X, k) = (∏(1 + k × Rᵢ)ᴾⁱ) - 1
  - Resolver el apalancamiento óptimo: k₀ = argmaxₖ C(X, k)
  - Utilizar el método iterativo de Newton para la solución numérica, manejando casos fuera del dominio factible.
- **Archivos relacionados**: [Análisis de Espectro Completo: El método óptimo para monetizar información](../QUANT/FSA/FSA.md)

---

## Temas Clave

### 1. Desarrollo de Software AI-Native

- [Arquitectura de ingeniería de software para colaboración humano-máquina a nivel de módulo](../INSIGHTS/1.md): Diseñar una arquitectura de ingeniería para colaboración humano-máquina a nivel de módulo mediante LLM, logrando control de calidad a través de cuatro capas de especificación: Protocol Spec, Implementation Spec, Test Spec, Benchmark Spec.
- [Cómo resolver el deseo de control humano: Sobre el problema de la confianza controlable en la colaboración humano-máquina](../INSIGHTS/2.md): Propone un modelo multiplicativo de dos capas para la confianza controlable, incluyendo alineación de intenciones y el triángulo de control de riesgos.
- [Abrazar lo "limitado", diseñar lo "ilimitado": Un nuevo paradigma para la construcción de sistemas de agentes basados en restricciones de LLM](../INSIGHTS/3.md): Propone tres marcos principales (Ingeniería de Coordinación, Economía de la Decisión de IA, Gestión del Flujo Cognitivo) para abordar las tres principales restricciones de los LLM: coordinación no coercitiva, presupuesto computacional limitado e incompresibilidad cognitiva.
- [Experiencia práctica y reflexiones sobre Vibe Coding](../LOGS/2.md): Actualmente, la IA no es adecuada para escribir código de programación orientada a objetos, tiende hacia la programación procedural y funcional; la IA es demasiado conservadora en los requisitos de compatibilidad hacia atrás, lo que lleva a código inflado.
- [Experiencia integrando tareas de traducción en OpenCode](../LOGS/23.md): Los Agentes tienen un rendimiento inferior a los LLM one-shot en tareas de traducción; la ventaja de los Agentes reside más en tareas que requieren razonamiento y toma de decisiones en múltiples pasos.

### 2. Colaboración de Sistemas Multi-Agente

- [Del campo de batalla al espacio digital: El significado inspirador de las órdenes de operaciones de Su Yu para el marco de colaboración de sistemas multi-agente en la nueva era](../INSIGHTS/5.md): Migrar la lógica organizativa de las órdenes de operaciones de grandes unidades de Su Yu a la colaboración multi-agente de IA, proponiendo cuatro principios principales: alineación situacional, desacoplamiento de tareas, estandarización de protocolos y coordinación central.
- [Multi-Agents: Traducción generativa adversaria](../LOGS/26.md): Introducir un Agente de Traducción y un Agente de Revisión para generación adversaria, mejorando significativamente la calidad de la traducción, pero con un consumo de tokens aproximadamente 10 veces mayor que la traducción normal.
- [Restricciones de control y optimización de memoria en Multi-Agents](../LOGS/27.md): Distinguir entre restricciones blandas (Agente controlando Agente) y restricciones duras (Script controlando Agente), proponiendo un esquema híbrido donde un Agente Orquestador genera el Script.

### 3. Inversión Cuantitativa y Estrategia de Capital

- [La naturaleza matemática del apalancamiento](../LOGS/25.md): El apalancamiento y la volatilidad son matemáticamente consistentes y pueden convertirse entre sí; se propone la distinción entre apalancamiento dentro del mercado y fuera del mercado, y sus respectivas estructuras de costos.
- [Debate comparativo: Guerra Prolongada del Capital vs. Crecimiento Estable](../DEBATES/奇葩说4v4辩论-稳健增长vs跨越阶级.md): Presenta los pros y contras de dos estrategias (crecimiento estable vs. superación de clases) en formato de debate inspirado en "Qi Pa Shuo".
- [Registro de la reunión de análisis en profundidad de fondos de inversión privados de valores de Guotai Haitong 2025](../MEETINGS/2026-01-22.md): Registra que el tamaño del mercado de fondos privados alcanzó los 7.04 billones de RMB, la influencia de los factores aumentó, el efecto de concentración en los principales actores es evidente, y la combinación orgánica de enfoques subjetivos y cuantitativos es la tendencia futura.

### 4. Creación y Distribución de Contenido

- [De la creación a la distribución: Construyendo un motor de contenido AI-Native](../INSIGHTS/4.md): Propone el principio central de "creación profunda, distribución superficial". La creación de contenido es un proceso de profundización desde la percepción hasta la cognición; la distribución de contenido debe evitar la sobrecarga de información.
- [El significado de la N en CZON: Línea quebrada del cambio de energía potencial desde la creación hasta la distribución del contenido](../LOGS/11.md): El punto de partida de la creación de contenido es bajo, la energía potencial aumenta con el refinamiento; al publicar, se proyecta en una dimensión inferior y la energía potencial disminuye; la retroalimentación de la audiencia hace que la energía potencial aumente nuevamente, formando una línea quebrada en forma de N.
- [Experimento de generación de comentarios comunitarios por IA](../LOGS/20.md): Explorar la posibilidad de que la IA genere comentarios para mejorar la interactividad del contenido, considerando añadir en el futuro una sección de comentarios y un sistema de identidad de usuario descentralizado.

### 5. Desarrollo Cognitivo y Gestión de la Complejidad

- [Regresar a lo esencial: La complejidad es un camino necesario en la cognición](../INSIGHTS/7.md): Cita la famosa frase de Oliver Wendell Holmes Jr., señalando que la simplicidad al otro lado de la complejidad es la verdadera simplicidad; una buena abstracción debe basarse en una comprensión profunda del dominio del problema.
- [Reflexión sobre la práctica de programación con IA: Evitar OOP y la compatibilidad excesiva](../LOGS/2.md): Actualmente, la IA aún no es adecuada para escribir código de programación orientada a objetos; la programación orientada a objetos requiere una comprensión profunda y capacidad de modelado del dominio del negocio, algo que la IA actualmente no puede realizar.
- [El poder del pensamiento de límite idealizado](../LOGS/24.md): Inspirado por el experimento Ralph-loop, comprender la esencia de las cosas con mayor claridad eliminando restricciones.

---

## Línea de Tiempo

| Fecha | Evento Clave |
|------|----------|
| 2025-08-10 | Finalización del documento del Análisis de Espectro Completo (FSA) |
| 2025-11 | Lanzamiento del proyecto EA en la cadena BSC |
| 2026-01-05 | Publicación de los tres artículos centrales INSIGHTS/1.md, 2.md, 3.md |
| 2026-01-08 | El proyecto ZEN cambia su nombre a CZON |
| 2026-01-10 | Publicación de INSIGHTS/4.md, proponiendo el principio de "creación profunda, distribución superficial" |
| 2026-01-14 | Publicación de INSIGHTS/5.md, proponiendo el esquema de migración del marco de órdenes de operaciones de Su Yu |
| 2026-01-17 | Finalización del borrador inicial de INSIGHTS/6.md (Guerra Prolongada del Capital) |
| 2026-01-20 | Formación de la idea central de la Guerra Prolongada del Capital |
| 2026-01-22 | Participación en la reunión de análisis en profundidad de fondos de inversión privados de valores de Guotai Haitong 2025 |
| 2026-01-24 | Finalización de la lectura de la Constitución de Claude, comprendiendo el enfoque de diseño de personificación de IA |
| 2026-01-25 | Finalización de la integración ligera de traducción con OpenCode en CZON |
| 2026-01-26 | Refactorización de la estructura de directorios de CZON (0.6.0), usando rutas en lugar de hashes |
| 2026-01-28 | Experiencia de investigación de problemas de conexión WebSocket, identificando la extensión del navegador 1Password |
| 2026-01-29 | Discusión sobre la funcionalidad RFC de LegionMind, enfatizando que la autonomía de la IA requiere alineación con una visión científica |
| 2026-01-30 | Celebración de la reunión de reafirmación de conceptos de la Guerra Prolongada del Capital |
| 2026-01-31 | Finalización del código experimental de la Guerra Prolongada del Capital utilizando OpenCode + Opus 4.5 (código abierto) |
| 2026-02-01 | Refactorización del diseño experimental de la Guerra Prolongada del Capital, proponiendo una estructura de tres capas (secuencia de mercado, estrategia de señales, estrategia de apuestas) |
| 2026-02-03 | Publicación de INSIGHTS/7.md (Regresar a lo esencial) |

---

## Personas Principales

| Persona | Rol | Notas |
|------|------|------|
| CZ (zccz14) | Iniciador del proyecto, Diseñador principal | Fundador de NTNL, graduado de la Universidad de Jiaotong de Xi'an, especialización de licenciatura desconocida. |
| Ryan | Trader cuantitativo senior | Responsable del desarrollo y optimización de estrategias de trading, licenciatura y maestría de la Universidad de Jiaotong de Xi'an. |
| Mage | Trader cuantitativo senior | Responsable del desarrollo y optimización de estrategias de trading, maestría de la Universidad de Jiaotong de Xi'an. |
| C1 (Thrimbda) | Ingeniero senior | Responsable de ingeniería de sistemas, licenciatura de la Universidad de Jiaotong de Xi'an. |
| Xi Chen | Director general del proyecto | Profesor titular de la Escuela de Negocios de la Universidad de Nueva York (NYU), licenciatura de la Universidad de Jiaotong de Xi'an, maestría y doctorado de CMU. |
| GB | Amigo, usuario temprano de CZON | Encontró problemas con el proxy HTTP al usar CZON. |
| Hobo | Amigo, interlocutor en discusiones | Tiene puntos de vista diferentes sobre el nivel de inteligencia de los LLM y los métodos de ingeniería. |
| C1 | Interlocutor en discusiones | Líder del proyecto LegionMind. |
| RYAN | Interlocutor en discusiones | Relacionado con el proyecto 1earn. |
| Thrimbda (C1) | Interlocutor en discusiones | Partidario del proyecto ZEN/CZON. |
| MiroThinker Pro | Herramienta de búsqueda | Utilizada para ayudar a investigar problemas de conexión WebSocket. |

---

## Resumen de Marcos Teóricos

### Modelo Multiplicativo de Dos Capas de Confianza Controlable

- **Idea central**: A través de mecanismos de garantía sistemáticos, permitir que los humanos deleguen con confianza en los Agentes bajo la premisa de que los riesgos están controlados.
- **Elementos clave**:
  - Capa base: Alineación de intenciones (alineación expresiva, alineación de valores, alineación estructural, alineación dinámica).
  - Capa de ejecución: Triángulo de control de riesgos (previsibilidad × intervenibilidad × recuperabilidad).
- **Expresión matemática**: Confianza controlable = Índice de alineación de intenciones × Índice de control de riesgos.

### Marco de Órdenes de Operaciones de Su Yu

- **Idea central**: Migrar el arte del mando de grandes unidades humanas a la colaboración multi-agente de IA.
- **Elementos clave**:
  - Unificación cognitiva primero.
  - La estructura determina la función (división modular del trabajo).
  - El protocolo por encima de la comunicación (estandarización de la colaboración).
  - Ejecución distribuida bajo mando centralizado.

### Marco de la Guerra Prolongada del Capital

- **Idea central**: Utilizar pérdidas controladas para buscar victorias decisivas, definiendo límites de riesgo y condiciones de victoria claros.
- **Elementos clave**:
  - Principio de no arriesgar: Controlar la velocidad máxima de pérdida.
  - Principio de no desperdiciar: Intercambiar tiempo por ventaja.
  - Principio de no retrasar: Aumentar posición con ganancias flotantes.
  - Principio de no ser ambiguo: Definir claramente las condiciones de victoria.
- **Expresión matemática**: F(t) ≥ -C × (t - T₀)

### Análisis de Espectro Completo (FSA)

- **Idea central**: Estrategia de apuesta probabilizada que maximiza la tasa de crecimiento compuesta.
- **Elementos clave**:
  - Definición del espacio de resultados (rentabilidad determinista, probabilidad determinista, exclusividad mutua, exhaustividad).
  - Cálculo de la rentabilidad compuesta.
  - Resolución del apalancamiento óptimo (método iterativo de Newton).
  - Mecanismo de defensa contra cisnes negros.
- **Expresión matemática**: C(X, k) = (∏(1 + k × Rᵢ)ᴾⁱ) - 1

---

## Índice de Proyectos y Tecnologías

| Nombre del Proyecto | Descripción | Archivos Relacionados |
|----------|------|----------|
| CZON | Herramienta AI-Native para creación y distribución de contenido | LOGS/8-12, LOGS/18-19, LOGS/23, LOGS/26, LOGS/32, INSIGHTS/4 |
| EA | Proyecto de fondo de prioridad en la cadena BSC | QUANT/EA/EA.md |
| 1earn | Sistema cuantitativo de estrategia Delta neutral | LOGS/27 |
| LegionMind | Marco de gestión y colaboración de proyectos con Agentes de IA | LOGS/14, LOGS/29 |
| Yuan | Infraestructura para trading cuantitativo | LOGS/27 |
| CapitalProtractedWar | Proyecto de código abierto para experimentos de la Guerra Prolongada del Capital | LOGS/36, LOGS/38, LOGS/39, LOGS/41 |
| CZONE | Versión en línea de CZON (en planificación) | LOGS/18, LOGS/32 |
| OpenCode | Plataforma de Agentes de IA | LOGS/18-19, LOGS/23, LOGS/25-26, LOGS/36, LOGS/38, LOGS/41 |
| vibe-kanban | Tablero Kanban para gestión de proyectos con IA | LOGS/1 |