# Proyecto-Base-de-Datos
## Pregunta 1: Construir un modelo E/R en notación Ramakrishnan o CHEN, pertinente para modelar el problema usando las ténicas y convenciones vistas en clases. Puede utilizar herramientas de modelamiento de datos o simplemente un programa de dibujo como draw.io.
Para la pregunta 1 se utilizó draw.io y el modelo se encuentra Diagrama E1.drawio.pdf
Para construir el modelo se utilizó notación CHEN
Las entidades que se definieron fueron: 
- Campeonato: Entidad fuerte, ya que tiene su propio identificador (id_campeonato) y existe independientemente a cualquier otra entidad.
- Fecha: Entidad debil, ya que esta depende de Campeonato, porque el número de la fecha se reinicia en cada campeonato y no es único por si solo.
- Equipo: Entidad fuerte, ya que tiene su identificador propio (rut_equipo) y existe independientemente de los campeonatos o partidos que participe.
- Partido: Entidad debil, ya que depende de la Fecha, el número de partido se reinicia dentro de cada fecha y no es un solo partido por si solo.
- Estadio: Entidad fuerte, ya que tiene su propio identificador (id_estadio) y puede existir sin estar asociado a ningún equipo o partido en particular.
- Arbitro: Entidad fuerte, ya que tiene un identificador natural propio (rut) y existe independientemente de los partidos que le toque dirigue.
- Gol: Entidad debil, ya que esta depende netamente del Partido: no tienen ningun atributo que sea fuera del contexto del partido.
- Tarjeta: Entidad debil, es debil por la misma razón que el gol, no existe fuera del partido y depende de este para poder identificarse.
- Jugador: Subclase de Integrante, ya que hereda sus atributos base y agrega los propios de Jugador como sueldo y posicion.
- Integrante: Superclase, ya que contiene los atributos comunes (rut_pasaporte, nombre, fecha_nacimiento, nacionalidad) que pertenecen a Jugador e Integrante Cuerpo Tecnico, los cuales son los unicos también que pueden recibir tarjetas y sanciones del tribunal
- Integrante Cuerpo Tecnico: Subclase de Integrante, porque hereda los atributos de Integrantes pero agregrando su atributo cargo.
- Miembro tribunal: Entidad fuerte, ya que tiene su propio identificador (rut) y existe independientemente de la sala a la que pertenezca.
- Sala tribunal: Entidad fuerte, ya que tiene su propo identificador (id_sala) y no depende de ninguna otra entidad para poder existir. 
- Dirigente: Entidad fuerte, ya que posee su propio identificador (rut) y existe independientemente al equipo que lo represente.
- Sentencia: Entidad debil, ya que este depende de Sala tribunal su numero de sentencia solo tiene sentido si este es dado en la sala que la dicto.

### Las llaves primarias fueron:
- (Campeonato)id_campeonato, (Equipo)rut_equipo, (Estadio)id_estadio, (Arbitro)rut, (Integrante)rut_pasaporte, (Miembro Tribunal)rut, (Sala Tribunal)id_sala, (Dirigente)rut

### Las llaves parciales fueron:
- (Fecha)numero_fecha, (Partido)numero_partido, (Gol)numero_gol, Tarjeta(numero_tarjeta), (Sentencia)numero_sentencia.

### Cardinalidades:
#### Para la página 1:
- programa - Campeonato (0,N) - Fecha (1,1): Ya que cada campeonato programa una cierta cantidad de fechas a lo largo del torneo, mientras que cada fecha pertenece siempre a un único campeonato, y sabes que no existe una fecha libre sin un torneo asociado, por lo que su participación total (1,1)
- participa - Equipo (0,N) - Campeonato (1,N):Un equipo no puede participar en un campeonato sino participar en algunos o en todos, de ahí el minimo 0 en Equipo, y cada campeonato necesita un conjunto de equipos inscritos para poder disputarse, pudiendo variar de una edición a otra.
- sale_campeon - Campeonato (0,1) - Equipo (0,N): Porque en todo campeonato siempre hay un campeón, pero mientras el torneo está en curso aún no existe(0), y una vez terminado queda fijado a exactamente un equipo (1), un equipo por su parte , puede ser campeón de varias ediciones a lo largo del tiempo o no haber ganado nunca (0,N)
- programación- Fecha (0,N) - Partido(1,1): Contiene la mimsa lógica que programa para cada fecha agenda una cierta cantidad de partidos y entre los equipos y cada partido pertence siempre a una única fecha del campeonato, ya que no puede jugarse un partido el cual no se encuentre programado.
- juega_local - Partido (1,1) - Equipo (0,N): Todos los partidos tienen siempre un equipo que juega de local, y un mismo equipo actúa como local en diferentes partidos a lo largo del campeonato.
- juega_visita - Partido (1,1) - Equipo (0,N): Todos los partidos tienen siempre un equipo que juega de visitante, y un mismo equipo actúa como visita en diferentes partidos a lo largo del campeonato.
- sede - Equipo(3,N) - Estadio (1,N): Porque cada equipo tiene un conjunto de al menos 3 estadios, por eso ponemos como mínimo 3 en Equipo y estadio puede ser usado por distintos equipos por eso el (1,N)

#### Página 2:
- Designa - Partido (7,7) - Arbitro (0,N): Porque la regla nos dice que necesitamos arbitro principal (1) + 2 jueces de línea + 1 asistente + 3 VAR lo cual nos da un total de 7 para cada partido, mientras que el un árbitro puede dirigir muchos partidos a lo largo del tiempo, o también  ninguno si recién se viene registrando.
- da - Partido(0,N) - Tarjeta (1,1): Un partido puede terminar sin ninguna tarjeta dada o con varias, y cada tarjeta pertenece siempre a un único partido y esta no existe fuera del contexto de partido.
- mete_gol - Partido (0,N) - Gol(1,1): Un partido puede terminar empatado a 0 goles o con varios goles, y cada gol peretence a un único partido, ya que este se identifica por el minuto dentro de ese partido en específico.
- recibe - Tarjeta(1,1) - Integrante (0,N): Cada tarjeta identifica exactamente a un jugador o un integrante del cuerpo técnico que la recibió, y una misma persona puede acumular varias tarjetas a lo largo de distintos partidos, o puede no recibir ninguna.
- convierte - Gol(1,1) - Jugador (0,N): Cada gol tiene siempre un único jugador que lo anota, y un jugador puede meter muchos goles durante el partido, o ninguno.
- convocado - Partido (34,34) - Jugador (0,N): El número exacto de los convocados son 11 titulares + 6 suplentes y esto para los dos equipos presentes, un jugador puede ser convocado a muchos partidos a lo largo del campeonato o ninguno.
- participa_en - Partido (10,10) - IntegranteCuerpoTecnico(0,N): Cada partido involucra un número de cuerpo técnico (1 entrenador + 2 ayudantes + 1 médico + 1 preparador físico) todo esto por un equipo, y un integrante del cuerpo tecnico puede participar en muchos partidos del equipo o en ninguno.

#### Página 3:
- integra- MiembroTribunal (1,1) - SalaTribunal (7,7): Cada integrante del tribunal pertenece a una única sala, y cada sala se encuentra integrada por un número fijo de personas (1 presidente + 1 secretario + 5 abogados).
- dicta - SalaTribunal (0,N) - Sentencia(1,1): Una sala puede haber dictado muchas sentencias a lo largo del tiempo, o ninguna si esque es una sala nueva, y cada sentencia es dictada siempre por una única sala.
- secumple_en - Sentencia(1,1) - Campeonato(0,N): Siempre un único campeonato en el cual se debe cumplir el castigo, un mismo campeonato puede acumular muchas sentencias distintas, o ninguna si esque aún no se le han dado castigos
- castiga_a - Sentencia(1,1) - Integrante (0,N): Cada sentencia solo castiga a una única persona puede ser jugador o integrante de cuerpo tecnico, y una misma persona puede acumular varias sentencias a lo largo de su carrera futbolistica o puede no tener ninguna sentencia.
- trabaja_en - IntegranteCuerpoTecnico (1,1) - Equipo (6,6): Cada integrante del cuerpo tecnico trabaja solo para un único equipo, y cada equipo tiene siempre una cantidad fija de cuerpo tecnico (1 DT + 2 asistentes + 1 médico + 1 preparador físico + 1 utilero).
- pertenece - Jugador(0,N) - Equipo (0,N): Esta relación modela el historial de pertenencia de un jugador a distintos equipos a lo largo del tiempo, un jugador puede no pertenecer a ningún equipo o haber pasado por varios a lo largo de su carrera, y un equipo puede tener en su historial a muchos jugadores o ninguno si recién se esta creando.
- representa - Dirigente(0,N) - Equipo (0,N): Un dirigente puede representar a distintos equipos en diferentes periodos a lo largo de su carrera, y un equipo puede haber tenido varios grupos de dirigentes a lo largo del tiempo

### Jerarquías:
- Se utilizó una jerarquía de generalización/especialización porque tanto los jugadores como los integrantes del cuerpo tecnico comparten caracteristicas, los mismos atributos de identificación de persona (rut_pasaporte, nombre, fecha_nacimiento, nacionalidad), y ambos pueden recibir tarjetas y ser castigados por el tribunal de disciplina. Sin esta jerarquía, las relaciones recibe y castiga_a tendrían que duplicarse: una versión hacia jugador y otra igual para IntegranteCuerpoTecnico, repitiendo la misma semántica dos veces. Con integrante como superclase, ambas relaciones se modelan solo una vez, hacia la superclase, y cada subclase hereda esa capacidad automáticamente.
- Es disjunta (d): Una persona no puede ser jugador e integrante del cuerpo tecnico al mismo tiempo ya que estos son roles excluyentes entre sí. Por eso el triangulo ISA lo etiquete con d-
- Es total: Todo integrante que pasa por esta jerarquía pertenece a una de las subclases, ya que no existe "Integrante" sin rol definido, siempre es jugador o cuerpo tecnico.

