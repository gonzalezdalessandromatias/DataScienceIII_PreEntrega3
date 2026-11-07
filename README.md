### README


# Clasificador de noticias con TF-IDF · AG News

## Descripción
Baseline de clasificación supervisada de texto sobre AG News (4 categorías:World", "Sports", "Business", "Sci/Tech).
Pipeline: carga -> preprocesamiento -> TF-IDF -> modelo -> evaluación.

## Cómo reproducir
pip install -r requirements.txt
jupyter notebook notebook.ipynb   # Kernel -> Restart & Run All

## Decisiones técnicas
- **Vectorización:** TF-IDF con ngram_range=(1, 2), max_features=30,000  (explorado en validación).
- **Sin fuga de datos:** el vectorizador se ajusta solo con el train; el test se usa una vez, al final.
- **Modelo:** SVM lineal. 
**Justificación:** 
Mejor rendimiento global: Lidera tanto en F1 macro (0.900) como en Accuracy (0.901), superando la barrera del 90%.
Excelente balance de eficiencia: Tarda solo 0.343 segundos en entrenar, lo cual es muy veloz y escala perfectamente con matrices dispersas de TF-IDF.

En espacios de alta dimensionalidad y dispersión léxica (como los generados por unigramas y bigramas en TF-IDF), los hiperplanos de margen máximo de SVM suelen encontrar mejores fronteras de decisión que los modelos lineales probabilísticos.

Modelo final seleccionado:SVM lineal, maximiza la separación de clases con el F1 macro más alto (0.900) y un tiempo de ajuste altamente eficiente.

## Resultados

modelo                 -F1 macro (val) - accuracy (val) - segundos fit		

Naive Bayes	           -    0.891	   -       0.892	    -       0.029

Regresión Logística	    -   0.889	   -       0.889	      -     3.445

**SVM lineal	          -     0.900	  -        0.901	    -       0.34**

## Análisis de errores
Categorías que más cuestan: Sport y World entre sí, pero incluso se observa que las demas categorias son mas propensas a confundirse con las categorias World o Sport. Se confunden entre sí porque comparten vocabulario como:
Ejemplos de 'Sports' predichos como 'World':

- Hundreds mourn loss of student killed by police during Red Sox &lt;b&gt;...&lt;/b&gt;. As hundreds of mourners paid their final respects Tuesday to Victoria Snelgrove, her pastor denounced the raucous
- Times to Scrap Broadsheet Edition. The Times is to scrap its broadsheet edition and go tabloid from Monday, it was confirmed today. The decision was made after a trial run of the compact edition prove
- Beijing gears up for 2008. Although the Beijing Olympics is still four years away, the Chinese capital is already gearing up to host the event. The city of over 12 million is refurbishing ancient land
- Cycling: Ulmer #39;s scorching times in secret rides. New Zealand #39;s star cyclist, Sarah Ulmer, last week rode under world record time twice in an hour during a secret training session in France.
- Agent: Extortion Plot Targeted Sheffield (AP). AP - New York Yankees slugger Gary Sheffield and his wife were the targets of a blackmailer who claimed to have embarrassing sexual videotapes of her and


Cómo mejorarlo: trigramas, embeddings, más limpieza.
