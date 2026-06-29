# Proyecto de Análisis de Datos: ConnectaTel 📊

Este proyecto consiste en un Análisis Exploratorio de Datos y limpieza de la base de datos de usuarios de **ConnectaTel**. El objetivo principal fue identificar la calidad de la información, corregir anomalías, estructurar segmentos de clientes por edad y consumo, y detectar oportunidades de negocio para optimizar la oferta actual de planes (Básico vs. Premium).

---

## ⚠️ Problemas Detectados en los Datos

Durante las primeras etapas del análisis, se identificaron varias inconsistencias críticas que afectaban la integridad de la base de datos:
* **Valores nulos ocultos con caracteres extraños:** La columna `city` presentaba signos de interrogación (`?`) en lugar de strings limpios o valores nulos estándar, lo que enmascaraba la falta de información en múltiples registros.
* **Fechas inconsistentes y futuristas:** En la columna `reg_date` se detectaron registros con años inválidos que superaban el 2024 (errores de captura o corrupción del sistema), los cuales debieron convertirse formalmente a valores no válidos (`NaT`).
* **Presencia extrema de outliers (Valores Atípicos):** Las variables de consumo (`cant_mensajes`, `cant_llamadas` y `cant_minutos_llamada`) presentaban distribuciones con un fuerte sesgo a la derecha. El caso más severo fue el de los minutos de llamada: mientras que el 75% de los usuarios hablaba un máximo de 31 minutos, se identificaron registros atípicos con hasta 160 minutos. Estos valores representaban aproximadamente un 5% de la muestra e inflaban artificialmente los promedios generales.

---

## 🔍 Segmentos por Edad

Para entender mejor demográficamente la base de usuarios, se segmentaron en tres bloques lógicos:
* **Predominio del segmento Adulto:** El grupo con mayor volumen está compuesto por **Adultos** (30 a 59 años), seguido por los **Adultos Mayores** (60 años o más), dejando a los **Jóvenes** (menores de 30) como la minoría en la plataforma.
* **Comportamiento idéntico entre planes:** Al cruzar los grupos de edad con el tipo de plan contratado, se descubrió que tanto los jóvenes de 20 años como los adultos de 75 eligen el plan Básico y el Premium exactamente en la misma proporción. La edad no influye en absoluto al momento de decidir pagar una suscripción más alta.

---

## 📊 Segmentos por Nivel de Uso

Se clasificó a los clientes según sus interacciones utilizando reglas lógicas basadas en la cantidad de llamadas y mensajes enviados:
* **Concentración masiva en 'Uso medio':** La gran mayoría de los clientes se ubica sólidamente en la categoría de **Uso medio** (usuarios que promedian entre 5 y 10 llamadas/mensajes por periodo), mientras que las categorías de 'Alto uso' y 'Bajo uso' retienen volúmenes drásticamente menores.
* **Falta de correspondencia con el plan contratado:** Analizando visualmente las distribuciones por plan, los usuarios catalogados como 'Alto uso' no están migrando al plan Premium, de la misma forma que los de 'Bajo uso' tampoco se quedan exclusivamente en el Básico. El comportamiento de consumo en telefonía tradicional está desconectado del servicio contratado.

---

## ➡️ Esto sugiere que...

Los clientes actuales **no están eligiendo su plan en función de cuántos minutos hablan o cuántos mensajes mandan** en su día a día. Los histogramas de consumo y las gráficas por segmentos resultaron ser un calco exacto entre el plan Básico y el Premium. 

Esto nos indica dos escenarios de negocio muy claros:
1. El plan Premium se está vendiendo exitosamente por otros beneficios técnicos que no figuran en estas métricas específicas (por ejemplo: gigas de internet móvil, datos ilimitados, beneficios de roaming o simples estrategias visuales de marketing).
2. **ConnectaTel tiene una fuga de valor latente:** Hay usuarios con picos de "Alto uso" que se están aprovechando de las ventajas del plan Básico sin tener un incentivo o una restricción real en el sistema que los impulse a mejorar su nivel de cuenta.

---

## 💡 Recomendaciones Estratégicas

* **Reestructurar los límites de los planes (Frenar los Outliers):** Basado en los límites superiores calculados bajo el método matemático del Rango Intercuartílico (IQR), se recomienda fijar un tope estricto de **60 minutos de llamada** en el plan Básico. Si un cliente supera este umbral, el sistema debería cobrar minutos adicionales o forzar la migración al plan Premium.
* **Diseñar un plan enfocado en el segmento 'Adulto' y 'Uso Medio':** Al ser el nicho demográfico y operativo más rentable y numeroso, se puede empaquetar una oferta ajustada que cubra perfectamente el rango estándar de 4 a 6 llamadas/mensajes diarios para maximizar la fidelización.
* **Campañas dirigidas de Up-selling:** Filtrar la base de datos para extraer exclusivamente a los usuarios etiquetados en el grupo de 'Alto uso' que sigan registrados bajo el plan Básico. A este segmento se le debe enviar una campaña publicitaria directa para demostrarles que, debido a su ritmo de consumo diario, les conviene adquirir la tarifa plana o los beneficios añadidos del plan Premium.
