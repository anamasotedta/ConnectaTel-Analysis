# Proyecto de Análisis de Datos: ConnectaTel 📊

Este proyecto consiste en un Análisis Exploratorio de Datos (EDA) y limpieza de la base de datos de usuarios de **ConnectaTel**. El objetivo principal fue identificar la calidad de la información, corregir anomalías, estructurar segmentos de clientes por edad y consumo, y detectar oportunidades de negocio para optimizar la oferta actual de planes (Básico vs. Premium).

---

## ⚠️ Problemas Detectados y Calidad de los Datos

Durante las primeras etapas del análisis, identifiqué e intervine varias inconsistencias críticas en la base de datos:
* **Valores nulos ocultos con caracteres extraños:** La columna `city` presentaba signos de interrogación (`?`) en lugar de strings limpios, ocultando la falta de información en múltiples registros.
* **Fechas inconsistentes y futuristas:** En la columna `reg_date` se detectaron registros con años inválidos que superaban el 2024 (errores de captura o corrupción del sistema), los cuales convertí formalmente a valores no válidos (`NaT`).
* **Análisis de Datos Faltantes - MAR (Missing At Random):** Al evaluar los nulos en las columnas `duration` y `length` dentro del dataset de consumos, comprobé estadísticamente que su ausencia depende por completo de la columna `type`. Los mensajes (`text`) presentan un **99.92% de nulos en `duration`** (ya que no tienen duración en tiempo) y las llamadas (`call`) tienen un **99.93% de nulos en `length`** (al no contener caracteres de texto). **Decidí conservar estos nulos intactos**, dado que imputarles cualquier valor (como ceros o promedios) alteraría negativamente las métricas del negocio.

---

## 📊 Patrones de Uso Extremo (Outliers)

Al aplicar el método del Rango Intercuartílico (IQR) en las variables de consumo (`cant_mensajes`, `cant_llamadas` y `cant_minutos_llamada`), encontré un fuerte sesgo a la derecha con la presencia de usuarios extremos (por ejemplo, clientes que acumulan hasta 160 minutos de conversación). 

* **Decisión Estratégica:** A diferencia de un criterio de limpieza puramente matemático, **decidí conservar estos outliers intactos**. Estos valores son completamente plausibles dentro del contexto de telecomunicaciones y representan a los usuarios más activos ("Heavy Users"). Modificar o recortar estos registros hubiera sesgado artificialmente los promedios y arruinado la segmentación posterior de clientes.

---

## 🔍 Segmentos por Edad

Para entender la demografía de nuestra plataforma, estructuré una segmentación en tres bloques lógicos:
* **Predominio del segmento Adulto:** El grupo con mayor volumen está compuesto por **Adultos** (30 a 59 años), seguido por los **Adultos Mayores** (60 años o más), dejando a los **Jóvenes** (menores de 30) como la minoría en la base de datos.
* **Comportamiento idéntico entre planes:** Al cruzar las edades con el tipo de plan contratado, descubrí que tanto los jóvenes de 20 años como los adultos de 75 eligen el plan Básico y el Premium exactamente en la misma proporción. La edad no influye al momento de decidir pagar una suscripción más alta.

---

## 📊 Segmentos por Nivel de Uso

Clasifiqué a los clientes según sus interacciones utilizando reglas lógicas basadas en la cantidad de llamadas y mensajes enviados:
* **Concentración masiva en 'Uso medio':** La gran mayoría de los clientes se ubica sólidamente en la categoría de **Uso medio** (usuarios que promedian entre 5 y 10 llamadas/mensajes por periodo).
* **Falta de correspondencia con el plan contratado:** Al mantener los datos reales de los outliers, se volvió evidente que los usuarios catalogados como 'Alto uso' (los que generan mayor tráfico) se están quedando en el plan Básico sin migrar al plan Premium. El comportamiento de consumo en telefonía tradicional está totalmente desconectado del servicio contratado.

---

## ➡️ Esto sugiere que...

Los clientes actuales **notoriamente no están eligiendo su plan en función de cuántos minutos hablan o cuántos mensajes mandan** en su día a día. Dado que las curvas de consumo y las distribuciones por segmentos resultaron ser un calco exacto entre el plan Básico y el Premium, esto nos indica que:
1. El plan Premium probablemente se vende por otros beneficios técnicos que no figuran en estas métricas de voz/texto (como gigas de internet móvil o datos ilimitados).
2. **ConnectaTel tiene una fuga de valor latente:** Los usuarios con picos de "Alto uso" se están aprovechando de la falta de restricciones del plan Básico, consumiendo recursos masivos de la red sin tener un incentivo real para pagar una cuenta Premium.

---

## 💡 Recomendaciones Estratégicas

* **Establecer límites inteligentes en el Plan Básico:** Utilizando el límite estadístico del IQR que calculamos (alrededor de los 61.85 minutos), se recomienda fijar un tope estricto de **60 minutos de llamada** en el plan Básico. Si un cliente supera este umbral, el sistema debería cobrar minutos adicionales o forzar la migración al plan Premium.
* **Campañas de Up-selling dirigidas al segmento de 'Alto Uso':** Ahora que tenemos bien identificados a los usuarios atípicos de alto consumo que están en el plan Básico, podemos lanzarles promociones directas para demostrarles que por su volumen de llamadas les conviene adquirir la tarifa plana o los beneficios del plan Premium.
* **Diseñar una oferta empaquetada para el segmento 'Adulto':** Al ser nuestro nicho demográfico más grande y concentrarse en un 'Uso medio', se puede estructurar un plan intermedio optimizado para sus necesidades, asegurando su fidelidad a largo plazo.
