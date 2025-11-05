# 🚲 Cyclistic Bike Sharing Analysis (2019–2020)

Este proyecto analiza el uso de un sistema ficticio de bicicletas compartidas en Nueva York durante los años 2019 y 2020.  
El objetivo es comprender patrones de demanda, estacionalidad, impacto del clima y congestión en estaciones, para orientar decisiones de negocio y planificación operativa.

---

## 📂 Estructura del proyecto

- **`/notebooks/`** → Contiene el análisis exploratorio y el desarrollo paso a paso en Jupyter Notebook.  
- **`/data/`**  
  - **`/raw/`** → Target tables resultantes de las consultas SQL en Google Big Query.  
  - **`/processed/`** → Datos extraídos, limpios y listos para análisis BI en Tableau. 
  - **`/sql-queries/`** → Consultas SQL que dieron origen a los datasets (target table).  
- **`/outputs/`**  
  - **`/visualizations/`** → Gráficos exportados desde los notebooks.  
  - **`/reports/`** → Resumen ejecutivo en formato PDF/Markdown.  
  - **`/dashboards/`** → Capturas y enlace al dashboard interactivo.  
- **`/project-planning-docs/`** → Documentación de planificación del proyecto (project requirements, stakeholder requirements, strategy).  

---

## 📊 Resultados principales

- Crecimiento interanual del **23%** en 2020.  
- **Manhattan concentra ~80% de los viajes**, con fuerte estacionalidad en verano.  
- La **temperatura es el factor climático más influyente** en la demanda.  
- Se detectan **estaciones críticas con déficit y superávit**, que requieren rebalancing operativo.  

---

## 📈 Dashboard interactivo

El dashboard ejecutivo con visualizaciones dinámicas está disponible en **[Tableau Public](https://public.tableau.com/views/AnlisisBICyclisticinfraestructuraytendenciasenlademanda/Mapadeestacionesmsdemandadas?:language=pt-BR&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link)**.  

---

## ⚙️ Reproducibilidad

1. Clonar este repositorio.  
2. Instalar dependencias:  
   ```bash
   pip install -r requirements.txt
   ```  
3. Abrir el notebook en `/notebooks/` y ejecutar las celdas.  

---

## 📜 Licencia

Este proyecto utiliza datos ficticios y se comparte con fines educativos y de demostración.  
Licencia: MIT.
