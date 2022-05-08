# Ervin_Portafolio
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

insertar imagen()

La gráfica anterior nos muestra que entre más grande, en terminos de edad, sea la población, la mediana del ingreso de las personas se reduce. Para poder encontrar una relación se procederá a utilizar una variable distinta con el ingreso, el nivel educativo.

`pobMexico %>% 
  ggplot(aes(x = nivelaprob, y = edad, group = nivelaprob )) + 
  geom_boxplot() + 
  labs(title = "Los niveles educativos más bajos tienen una edad mediana más alta", 
       subtitle = "¿Causalidad? No, pero explicación sí: la expansión de la educación media/superior fue en los 60/70", 
       y = "Edad", 
       x = "Último nivel aprobado",
       caption = "Elaboración propia a partir de las bases de datos de la Encuesta Nacional Ingreso Gasto del INEGI")`

insertar imagén()

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
Insertar imagen()

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
 
INSERTAR IMAGEN()

La gráfica de arriba explica de mejor manera como la expansión de la educación media/superior fue en 1960, por lo que la mediana se ve afecada por la política pública de ese momento, un buen tema de análisis para alguien que estudie el impacto de las políticas públicas a largo plazo.


