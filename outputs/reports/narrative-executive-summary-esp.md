# 📑 Resumen Ejecutivo – Cyclistic Sistema de Bicicletas Compartidas (2019–2020)

Durante el período 2019–2020, el sistema de bicicletas compartidas registró **17,7 millones de viajes**, con un crecimiento interanual del **23.8%**. Este aumento sugiere que la bicicleta se consolidó como alternativa de transporte confiable y flexible, y que algunos ciudadanos optaron por transporte individual y saludable por sobre el transporte colectivo clásico.

El análisis muestra que la **base de usuarios está dominada por suscriptores** (88% de los viajes), lo que refleja un uso recurrente y fidelizado. Los clientes ocasionales representan el 12% restante, un segmento menor pero con potencial de crecimiento, especialmente en temporada alta y en zonas turísticas.  

En términos geográficos, la **demanda está fuertemente concentrada en Manhattan**, particularmente en barrios como **Lower East Side, Chelsea, Greenwich Village y Gramercy**. Estas estaciones aparecen tanto como principales puntos de inicio como de destino, lo que indica un patrón de **viajes intra-borough de corta distancia**. Sin embargo, Brooklyn comienza a emerger en el mapa: estaciones como **11201 (Northwest Brooklyn)** figuran entre las más relevantes en minutos totales de viaje, lo que sugiere **conexiones inter-borough de mayor duración**.  

La concentración es marcada: apenas un 20% de las estaciones explican cerca del 80% de los viajes. Esto implica que la experiencia del usuario depende de un número reducido de nodos críticos, lo que aumenta la vulnerabilidad ante saturación o fallas operativas.  

El análisis temporal confirma una **estacionalidad fuerte**, con picos en los meses de verano (julio–septiembre). En 2020, estos picos superaron ampliamente a los de 2019, consolidando la tendencia de crecimiento. El clima es un factor determinante: la **temperatura muestra una correlación positiva fuerte con la demanda**, mientras que el viento tiene un efecto negativo leve y la precipitación diaria no presenta un impacto claro, probablemente por la granularidad de la medición.  

En cuanto a la **congestión de estaciones**, se identifican desequilibrios significativos. Estaciones como **10199 y 10019** presentan déficit crónico (más salidas que llegadas), mientras que otras como **10014, 10011 y 10003** acumulan superávit (más llegadas que salidas). Este patrón refleja flujos netos que requieren **rebalancing operativo constante** y, en algunos casos, **ampliación de capacidad de docks**.  

---

## 🎯 Implicaciones para el negocio
1. **Refuerzo en estaciones críticas:** priorizar mantenimiento, aprovisionamiento y capacidad en las estaciones top de Manhattan.  
2. **Gestión de congestión:** implementar estrategias de redistribución dinámica entre estaciones deficitarias y superavitarias.  
3. **Expansión estratégica:** considerar nuevas estaciones en Brooklyn y zonas limítrofes para diversificar la red y aliviar presión en Manhattan.  
4. **Aprovechar la estacionalidad:** reforzar operaciones y campañas de captación en verano, especialmente orientadas a clientes ocasionales.  
5. **Optimización de experiencia:** garantizar disponibilidad de bicicletas y docks en estaciones de alta rotación para sostener la satisfacción del usuario.  