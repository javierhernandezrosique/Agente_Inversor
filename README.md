# 📈 Optimización de Carteras Multiactivo mediante Aprendizaje por Refuerzo Personalizado

Este repositorio contiene el código y la memoria del proyecto final de la asignatura **Extensiones de Machine Learning (Bloque II)** del Grado en Ciencia e Ingeniería de Datos de la **Universidad de Murcia** (Curso 2025/2026).

**Autores:** Javier Hernández Rosique y Álvaro Espejo Martínez.

---

## 🚀 Descripción del Proyecto

El objetivo de este proyecto es el diseño e implementación de un agente de inversión autónomo basado en **Aprendizaje por Refuerzo (Reinforcement Learning)**. A diferencia de los enfoques tradicionales que buscan puramente batir al mercado en rentabilidad absoluta, este agente realiza una gestión de activos contextualmente consciente, integrando la **aversión al riesgo y el horizonte temporal del inversor** como variables fundamentales.

El proyecto evalúa la hipótesis de que el RL puede modelar carteras multiactivo, comparando arquitecturas tabulares clásicas (SARSA) frente a aproximadores de función profundos (Deep Q-Network).

## 📊 Datos y Entorno

El entorno de simulación (basado en `Gymnasium`) utiliza datos históricos extraídos mediante la API de *yfinance*, abarcando desde 2010 hasta principios de 2025. 

* **Activos Comerciales:** * Renta Variable: `SPY` (S&P 500) y `QQQ` (Nasdaq 100).
  * Activos de Refugio: `TLT` (Bonos del Tesoro) y `GLD` (Oro).
* **Sensores de Contexto (EDA):** Índice de volatilidad (`^VIX`) y Tasa de bonos a 10 años (`^TNX`).
* **División Temporal (Sin Data Leakage):** * *Train:* 2010 - 2021 (Entorno de tipos bajos y crecimiento secular).
  * *Test (Out-of-sample):* 2022 - 2025 (Cambio de régimen, alta inflación, volatilidad).

## 🧠 Modelos Implementados

### 1. SARSA V1 & V2 (Baseline y Optimización)
Se implementó un algoritmo temporal tabular *on-policy*. La versión V2 introdujo un realismo operativo estricto con costes de transacción (0.05%) y entrenamiento por ventanas aleatorias (Domain Randomization).
* **Resultados destacados en Test:** El agente SARSA V2 demostró una robustez excepcional, logrando **superar al mercado (Buy & Hold)** reduciendo drásticamente el riesgo de cola:
  * **Sharpe Ratio:** 1.17 (vs 0.77 del SPY).
  * **Max Drawdown:** -10.39% (vs -22.09% del SPY).
  * **Rentabilidad Total:** 41.65% (vs 40.26% del SPY).

### 2. Deep Q-Network (DQN) Multiobjetivo
Se diseñó una arquitectura de red neuronal profunda (MLP 128-128-64 con Dropout) para gestionar un vector de estado continuo y multidimensional que incluye los cuatro activos y parámetros del usuario.
* Implementa *Experience Replay* y *Target Network*.
* Función de recompensa que penaliza la volatilidad dinámicamente según la aversión al riesgo configurada.
* **Resultados:** El agente aprendió exitosamente a diferenciar perfiles (Agresivo, Moderado, Conservador), modulando su exposición a liquidez y activos refugio. Sin embargo, acusó la dimensionalidad del espacio y el cambio drástico de régimen macroeconómico en el periodo de test, mostrando un sesgo defensivo frente al benchmark.

## 📂 Estructura del Repositorio

* `Notebook_Descriptivo_Dataset (2).ipynb`: Análisis Exploratorio de Datos (EDA), validación estadística, distribuciones y matriz de correlación.
* `Guia.ipynb`: Cuaderno guía principal del proyecto.
* `SARSA_Implementation.ipynb`: Implementación del prototipo inicial tabular (V1).
* `SARSA_V2.ipynb`: Modelo SARSA optimizado con fricciones operativas y validación superior al mercado.
* `DQN_Implementation.ipynb`: Arquitectura base de Deep Q-Network.
* `Advanced_DQN_Implementation.ipynb`: Implementación avanzada multiactivo con perfilado de riesgo de usuario.
* `env_code.txt`: Código fuente del entorno financiero personalizado en Gymnasium.
* `Memoria_TrabajoFinal_EML.docx (2).pdf`: Documento académico completo con el marco teórico, justificación matemática y conclusiones detalladas.

## 💡 Conclusiones Principales

1. **Simplicidad vs Complejidad:** En entornos financieros con alta varianza y datos limitados, un modelo simple con un espacio de estados acotado (SARSA V2) puede resultar más robusto frente a cambios de régimen que redes neuronales profundas (DQN).
2. **Protección del Capital:** El Aprendizaje por Refuerzo demuestra una alta eficacia para aprender políticas defensivas que minimizan los *drawdowns* severos.

## 🔮 Vías Futuras
Se propone la expansión del modelo mediante **Procesamiento de Lenguaje Natural (NLP)** integrando arquitecturas como *FinBERT* para procesar informes de la Reserva Federal y noticias, fusionando el sentimiento macroeconómico con los sensores de mercado para anticipar cambios de régimen.
