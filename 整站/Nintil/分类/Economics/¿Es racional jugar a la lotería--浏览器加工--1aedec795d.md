Existe cierto debate sobre este tema

Por un lado, tenemos el argumento clásico a favor de la tesis de irracionalidad: Si se atiende a criterios puramente monetarios, y se asume que el jugador quiere maximizar la esperanza estadística de sus ganancias, ciertamente es irracional.

Asumamos una lotería simple donde existe una probabilidad p de ganar una cantidad Q. El coste de la lotería es C. La utilidad esperada de esta lotería es pues U(Q,C,p)=p\*Q-C

Normalmente p es muy pequeña, Q es muy grande y C es relativamente pequeña. Imaginemos una lotería donde participar cuesta 10€, y el premio es 1 millón de €. La probabilidad de ganar es de p=10^-9. Multiplicando, U(10^6,10,10^-9)=10^-3-10=-9.99. Luego lo más plausible es que perdamos dinero jugando a esta lotería si jugamos una vez. Si jugamos un millón de veces a esta lotería, lo normal es que perdamos 1 millón de euros. Sí, por poder quizá nos toque un millón la primera vez. Pero poder poder quizá no. Es como tener una pared enorme donde una diana minúscula y pagar para que un mono lance dardos al azar a ver si acierta.

Ahora bien, más apropiado que esto es tener en cuenta una idea de maximización de la utilidad _subjetiva_ esperada. Aquí podemos introducir otras consideraciones:

El beneficio psíquico de ganar la lotería S, el coste psíquico de perderla si otros con el mismo número (familiares) la ganan, P. Entonces, U(Q,C,S,P,p)=p\*(Q+S)-C-P(Perder|Familia gana)\*P

Esta ecuación puede resultar positiva subjetivamente, y entonces será racional jugar a la lotería. La probabilidad de perder si la familia gana será uno si la familia ha jugado a la lotería y será cero si no lo ha hecho, luego alguien cuyo entorno social no sea fan de la lotería no tendrá P en su ecuación. Si asumimos que es cero, para que resulte racional jugar a la lotería, debe ser el caso que p\*(Q+S)>C, es decir, S>C/p -q~10^10. Básicamente, la satisfacción subjetiva de jugar a la lotería, descontado la satisfacción que genera el tener el millón de euros, tiene que ser equivalente a la de ganar 10.000 millones de euros. Personalmente, me cuesta pensar que alguien pueda sentir una satisfacción tal por ganar el millón de euros. Técnicamente es posible, así que para esa gente que tenga una función de utilidad muy rara, como recuerda en twitter Arturo Goosnargh

https://twitter.com/ArturoGoosnargh/status/684847964657987584

Para la mayoría de la gente, intuyo que juegan a la lotería porque no hacen el cálculo explícito de costes y beneficios, no porque tengan un beneficio psicológico por encima del monetario astronómico. También, por presión social o inercia. En grandes comidas familiares a vece se vende lotería y muchos comprarán ahí para señalizar su pertenencia al grupo familiar, como una suerte de rito colectivo.

En Rasgo Latente, un blog de psicología se plantean esta misma cuestión aquí: [¿Es la lotería una estafa?](http://rasgolatente.es/loteria-estafa-juego-azar-irracional/) y tratan argumentos similares a los aquí expuestos.

Consideremos [la lotería Powerball](http://uk.businessinsider.com/powerball-lottery-expected-value-2016-1?r=US&IR=T), donde el premio máximo es de 450 millones de dólares (Descontando impuestos). Aquí podemos hacer un cálculo más completo de lo que supone jugar a una lotería de verdad. No hay una probabilidad, sino varias, pues hay varios premios, de modo que

U=sum(p\_i\*(Q+S)\_i)-sum(C\_i)-sum(P(Perder|Familia gana)\*P\_i)

Por simplificar, asumamos P(Perder|Familia gana)=0 (Alguien que no es envidioso). Entonces,

U=sum(p\_i_S)-sum(C\_i)+sum(p\_i_S\_i)>0

sum(p\_i\*S\_i)>0.14

Aquí podríamos decir que esa felicidad enorme de ganar la lotería sólo la tenemos cuando ganamos una cantidad sustancial de dinero, limitando el cálculo a los premios grandes de 50k,1M y 450M dólares. Asumiendo la misma S,

S>0.14/(0.0000010951+0.0000000856+0.0000000034)

S>118.233$

Esta es la satisfacción subjetiva que necesitaríamos para racionalizar el jugar a esta lotería. No es _tan_ absurdo como jugar a la lotería propuesta por mí anteriormente, pero sigue pareciendo increíble que la satisfacción sea tan alta. Y aquí estoy siendo generoso: Asumo que si ganas el premio de 50.000 dólares tendrás ese beneficio, más un beneficio psíquico del doble de la cantidad que has ganado.

Si introdujésemos el caso donde la famila gana algo y queremos asegurarnos contra la desutilidad de la envidia, tendríamos que estar valorando esa envidia bastante, cosa que, quizás podría argumentarse, sea inconsistente con nuestras otras reacciones cuando otros ganan dinero. Si la motivación para jugar a la lotería es un seguro contra la envidia futura, ¿No es más sano tratar de cambiar nuestra forma de ser y dejar de ser envidiosos?

Si tras leer este artículo sigues creyendo racional jugar a la lotería, y has pensado seriamente sobre el cálculo de probabilidades y beneficios para tu caso particular, para ti es racional jugar a la lotería. En caso contrario no lo será.

Para finalizar, me gustaría invitar a los lectores a tratar de argumentar, si juegan a la lotería, el por qué lo hacen, para poder entender mejor esta realidad.