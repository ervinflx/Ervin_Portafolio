# Ervin Portafolio
Portafolio de data science


## Proyecto 1.- 
¿Existe una relación entre los ingresos y la edad de las personas en México?
Observemos el siguiente gráfico. 

`pobMexico %>% 
  ggplot(aes(x = edad, y = ingreso_trim, group = edad )) + 
  geom_boxplot() + 
  labs(title = "Los ingresos medianos bajan con la edad", 
       subtitle = "Pero no se sabe por qué a simple vista", 
       y = "Ingreso trimestral en $ M.N.", 
       x = "Edad en años cumplidos", caption = "Elaboración propia a partir de las bases de datos de la Encuesta Nacional Ingreso Gasto del INEGI")`

![](/IMAGES/ingresos%20primera%20foto.jpeg)

La gráfica anterior nos muestra que entre más grande, en terminos de edad, sea la población, la mediana del ingreso de las personas se reduce. Para poder encontrar una relación se procederá a utilizar una variable distinta con el ingreso, el nivel educativo.

`pobMexico %>% 
  ggplot(aes(x = nivelaprob, y = edad, group = nivelaprob )) + 
  geom_boxplot() + 
  labs(title = "Los niveles educativos más bajos tienen una edad mediana más alta", 
       subtitle = "¿Causalidad? No, pero explicación sí: la expansión de la educación media/superior fue en los 60/70", 
       y = "Edad", 
       x = "Último nivel aprobado",
       caption = "Elaboración propia a partir de las bases de datos de la Encuesta Nacional Ingreso Gasto del INEGI")`

![](/segunda%20foto.jpeg)

Se observa en la gráfica anterior de boxplots que la mediana de los ingresos aumenta conforme el nivel educativo de las personas va incrementando ¿En este caso se podría hablar de una causalidad? No, se tienen que realizar diferentes estudios como los que se explican en la gráfica para poder hablar de una causalidad.
Hay que tomar la variable de años y la variable del nivel educativo para observar qué relacionan tienen entre sí.

`pobMexico %>% 
  ggplot(aes(x = nivelaprob, y = edad, group = nivelaprob )) + 
  geom_boxplot() + 
  labs(title = "Los niveles educativos más bajos tienen una edad mediana más alta", 
       subtitle = "¿Causalidad? No, pero explicación sí: la expansión de la educación media/superior fue en los 60/70", 
       y = "Edad", 
       x = "Último nivel aprobado",
       caption = "Elaboración propia a partir de las bases de datos de la Encuesta Nacional Ingreso Gasto del INEGI")`

![](https://github.com/ervinflx/Ervin_Portafolio/blob/main/IMAGES/tercera%20imagen.jpeg)

La gráfica anterior es interesante de observar. Por un lado, los niveles educativos más bajos tienen medianas más altas, entonces ¿se puede aseverar que los niveles educativos influyen a que las personas jovenes solamente busquen tener mayores niveles educativos? No, pero hay una explicación más interesante aún, entre 1960 y 1970 el gobierno de México comenzó a expander la educación media/superior a nivel nacional, por ende, han pasado entre 50 y 40 años, lo que ha permitido que la mediana de los niveles de educación vayan descendiendo.

`activo %>% 
  filter( edad <90) %>% 
  mutate(nivelaprob = fct_relevel(nivelaprob, "Ninguno"), 
         año = 2016-edad) %>% 
  count(año, nivelaprob) %>%
  group_by(año) %>%
  mutate(n = n/ sum(n)) %>% 
  ggplot(aes(x=año, y = n, group = nivelaprob, fill= nivelaprob)) + 
  geom_area() + 
  scale_fill_viridis_d() +
  scale_y_continuous(labels = scales::percent) +
  labs(title = "Los niveles educativos más bajos tienen una edad mediana más alta", 
       subtitle = "¿Causalidad? No, pero explicación sí:\n la expansión de la educación media/superior fue en los 60", 
       x = "Años", 
       y = "Proporción condicional a edad",
       fill= "Nivel de Aprobación",
       caption = "Elaboración propia a partir de las bases de datos\n de la Encuesta Nacional Ingreso Gasto del INEGI")`
 
![](https://github.com/ervinflx/Ervin_Portafolio/blob/main/IMAGES/cuarta%20foto.jpeg)

La gráfica de arriba explica de mejor manera como la expansión de la educación media/superior fue en 1960, por lo que la mediana se ve afecada por la política pública de ese momento, un buen tema de análisis para alguien que estudie el impacto de las políticas públicas a largo plazo. Sin embargo, aún no se explica ¿por qué los ingresos medianos bajan con la edad? Se plantean dos hipótesis, 1) por diversos factores que están asociados directamente al envejecimiento (disminución de capacidad, discriminación, desactualización); 2) la composición educativa de las edades.


Se reducirán las categorías educativas a tres grupos para poder tener una mejor visualización: hasta primaria, secundaria y mayor que secundaria.

`reco_nivelaprob <- 
  tibble::tribble(
    ~nivelaprob, ~nivelaprob_reco,
    "Profesional", "profesional",
    "Posgrado", "profesional",
    "Primaria", "primaria<=",
    "Técnica/Normal", "> primaria",
    "Secundaria", "> primaria",
    "Preparatoria o bachillerato", "> primaria",
    "Ninguno", "primaria<=")`


`activo %>% 
  left_join(reco_nivelaprob) %>% 
  mutate(edad = chop_width(edad, width = 5), 
         nivelaprob_reco = fct_relevel(nivelaprob_reco, "primaria<="))  %>%
  ggplot(aes(x= edad, y = ingreso_trim)) + 
  geom_boxplot(aes(fill = nivelaprob_reco)) + 
  geom_boxplot(alpha = 0.2, color = "grey") + 
  scale_fill_viridis_d() + 
  labs(title = "El ingreso siempre es más bajo con educación baja", 
       subtitle = "Pero también baja con la edad independientemente de la educación", 
       x = "Grupo de edad",
       y = "Ingreso trimestral en $ M.N.", 
       fill = "Nivel educativo
       agrupado",
       caption = "Elaboración propia a partir de las bases de datos\n de la Encuesta Nacional Ingreso Gasto del INEGI")`

![](https://github.com/ervinflx/Ervin_Portafolio/blob/main/IMAGES/Ingreso%20siempre%20es%20bajo%20con%20educaci%C3%B3n%20baja.jpeg)

La gráfica muestra que en efecto hay una relación entre la composición educativa de las edades, el ingreso siempre es más bajo con una educación baja, sin embargo, también baja con la edad, independientemente de la educación. Entonces ninguna de las dos hipótesis puede ser descartada. Finalmente midamos el nivel de relación que tienen cada una de las variables respecto al ingreso, para ello se realizará una regresión lineal simple.

`lmingreso = (lm(ingreso_trim ~ edad + nivelaprob, data = activo))`
`stargazer(lmingreso, type = "text")`


![](https://github.com/ervinflx/Ervin_Portafolio/blob/main/IMAGES/2022-05-08%20(2).png)


La tabla nos muestra que existe una alta relación entre todos los niveles educativos y la edad al momento de esperar cierto incremento en el ingreso trimestral de las personas mexicanas. Agreguemos más variables y a partir de entonces veamos qué variables de todas las que INEGI provee son más importantes o menos importantes en el impacto que tengan con el ingreso trimestral.





