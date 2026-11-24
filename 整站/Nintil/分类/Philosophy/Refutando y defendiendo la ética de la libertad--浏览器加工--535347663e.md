Hace unos meses, [esto que escribí](https://artir.wordpress.com/2015/09/08/sobre-normas-universales-simetricas-y-funcionales/) recibió una crítica [aquí](https://www.juandemariana.org/ijm-actualidad/analisis-diario/sobre-la-etica-de-la-libertad-respuesta-jose-luis-ricon). No me convenció la crítica, y aquí explicaré por qué. Recomiendo leer antes el primer artículo y la crítica antes de leer este, pues aquí iré citando de la réplica y respondiendo, sin repetir lo que ya dije.

Argumentaré que la defensa de la ética de libertad de Francisco Capella, aún conduciendo a conclusiones plausibles y estando en gran medida de acuerdo con ellas, es en última instancia errónea, poco rigurosa, y falaz,  siendo la formulación de Rallo más plausible.

## Índice

1.  Universalidad y simetría
2.  Funcionalidad
3.  Algunos contraejemplos
    1.  Los aversos a la desigualdad
    2.  El meteorito
4.  Contratos
5.  Pragmatismo
6.  Intuitions all the way down
7.  Formulando la ética de la libertad
    1.  La ética de la libertad
    2.  Argumentando la ética de la libertad
    3.  Errores en el argumento
8.  Capella debate con sus críticos
9.  Conclusión
10.  Apéndice: Universalidad fuerte

> Artir acepta universalidad y simetría pero se pregunta qué entiendo por funcionalidad, sin aclarar qué entiende él por este término.

Justamente mi artículo resalta la importancia de la definición de funcionalidad, porque pueden darse muchas. Mi punto no es que su definición no sea buena y que haya que usar la mía (no dí ninguna, de hecho), sino que Capella tiene que explicitar qué se entiende por funcionalidad cuando hace razonamientos éticos. Cosa que hace:

> La funcionalidad y la eliminación  de conflictos son para mí equivalentes (mejor evitar, minimizar y resolver en la medida de lo posible).

Para los nórdicos, parece admitir que tienen normas funcionales, pero que no son universales ni simétricas,

> Los países nórdicos funcionan razonablemente bien pero no aplican normas universales ni simétricas, y de hecho probablemente funcionan bien, al menos en parte, por ser pequeños y homogéneos y tener más fácil alcanzar un consenso social.

Comparto este párrafo. Los nórdicos (_ni ningún sistema político vigente_) tienen normas del todo universales, simétricas y funcionales. Pero podemos tratar de buscar qué normas universales, simétricas y funcionales, de forma idealizada, aproximan las normas actualmente existentes en esos países.

Un primer ejemplo, siguiendo a [Scott Sumner](http://econlog.econlib.org/archives/2014/07/beyond_left_and.html) es el utilitarismo. Otro, una ética de inspiración Rawlsiana.  Otros ejemplos plausibles, que no cuadran con los Nórdicos, pero que cumplen con las condiciones de Capella son el egoísmo ético, la deontología kantiana y la ética de la virtud aristotélica. Algo en lo que no caí cuando leí su réplica en su día es que los tres criterios propuestos para la ética son cumplidos por todas las teorías éticas con cierto renombre. Luego aunque sean necesarios, no son suficientes. A continuación explicaré por qué.

Por recordar, las tres propiedades de las que se hablan constantemente:

-   Una norma es universal si aplica a todo agente
-   Una norma que obliga a un sujeto A con otro B a (no) hacer Z es simétrica si también se acepta la misma norma, pero que obligue a B con A a (no) hacer Z
-   Una norma es funcional si cumple criterios de funcionalidad

## Universalidad y simetría

Las dos primeras cualidades no parecen necesitar de mucha más discusión: sus requisitos son casi matemáticos, y podríamos incluso representarlos en lógica de predicados:

Tenemos sujetos éticos x, y luego un operador de construcción de normas N(x,M) donde x es un sujeto ético e y M es un mandato (prohibido(P), obligado(O),etc). N(Artir,O(Artir,Correr)) significa que existe una norma que me aplica a mí y que me obliga a correr.

Con esto dicho, una norma es universal si:

> (∀x)(∃N(x,M))

Es decir, que para todo sujeto, existe la norma definida por el mandato M y aplica a todos los sujetos. Nadie puede matar podría escribirse (∀xi)(∀xj)P(xi,xj) donde P es una prohibición de que xi no mate a xj.

Capella ha dicho en otras ocasiones que la universalidad y la simetría están estrechamente vinculadas. Aquí está el por qué: Como xi y xj pertenecen al mismo conjunto (todos los agentes éticos), al final xj está obligado respecto a xi a lo mismo que esté obligado xi respecto a xj.

¿Cuál sería una norma no universal? "Yo tengo prohibido matar a cualquiera", o (∀xj)N(Yo,P(Yo,xj)). Está definida M para todo agente M? (∀x)(∃N(x,P(xi,xj))) ? No, sólo para uno, (yo), por tanto no es universal. Para que fuese universal, todos tendrían que tener prohibido matar a todos.

Puede haber normas que sí sean universales, pero que no sean simétricas. "Está prohibido matar a los judíos" es una norma universal, pero no es simétrica. Todos los agentes tienen una obligación no respecto a todos los agentes, sino respecto a un subconjunto de ellos: los judíos.

Simetría podríamos definirla como

> (∀xi)(∀xj)(N(xi,M(xi,xj))->N(xj,M(xj,xi)))

Esto es, que si tenemos una norma N que aplica a un agente xi que dice que existe la relación M entre los agentes xi y xj debe existir también una norma que aplique al agente xj que diga que exista la relación M entre los agentes xj y xi.

En el caso de "Estar prohibido matar a los judíos", (∀x)N(x,P(x,judíos)). Aquí, no es el caso que la norma sea simétrica. Para que lo fuese necesitaríamos (∀judíos)N(judíos,P(judíos,x)), que los judíos también tengan prohibido matar a los demás. Lo que al final resulta que todos tienen prohibido matar a todos.

Entonces nos preguntamos ¿Pasan estos tests el utilitarismo, el kantianismo, el rawlsianismo y el egoísmo ético?

-   El utilitarismo puede expresarse como "Todo x está obligado a llevar a cabo la acción A que[maximice la suma del bienestar](http://plato.stanford.edu/entries/consequentialism/) de todos los x. (∀x)(N(x,O(x,A\\max(sum(W(x)))).
-   El kantianismo puede expresarse como "Todo x debe actuar según las máximas que puedan racionalmente exigirse que sean [una ley universal](http://plato.stanford.edu/entries/kant-moral/)" (∀x)(∀N)N(x,M)\\U(N(x,M))
-   El rawlsianismo se puede formular de dos maneras. La primera "Todo agente está obligado a actuar según los principios que se decidirían tras un velo de ignorancia" (∀x)(∀N)N(x,M)/V(N(x,M))
-   O directamente, yendo a los dos principios de Rawls:
    -   Todo xi tiene los derechos D ante xj (D Es un conjunto variado de obligaciones y permisos) (∀xi)(∀xj)(N(xi,D(xi,xj))
    -   Todos están obligados a llegar a cabo aquellas acciones que resulten en a distribución de propiedades debe ser aquella que maximice el bienestar del agente social más desfavorecido, tal que el agente no viole los derechos D de nadie. (∀x)N(x,O(x,A\\PR\\max(min(W(x))\\O(x,D)) (W es una función vectorial que recoge el bienestar de cada agente)
-   El egoísmo ético se puede definir como que está prohibido actuar de forma altruista, o como que es obligatorio actuar según tu propio interés, (∀x)N(x,P(x,Altruismo))

Antes de estudiar la funcionalidad, ¿Son estas normas universales y simétricas?

El utilitarismo, el kantianismo y el egoísmo ético claramente: sólo incluyen cuantificadores universales (Hablan de todos los x, no de un x en particular que sería xi), luego es trivial que son universales y simétricas. El velo de ignorancia rawlsiano también es universal y simétrico. La estructura es idéntica a la de Kant. Por algo Rawls se definía como un constructivista kantiano ([Rawls, 1980](http://philpapers.org/rec/RAWKCI)). El rawlsianismo formulado según sus dos principios también cumple. El primer principio tiene la misma estructura de la definición de simetría, y está definido para todo agente, luego es universal. El segundo principio también está definido para todos los agentes. ¿Pero es simétrico? Lo es, aunque parezca que ese agente desfavorecido está privilegiado. Un caso concreto de esa regla es que yo estoy obligado a llevar a cabo acciones que maximicen el bienestar de agente social más desfavorecido. Pero la regla simétrica existe: el desfavorecido también está obligado a llevar a cabo acciones que maximicen el bienestar del agente social más desfavorecido, que es él mismo. ¿No sería la regla simétrica que el agente desfavorecido debe hacer cosas para maximizar el bienestar del resto de agentes?  Nup. Y es importante ver por qué: El agente menos desfavorecido no es un agente en concreto, como quien dice 'Pedro' o 'Paco' o 'Las mujeres', sino que está definido como 'aquel agente que esté peor'. Quizá sea yo, o quizás seas tú, o el vecino.

Podría replicárseme que si aceptamos esto, abrimos la puerta a decir que "Todos tienen permitido asesinar mujeres" si lo enunciamos como "Aquellos agentes que coincida que tengan dos cromosomas X". Esta norma así expresada sería universal y simétrica. Trato esto en el apéndice final.

Otro ejemplo más sería "Si A tiene muchos más recursos que el resto de agentes, A está obligado a dar recursos al resto de agentes." Esta podría considerarse la norma de los socialdemócratas, y es también simétrica, universa, y funcional. (Funcional para los socialdemócratas, claro). Capella no podría argumentar que la norma no es válida por no ser precisa, pues si dice que 'muchos recursos' no es preciso, caerá en el problema de que 'agresión' tampoco lo es, y socavará su propia teoría. Sí podría argumentar que no es del todo universal, pues habla de algo concreto (muchos recursos, ricos, pobres). Volveré a esto al final de este artículo.

En cualquier caso, la justificación de los principios de Rawls es que salen del velo, y eso sí cumple con universalidad y simetría, y aunque finalmente no fuera así, con que exista al menos un contraejemplo me vale.

## Funcionalidad

Aquí está la cuestión más interesante

En la cita anterior, Capella decía que una posible razón por la que los Nórdicos funcionen bien es que son pequeños y tienen poblaciones homogéneas. Efectivamente hay evidencia de que poblaciones homogéneas étnicamente producen más bienes públicos ([Habyarimana et al. 2007](http://www.columbia.edu/%7Emh2245/papers1/HHPW.pdf)), pero no de que el tamaño de los países esté relacionado con su crecimiento económico a igualdad de libertad económica ([Alesina 2003](https://dash.harvard.edu/bitstream/handle/1/4551794/alesina_size.pdf?sequence=2)). En cualquier caso la cosa es que parece que Capella admite que los Nórdicos son países funcionales.

> Aunque las normas no bastan, son muy importantes: el análisis empírico muestra que la mayor parte de la riqueza de las naciones está en su capital social, en sus instituciones formales e informales que fomentan la confianza, facilitan las interacciones cooperativas, minimizan costes legales de transacción y evitan malgastar recursos en protegerse unos de otros. Evitar, minimizar y resolver conflictos en la medida de lo posible es muy funcional. Es imposible garantizar que un sistema tiene respuestas perfectas para todo, pero algunos las tienen muy buenas.\[...\] La funcionalidad tiene que ver con la eficiencia económica si esta se entiende como conseguir el máximo de lo que la gente quiere, pero es cierto que no es sólo eso. La funcionalidad de las normas es conseguir una convivencia lo más armoniosa posible evitando, minimizando y resolviendo conflictos, que son lo contrario a la armonía. Las normas no garantizan por sí solas la prosperidad o el progreso económico, el cual depende de que los individuos actúen y lo hagan de forma inteligente: las normas más liberales no van a conseguir el enriquecimiento de una sociedad de vagos incompetentes, o el avance material de un grupo de ascetas o místicos dedicados a la contemplación.

Capella dice por tanto que lo que entiende por funcional es "Normas que minimicen conflictos, que consigan una convivencia lo más armoniosa posible", y admite que es posible que las normas no conduzcan a una sociedad próspera si se tiene "Una sociedad de vagos incompetentes".

¿Y qué entendemos por lo anterior? Una primera interpretación sería entender conflicto como violación de la ética de la libertad, pero esto sería una tautología vacía, decir que la ética de la libertad es válida porque es funcional, y que es funcional porque la ética de la libertad proscribe violaciones de la ética de la libertad.

Podríamos entender, más plausiblemente que son aquellas normas que conducen a un menor índice de conflicto, integrando en este homicidios, atracos, violaciones, violaciones de contratos o algo así. Entonces sería una cuestión empírica determinar esto, pero prima facie no parece que una norma alternativa necesariamente no conduzca a un menor número de este tipo de crímenes.

Capella añade más cosas a funcionalidad, no obstante. [Aquí dice](https://www.juandemariana.org/ijm-actualidad/analisis-diario/malas-criticas-contra-el-liberalismo):

> Sin embargo el problema de este sistema \[Uno donde los derechos de propiedad están limitados a lo que maneja un agente en un momento dado\] está en lo disfuncional que sería su aplicación: los agentes no podrían hacer planes mínimamente complejos con la seguridad legal de que cuentan con los recursos necesarios, y la producción o elaboración de bienes duraderos o bienes de capital sería extremadamente limitada al no poder los agentes mantener su control en el tiempo y beneficiarse de las inversiones realizadas. Sería muy mala idea no legitimar el uso de la fuerza para defender la propiedad duradera de objetos que no están en uso permanente. Los individuos no sólo quieren disponer de bienes en el momento en que los usan de forma efectiva, sino que necesitan saber que van a estar disponibles en el futuro: así se fomenta la producción, la acumulación y el intercambio de riqueza, y muy especialmente se facilita la coordinación de los planes de acción. Para ser completo el análisis ético debe considerar todas las normas imaginables, pero una vez estudiadas puede rechazar aquellas que resultan absurdas, destructivas o disfuncionales, como es este caso de propiedad solamente por uso directo. Es posible imaginar mundos sin propiedad o con formas extrañas de la misma, pero resultan ser malas ideas.

Bruenig ha encontrado unas normas que son universales y simétricas (Uno sólo tiene derecho de propiedad sobre su cuerpo y lo que está tocando), pero le critica que ese sistema ético no está bien definido:

> Este sistema además no está bien definido, ya que los bienes pueden tocarse sin usarse y usarse sin tocarse, y es posible que varios agentes toquen algo a la vez y entonces no está claro quién tiene mejor derecho.

Y que no es funcional, entendiendo como parte de la funcionalidad aquí como que condena al subdesarrollo económico. Nótese que aquí no habla de que no minimicen conflictos: lo hacen. Sino que añade además un requisito de que permitan cierto desarrollo económico, y de que no resulten absurdas, destructivas o disfuncionales.

Funcionalidad es pues para Capella, finalmente, aquellas normas que no son absurdas, destructivas o disfuncionales, que permiten cierto desarrollo económico, y que minimizan los conflictos.

Es crucial algo así: Capella está construyendo su sistema a partir de dos principios que no dependen de observaciones empíricas (universalidad y simetría) y uno que sí (funcionalidad). Si sólo se deduce a partir de principios a priori, se puede caer en el error en el que cae Rawls, donde propone instituciones para lograr directamente un fin, cuando es el caso que tratar de lograr directamente ese fin es peor que intentar lograrlo indirectamente ([Brennan 2007](http://philpapers.org/rec/BRERP)).

Sabemos empíricamente que los principios de la ética de la libertad sí permiten el crecimiento económico. No permiten, creo, el máximo crecimiento económico posible: puede haber situaciones donde una intervención coercitiva (expropiar terrenos para una autopista, reformas agrarias, R&D pública ) motive un crecimiento económico más rápido. Capella no obstante, no ha afirmado nunca que la libertad necesariamente sea equivalente a la maximización de la eficiencia económica ([Robin Hanson](http://econfaculty.gmu.edu/bcaplan/handeb.htm) defiende que el objeto de la ética es la maximización de la eficiencia económica), y probablemente se conforme con el nivel de crecimiento que permite la ética que propone.

Volviendo a las diversas éticas, Capella podría aceptar que efectivamente son simétricas y universales, pero no funcionales, pero

-   El utilitarista querrá definir funcional como 'aquello que maximiza la utilidad', y definirá el resto de éticas como disfuncionales por no hacerlo. Si algo llevase a consecuencias absurdas, el utilitarismo lo rechazaría, pues sólo se preocupa de las consecuencias.
-   El kantiano incluye su propio criterio de funcionalidad en la propia norma: Si uno propone como máximas reglas universales que se contradigan en cierta forma, deben rechazarse. Por ejemplo, si se propone la regla 'Permitido robar', eso conducirá a un mundo donde termine por no haber propiedad privada, y por tanto nada que robar. O 'Permitido mentir', si mientes para salvar[a un amigo de un asesino](http://slatestarcodex.com/2014/05/16/you-kant-dismiss-universalizability/), y esa máxima fuese universal, la sociedad colapsaría y no habría posibilidad de mentir más ([Varden 2010](http://onlinelibrary.wiley.com/doi/10.1111/j.1467-9833.2010.01507.x/pdf)). Al igual que Capella, a Kant no le preocupa en su análisis la ética personal de cada uno o lo virtuoso que una pueda ser, sino puramente del derecho, de lo que es respaldable con la fuerza (A lo que Capella llama ética). El punto aquí no es defender a Kant, sino exponer lo que un Kantiano entiende como funcional. Nótese también las similitudes entre la ética kantiana y la de Capella: ambas se construyen sobre principios similares, y no admiten excepciones, por terribles que puedan parecer las consecuencias.
-   El rawlsiano propone imaginar a agentes tras un velo de ignorancia con conocimiento pleno de las ciencias físicas y sociales. El velo garantiza universalidad y simetría, y lo agentes no escogerán normas disfuncionales, porque van a vivir bajo las normas que están escogiendo.
-   El egoísta ético, entendido éste más específicamente como el [Yangismo](https://en.wikipedia.org/wiki/Yangism), defenderá que una sociedad que restrinja al propio interés individual es censurable, pues obliga al individuo a ser altruista. Una sociedad donde exista altruismo no será todo lo funcional que debiera. Aunque los Yangistas tienden a ver la funcionalidad del sistema como conducente al bienestar del individuo, no deja de ser cierto que no es compatible con la idea de derechos. Quizá generalmente sea el caso que a alguien le beneficie actuar como si respetase ciertos derechos ajenos, pero en otras ocasiones no, y ahí coartar la violencia ajena (si no está en nuestro propio interés) sería disfuncional.

Con esto concluyo que toda ética viable es universal, simétrica y funcional, dentro de las definiciones de funcionalidad (todas ellas plausibles) de cada ética.

## Algunos contraejemplos

### Los aversos a la desigualdad

> Sobre el ejemplo de la sociedad aversa a la desigualdad: si la sociedad es toda ella aversa a la desigualdad la evitará de forma espontánea y sin necesidad de violencia. Si sólo parte de la sociedad es aversa a la desigualdad y si las normas consiguen igualdad privilegiando a unos frente a otros, entonces ya no son ni universales ni simétricas. No vale tratar la funcionalidad y olvidar el otro aspecto esencial de las normas éticas.

Ciertamente. Lo que ocurrirá será que instituirán normas de redistribución de la renta, y en su definición de funcionalidad incluirán la igualdad. Como ya hemos visto, es trivial construir normas que sean universales y simétricas.

### El meteorito

> Para reforzar su crítica y diferenciar funcionalidad y evitar conflictos, recurre a un ejemplo de David Friedman: ¿robas algo por valor de cien dólares que puede salvar a toda la humanidad o dejas que la humanidad perezca? David Friedman es un gran pensador, pero este ejemplo (al que desgraciadamente se recurre muy a menudo en versiones equivalentes) es muy malo, por irreal, forzado y mal razonado, mezclando en la noción de valores lo que son reglas (la propiedad) y lo que son preferencias (violar o no las reglas, que la humanidad perezca o no). El ejemplo ignora que las normas no dicen sólo no robar (que parece algo muy rotundo, solemne, claro y fácil de entender, pero que es esencialmente incompleto), sino que avisan de que el robo será castigado con una pena y compensación a la víctima. Si por cualquier motivo, quizás excepcional, en una situación dada el beneficio que se obtiene al violar la norma compensa el castigo, hágase: tal vez incluso la víctima del robo esté de acuerdo con ello, en cuyo caso no habría ningún problema. Yo por ejemplo deseo que me roben, y no pienso protestar, si tengo garantizado que me van a compensar rápidamente y sin riesgos ni molestias con mucho más de lo que me han robado: los ladrones me harán rico.

A Capella el argumento le parece "muy malo, por irreal, forzado y mal razonado, mezclando en la noción de valores lo que son reglas (la propiedad) y lo que son preferencias (violar o no las reglas, que la humanidad perezca o no).

El ejemplo es bien sencillo, y extraña que alguien que hable de lógica y rigor no haya caído en la validez formal de algo tan elemental como convertir un modus ponens en un tollens:

1.  P->Q
2.  P
3.  Por tanto Q

Pero también  se puede decir

1.  P->Q
2.  ¬Q
3.  Por tanto ¬P

Aquí P es la ética de la libertad, y Q es que no es ético robar para evitar el asteroide. Lo que dice Friedman simplemente es que uno puede hacer ambos argumentos, y que uno concluye que, efectivamente, no es ético robar para salvar a la humanidad, y otro concluye que sí que lo es (y la ética de la libertad debe ser revisada). O bien, que quizá la implicación es falsa y la ética de la libertad sí permite robar en la situación del asteroide. Bien razonado está. Irreal sí es, desde luego. Pero la falta de realismo nunca ha sido un problema en Filosofía a la hora de proponer experimentos mentales. Esto a Rothbard le molestaba ([Rothbard 1998, cap 20](http://anarcho-capitalist.org/wp-content/pdfs/Rothbard%20\(Murray\)%20-%20The%20Ethics%20of%20Liberty.pdf)).

Scott Alexander explica el por qué es apropiado usar este tipo de situaciones:

> Moral dilemmas are extreme and disgusting precisely because those are the only cases in which we can make our intuitions strong enough to be clearly detectable. If the question was just “Which is worse, a thousand people stubbing their toe or one person breaking their leg?” neither side would have been obviously worse than the other and our true intutition wouldn’t have come into sharp relief. So a good moral philosopher will _always_ be talking about things like murder, torture, organ-stealing, Hitler, [incest](https://www.psychologytoday.com/blog/experiments-in-philosophy/200804/what-s-the-matter-little-brothersister-action), [drowning children](http://www.utilitarian.net/singer/by/199704--.htm), [the death of four billion humans](http://lesswrong.com/lw/tn/the_true_prisoners_dilemma/), et cetera. ([Extremism in Thought Experiment is no Vice, 2015](http://slatestarcodex.com/2015/03/26/high-energy-ethics/))

A Capella le digo lo mismo que le dice Caplan a Hanson. La teoría de Hanson dice que el Holocausto estaría justificado si un número suficiente de Nazis está dispuesto a pagar más de lo que lo estarían los judíos por comprar su vida,

> When I point out that Robin's theory implies that the rightness of the Holocaust depends on the number of Nazis, then, I'm not trying to smear him as a Nazi sympathizer.  I'm saying that his theory has crazy implications, so his theory is false and he should revise it.  End of story. ([Are grotesque hypothetical cheap shots?](http://econlog.econlib.org/archives/2009/04/are_grotesque_h.html#))

Por último, Capella dice que esto mezcla reglas y preferencias. Yo prefiero hablar que mezcla intuiciones formales y abstractas y otras intuiciones menos abstractas. Para Capella, la ética de la libertad no es verdadera ni falsa. Él entiende la ética como órdenes. La ética de la libertad es una forma breve de decir "No mates!,Cumple los contratos! Si algo no es de nadie, es propiedad de otro, y no puedes violarla!". Las órdenes no son proposiciones, y por tanto no puede hablarse sobre su verdad. En este sentido es parecido a la metaética de R.M. Hare, el [prescriptivismo universal](http://plato.stanford.edu/entries/moral-cognitivism/#PreUniPre). Sólo que Hare usa un procedimiento similar al de Capella para derivar no el liberalismo, sino el utilitarismo de preferencias.

Insisto en lo anterior, y volveré sobre ello luego, para Capella 'Matar está mal' carece de sentido, es un error. 'Matar está prohibido' es la interpretación más plausible que le da él a lo que la gente se refiere cuando dice eso.

En cualquier caso, dado que ya hemos visto que hay múltiples éticas que cumplen con los criterios, hace falta algo más para elegir entre ellas. Ahí entran nuestras concepciones de lo que es funcional. Permitir la muerte de la humanidad por no robar 100$ es absurdo y disfuncional, por tanto la ética de la libertad no vale. Incluso Robert Nozick aceptó que su ética, que es básicamente la misma que la que plantea Capella, [permite ser vulnerada](http://www.iep.utm.edu/noz-poli/) en "situaciones de horror moral".

[Sasha Volokh](http://volokh.com/2011/02/15/asteroid-defense-and-libertarianism/), de blog jurídico The Volokh Conspiracy, que si bien acepta impuestos  para financiar instituciones de protección de derechos, acepta el dilema que supone el ejemplo del asteroide, admitiendo que las reducciones al absurdo no cuentan contra alguien que quiere sea absurdo (y diciéndolo en serio)

> Bryan Caplan once suggested the asteroid hypo to me as a reductio ad absurdum against my view. But a reductio ad absurdum doesn’t work against someone who’s willing to be absurd, and I may be willing to bite the bullet on this one.

Capella podría decir lo mismo, pero entonces no podría usar la reducción al absurdo con Bruenig.

## Contratos

> Igual que violar la propiedad exige una compensación, violar un contrato exige alguna compensación a la víctima perjudicada. Con una diferencia esencial: en el propio contrato es posible acordar cuál será esa compensación y qué mecanismos utilizar para resolver los problemas relacionados, lo que no es posible con agresiones no contractuales contra la propiedad (robo, destrozos, violación, secuestros, daños, asesinato). En esto la idea de contratos es mucho más potente que la idea de propiedad. Obviamente también es posible incumplir la compensación pactada dentro de un contrato, lo que implica pensar qué es justo hacer con quien llegue a este nivel de incumplimiento. Un contrato puede entenderse como una estructura finita con niveles anidados unos dentro de otros (o apilados unos sobre otros) de modo que un incumplimiento en un nivel implica saltar al nivel siguiente de forma recursiva: como se trata de un sistema finito es posible llegar al último nivel e incumplirlo también, violando así todos los mecanismos de protección pactados en propio contrato. Las cláusulas contractuales no garantizan un sistema legal infalible pero pueden proporcionar tantos niveles de seguridad como se desee.

Guardo esto para después.

> Cuando se critica a alguien conviene entender lo que quiere decir y sobre todo no inventarse inferencias e implicaciones lógicas. Según la ética que propongo alguien que voluntariamente se esclavice (algo que seguramente sería muy raro en una sociedad libre y próspera) puede dejar de ser esclavo cuando el dueño de los derechos sobre su persona se los devuelva, como un regalo o porque otro le ha pagado para hacerlo: además se supone que el que se vendió a sí mismo como esclavo sabía lo que estaba haciendo (la esclavitud voluntaria o sumisión total es algo muy serio que no se decide a la ligera), y quizás estamos olvidando lo que recibió por aceptar ser esclavo (tal vez una cantidad considerable). Lo que no puede hacer ninguna parte de un contrato es decidir incumplirlo o cambiar las condiciones de forma unilateral, sin considerar los intereses y derechos de la otra parte.

Esto es en referencia a algo que yo comenté en mi post anterior,

> Mi segunda crítica es a la idea de que una vez se ha entrado en un contrato, no debe romperse bajo ninguna circunstancia. Según la ética propuesta por Capella, alguien que voluntariamente se esclavice debe ser un esclavo el resto de su vida\[...\] Hemos dicho que las condiciones de estudio de la ética de la libertad (en principio) es un mundo donde todo el mundo la cumple, y la ética de la libertad no dice que uno sea libre de violar derechos siempre que pague una compensación (tema tratado por Nozick en Anarchy, State, and Utopia), por tanto alguien que la cumpla bien sin excepciones debe morirse de hambre.

Capella dice que me invento inferencias e implicaciones lógicas, pero no dice cuáles. Puedo entender que se refiere a que dije que alguien puede quedar esclavizado de por vida sin posibilidad de rescindir ese contrato. Obviamente, hay que entender que el dueño del esclavo puede poner fin a la esclavitud, ello va implícito en la idea de esclavitud y así funcionaba en los sistemas legales donde se permitía.

Lo que intenta hacer es edulcorar un poco la conclusión a la que llega la teoría: Sí, uno puede convertirse en un esclavo, pero eso será a) Raro b) Reversible si el propietario así lo quiere y además c)El esclavo habrá recibido algo a cambio de esclavizarse

¿Por qué yo, y Rallo (Y Rothbard) y otros muchos liberales rechazan la idea de esclavitud voluntaria? Tiene que ver con el siguiente ejemplo que planteo, el del niño. Pero antes de tratar ese ejemplo, un comentario sobre el párrafo siguiente

> Las condiciones de estudio de la ética de la libertad no consisten en un mundo donde todos la cumplen. Eso es algo totalmente irreal y que haría irrelevantes los castigos y todo el sistema policial y judicial. La ética de la libertad no dice que uno sea libre de violar derechos siempre que pague una compensación: lo que dice es que el castigo por violar derechos debe ser una compensación suficientemente alta (una pérdida de derechos) como para desincentivar al criminal y compensar (incluso mejorar) la situación de la víctima. El sistema no es perfecto, y algunos criminales pueden hacer daño: la pregunta es qué hacer al respecto, si simplemente protestar por la violación de la propiedad y la libertad e insistir en que eso está mal o buscar la mejor respuesta posible.

Aquí usé una expresión bastante desafortunada, he de admitir, al decir eso de "Hemos dicho que las condiciones de estudio de la ética de la libertad (en principio) es un mundo donde todo el mundo la cumple". Cuando dije eso, en lo que estaba pensando era: En mundo donde la gente se sujeta plenamente a la ética de la libertad, ¿Qué podría pasar? (Sin que nadie haga trampas y viole alguna de sus normas) y demuestro que puede darse el caso del hombre que se muere de hambre.

Capella debió entender que yo dije que la ética de la libertad permite violar derechos si se paga una compensación, pero si uno me lee

> la ética de la libertad**no** dice que uno sea libre de violar derechos siempre que pague una compensación

Luego explica el sistema restitutivo. Volveré a ello después.

> Para continuar con su crítica a mi explicación de los contratos recurre al ejemplo de un niño comprometiéndose en un acuerdo vitalicio. Pocos errores, distracciones o trampas argumentales hay tan graves como poner un ejemplo que no sólo no aclara sino que ofusca el tema de estudio añadiendo factores independientes o irrelevantes: los niños (y otras personas como los incapacitados mentales) son un caso especial para la ética de la libertad, y una de sus peculiaridades es si son competentes para contratar y obtener así derechos y obligaciones. Por otro lado que el acuerdo sea vitalicio puede parecer algo muy impresionante, como casi infinito, pero conviene no olvidar que existen activos financieros con horizonte temporal perpetuo y sin embargo su valor presente es finito (cosas de la preferencia temporal).

Capella comete errores bastantes graves al interpretar mi ejemplo del niño. Dice que no aclara, que ofusca, y que lo hace añadiendo factores independientes o irrelevantes. Lo cual es sorprendente, pues justo el caso de los niños lo planteo para aclarar la crítica a la idea de contratos irrompibles. Luego dice que son irrelevantes o independientes. ¿Qué quiere decir con esto? Podría dejarlo en "La ética de la libertad no tiene nada que ver con esto", que es lo que podría decir si yo hubiese escrito "La ética de la libertad es falsa porque la opresión estructural del sistema capitalista es explotación para los trabajadores". Pero los niños son un ejemplo bastante relevante, y de interés, que es justo por lo que él habla de sus peculiaridades. No explica por qué tienen esas peculiaridades, aunque según comenta [en una charla](http://newmedia.ufm.edu/gsm/index.php/Capellaetica), un niño pasa a considerarse adulto cuando así lo solicita. Antes de eso, dice Capella, puede entenderse que sus padres son sus tutores y deciden por él. Con todo, puedo insistir: ¿Por qué? Porque el niño no es consciente de lo que está haciendo. A lo que se puede decir que si alguien se arrepiente de un contrato, es porque en orígen no era del todo consciente de a lo que se estaba comprometiendo: estimó mal lo que le reportaría el contrato y los costes que tendría.

Sobre lo de los acuerdos vitalicios, es irrelevante que sean vitalicios o no. Un contrato de trabajo de 50 años plantea el mismo problema que describo, que podría describirse como la rigidización de los planes vitales de alguien por un periodo muy elevado de tiempo.

>  menciona la consciencia de lo que uno está haciendo al contratar, y también los cambios de opinión en el futuro que pueden hacer que un contrato ya no interese. Cuando uno firma un contrato se supone que lo ha leído y entendido, y con algo de capacidad intelectual pueden inferirse sus implicaciones lógicas (aunque quizás no todas). Lo que parecen no entender los críticos de los contratos exigibles por la fuerza es que precisamente los contratos se realizan para proteger a cada parte de los posibles cambios de opinión futuros de la otra parte, y que atarse voluntariamente de manos puede ser una estrategia exitosa que sería imposible o más difícil si llegara a prohibirse o a limitarse en su extensión. Cuanto más fácil sea para una parte salirse de un contrato, menos le interesará a la otra parte entrar en dicho contrato. Los contratos no requieren sólo costes de mantenimiento o cumplimiento, también exigen costes de salida, y si estos son muy bajos (al imponer desde fuera límites legales) pueden dejar de cumplir su función coordinadora. En la ética de libertad hay una asimetría sobre la entrada y la salida de los compromisos contractuales: la entrada debe ser libre (al menos en principio; uno puede prohibirse a sí mismo realizar determinados contratos), pero la salida quizás no, porque uno mismo se la ha cerrado.

Yo entiendo perfectamente la lógica de los contratos. No estoy diciendo que romperlos salga gratis, estoy diciendo que debe haber maneras de salirse de ellos. No estoy diciendo que, pongamos, un piloto de avión que se canse de su trabajo pueda dejarlo en mitad de un vuelo. Podrá largarse por los procedimientos especificados en el contrato. Si incumple esos procedimientos, entonces podría imponérsele por la fuerza cierto coste (Que determinaría el sistema judicial correspondiente). Si estamos de acuerdo con esto, estamos de acuerdo con la idea de poder dar fin a contratos de forma relativamente 'fácil', siempre que se pague esa penalización por la creación en la otra parte de expectativas incumplidas.

Recuerdo que alguien que defienda la esclavitud voluntaria de forma consistente, debería decir que el esclavista podría forzar al esclavo a no huir y a seguir trabajando. En principio ha alienado todos sus derechos. Capella tampoco está tan lejos de aceptar lo que yo propongo, pues arriba ya ha dicho que

> Obviamente también es posible incumplir la compensación pactada dentro de un contrato, lo que implica pensar qué es justo hacer con quien llegue a este nivel de incumplimiento.

Si lo justo es imponer cierto coste de salida (con los matices que uno quiera poner), ya estamos donde estoy yo.

> Una de las pruebas de consistencia de una buena teoría y de sus conceptos y relaciones es comprobar cómo se aplican de forma recursiva, reflexiva o autoreferencial sobre sí mismos, mediante realimentación positiva o negativa. Igual que algunos matemáticos contrarios a la metalógica (teorema de Goedel sobre consistencia y completud) pretendían prohibir que el lenguaje hablara sobre sí mismo (metalenguaje) para evitar paradojas destructivas (una pretensión arbitraria y estéril), algunos liberales pretenden limitar la libertad de los individuos para ellos mismos limitar o destruir su propia libertad. Olvidan que la ética de la libertad prohíbe dañar a otros, pero permite dañarse a uno mismo.

La ética liberal permite el suicidio. Ahora bien, ante una persona que sabemos que ha estado teniendo problemas mentales, o ha bebido alcohol o tomado drogas que va a suicidarse, ¿Es correcto detenerle? Parece que sí, especialmente si sabemos que la mayoría de personas cuyo suicidio es detenido agradecen que les detengan. Ahora, un suicidio de alguien que se lo haya pensado, y si eso, haya convencido al médico que le practique la eutanasia de que realmente quiere suicidarse, puede suicidarse sin problemas.

El problema con el contrato de esclavitud voluntaria no es que uno haga uso de su libertad para dañarse. Ceder derechos no es dañarse, es cambiar la estructura de derechos. En un contrato donde me comprometo a no rascarme, gano la obligación de no rascarme, pero eso no me daña per se. Lo que hace es reducir mi libertad, entendida como el conjunto de acciones que tengo permiso para realizar. El esclavo apena tiene: está obligado a hacer lo que diga el amo, y sólo tiene permiso para hacer aquello compatible con la obligación. Si se valora la libertad de los agentes como un bien a proteger, como argumenta Rallo, podemos justificar el por qué de la rotura de contratos, con compensación por la rotura del mismo incluida.

>  lía todavía más el tema con una breve disquisición filosófica sobre la personalidad y la identidad: Las relaciones sociales se basan en la continuidad de la persona (véase por ejemplo “[Personhood: A Game for Two or More Players](http://www.meltingasphalt.com/personhood-a-game-for-two-or-more-players/)”, de Kevin Simler). El arrepentimiento de un agente sobre una decisión siempre es posible, pero consideremos qué significa que mi yo presente pueda dañar a mi yo futuro: ¿da eso derecho a mi yo futuro a defenderse de mi yo presente o a exigir compensación cuando el futuro sea presente y el presente pasado?; ¿cómo va a hacerlo, si no coinciden en el tiempo? ¿Por qué deben tener precedencia las valoraciones y el conocimiento futuros sobre los presentes, o los presentes sobre los pasados? Si mi yo presente no es capaz de velar por los intereses de mi yo futuro, ¿por qué pretender que otras personas que no son yo van a hacerlo mejor?

No planteo una disquisición, sino dejando un cabo sin atar sugiriendo futuras indagaciones.

Pace Capella, las relaciones humanas no se basan tan explícitamente en la continuidad de la persona, o no al menos de forma estricta. Entendemos que la gente cambia de gustos, aficiones, trabajo, ropa, y hasta de valores o reacciones emocionales. Sí, la consciencia es la misma en todos los casos. Es un tema, [Personal Identity](http://plato.stanford.edu/entries/identity-personal/), que no he explorado aún (Derek Parfit es un autor que tengo pendiente), pero que tiene bastante relevancia en filosofía.

En cualquier caso, no baso lo que he dicho en una visión en particular de lo que es una persona. Otro argumento en contra de los contratos de esclavitud, o de forma general contra contratos que parezcan abusivos, por parte de alguien que acepta un Principio de No Agresión absoluto, Roderick Long, puede encontrarse [aquí](http://freenation.org/a/f22l1.html).

Simler respalda lo que digo sobre cambiar de opinión:

> Of course we all grant each other plenty of leeway to change our minds — it's not like one flip-flop will get you kicked out of the club. But if you change your mind often enough, or continue to state preferences that you later deny, sooner or later you'll end up frustrating your fellow persons.

Sobre en qué se basan las relaciones sociales, Simler admite que la idea de presentar identidades más o menos constantes es una ficción útil:

> Personhood, it should be noted, is always a fiction: the fiction of being a consistent, singular agent. But as long as the fiction is convincing enough, it will pass muster and be accepted by other persons. An addict's problem, then, is just that he can't maintain the fiction as convincingly as the rest of us.

Pero una ficción, que quizá sea bastante cierta a escalas cortas de tiempo, pero no a escalas largas de tiempo. Expresiones como 'Ya no soy la misma persona que era hace 10 años' tienen sentido por eso. Por eso aceptamos (véase la primera cita de Simler) pequeñas violaciones de esa ficción. Sabemos que no es real.

## Pragmatismo

>  realiza afirmaciones algo extrañas:

Sí, es cierto. Ese párrafo no lo pensé muy bien, y las cuestiones de Capella al respecto son muy pertinentes:

> no ha definido robustez, así que no queda claro qué quiere decir: normalmente las excepciones (presentes en Rallo) son como grietas o parches que ponen en peligro el sistema. Lo de algorítmico convendría aclararlo en vez de usar términos que pocos van a entender. Un algoritmo es una estructura bien definida e interpretable de instrucciones, independientemente de que sea simple o compleja, y que produce un resultado final determinista (salvo que se incluyan elementos aleatorios) preferiblemente en un tiempo finito y evitando bucles infinitos (algunos algoritmos pueden funcionar de forma indefinida). Toda ética al estar expresada en lenguaje humano natural requiere algún tipo de interpretación para ser aplicada, y puede requerir el apoyo de convenciones sociales implícitas acerca de qué es razonable explicitar o no. Tanto el sistema de Rallo como el mío están en ese sentido bien estructurados y definidos (e incluso resulta, pero no lo menciona, que los resultados de ambos algoritmos son prácticamente equivalentes, lo cual es parte esencial de mi crítica a Rallo). Rallo sí ha definido cuál de los tres principios de justicia es superior, la libertad (o al menos en qué condiciones es superior): parece que no lo ha entendido. En mi sistema ninguno es superior porque son manifestaciones de lo mismo.

Cuando hablaba de robustez tenía en mente la idea de algo difícil de romper, frente a algo frágil que sí puede romperse. Adicionalmente, tenía en mente la idea de un diamante y un trozo de carbón. El diamante es más vistoso y ordenado que el carbón, pero el diamante es más frágil. El símil es que una ética de principios absolutos es como un diamante: presenta orden y estructura, pero se encuentra con situaciones raras donde se 'rompe'. Una ética más blanda como la de Rallo no es tan ordenada y estructurada, pero permite dar respuesta a estos casos, de modo que resulta más robusta, en ocasiones de forma incluso ad hoc.

Por algorítmico me refería a un sistema que aplicado sin pensar mucho diga qué hacer (o qué está prohibido hacer). El utilitarismo y la ética de la libertad de Capella son así. Con el utilitarismo sabes que lo que tienes que hacer siempre es mirar cuánta utilidad aporta cada acción (haciendo tu mejor esfuerzo para ello), y hacer la que aporte más. Con la ética de la libertad, para ver si puedes hacer algo simplemente uno debe preguntarse si a)Uno es propietario de eso o b)Uno tiene derecho a eso por un contrato. Y ya está, no hay más vueltas que darle.

Dice Capella que no he entendido el sistema de Rallo, porque él sí ha definido qué principio es superior: la libertad.

Pero esto es tramposo: yo no hablé de un principio superior, hablé de una meta-regla para decidir. Leamos a Rallo:

> Ahora bien, que existan tres principios de justicia con los que estructurar la convivencia humana implica que estos principios no siempre tendrán que interrelacionarse de manera armoniosa entre sí. En ocasiones, podrán aparecer fricciones y conflictos entre ellos que, **normalmente**, serán sencillos de resolver: **como regla general**, la propiedad privada limita la libertad personal (esto es, una persona no es «libre» de robar a otra, ya que en tal caso la propiedad privada quedaría vacía de contenido); y los contratos, por su parte, limitan tanto la libertad personal (es decir, una persona no es «libre» de incumplir impunemente un contrato, ya que en tal caso los contratos no serían de obligado cumplimiento) como la propiedad privada (por ejemplo, los contratos sirven para transferir la propiedad o para imponerle cargas, como la hipoteca). Pero estas relaciones entre los principios **no tienen por qué ser absolutamente rígidas: no hay ningún motivo para que, bajo ciertos supuestos excepcionales, el liberalismo no pueda aceptar que la libertad personal prevalezca sobre la propiedad privada o sobre los contratos, esto es, que la libertad personal pueda limitar el uso que se ejerce de la propiedad privada y de los derechos contractuales.**
> 
> **La causa de fondo de esta posible prevalencia del principio de libertad** sobre el de propiedad o el de autonomía contractual es que tanto la propiedad privada como los contratos son mecanismos jurídicos mediante los cuales ordenar las interacciones de los planes de acciones individuales y, por  tanto,**siempre deberían ser interpretados de tal modo que minimicen aquellas situaciones que anulen grave e irreversiblemente la capacidad de agencia** (la capacidad de ejecutar fines propios) de una persona (Mack, 1995a).
> 
> (Contra la Renta Básica)

La meta-regla que según Capella no habría visto es que 'bajo ciertos supuestos excepcionales, la libertad prima sobre el resto'. No es una regla, es una más bien una heurística. No dice qué es un supuesto excepcional, más allá de decir que son situaciones donde se anula grave e irreversiblemente la capacidad de agencia. Es lo suficientemente amplio como para que todos los ejemplos raros que se me ocurren puedan ser respondidos con un 'entra en la excepción'. Si Rallo hubiese especificado algo más concreto, posiblemente también sufriría de lo que le critico a Capella.

Después veremos que en el sistema de Capella los principios de la ética de la libertad no son manifestaciones de lo mismo, o al menos, deducibles desde lo mismo.

> El problema de las decisiones jurídicas de bajo nivel existe pero no es lo que se discute: una cosa es cuáles son las reglas abstractas (cuáles son las leyes), otra cosa es decidir en cada caso concreto cuál de esas reglas es aplicable (qué ha sucedido, qué ha hecho cada agente, qué derechos y deberes concretos tiene cada uno, qué leyes se han violado): de nuevo cambia de tema, seguramente sin darse cuenta, en lugar de resolver el problema planteado.

Empezando por el final, no tengo ningún problema que resolver, pues no hago más que afirmar que la ética de Rallo no presenta los problemas de la de Capella.

Ahora, lo que sí digo es que la ética da principios generales que deben informar qué leyes debe tener un sistema de derecho positivo, y que no es trivial pasar de la ética a las leyes. Hablo no sólo de decidir los detalles de cada caso, sino de definir, y si conviene, redefinir conceptos tan importantes como agresión, propiedad, contrato, cómo interpretar contratos, o qué riesgo as admisible imponer a los demás. Esto no es un problema para Capella, viéndolo en retrospectiva (no debí haber escrito ese párrafo), pues él acepta plenamente esto. Remito al lector a la [charla](http://newmedia.ufm.edu/gsm/index.php/Capellaetica) y en general a sus artículos escritos en el Instituto Juan de Mariana, por ejemplo

> Los contenidos específicos de la noción de agresión son en cierto modo abiertos y debatibles; no es un concepto con límites abruptos, es parcialmente difuso y arbitrario. No puede ser completamente determinado mediante la deducción utilizando la razón pura, depende de costumbres, tradiciones, convenciones (bloquear la luz solar, luces de alta intensidad, sonidos a alto volumen, contaminantes). Algunos criterios objetivos pueden ser utilizados para determinar si un evento es considerado adecuadamente como agresión o no: intensidad, relación directa, extensión, duración, acumulación. ([La ética de la libertad y el cambio climático (1)](https://www.juandemariana.org/ijm-actualidad/analisis-diario/la-etica-de-la-libertad-y-el-cambio-climatico-1-libertad-propiedad-y))

Luego tenemos la cuestión del proviso Lockeano:

> Sobre el proviso lockeano, que es un tema muy interesante, comete el mismo error que Rallo: usarlo para justificar que un necesitado robe para sobrevivir a alguien que ya tiene una propiedad consolidada, cuando el proviso se refiere claramente a las condiciones a cumplir en el momento de una apropiación original de un bien previamente no poseído por nadie.

El proviso Lockeano es la condición de que uno puede apropiarse de algo sólo si deja suficiente y tan bueno para los demás. Una lectura incompleta de esto lleva a uno a pensar que la cosa se queda en la primera adquisición. Pero todo buen liberal que se haya leído a su Simmons, su Nozick y su Mack sabe que el proviso no funciona así. (Admito que no me he leído a Locke!)

Otros autores han interpretado a Locke, o redefinido el proviso, o incluso prescindido de él. La interpretación de Locke que hace A. John Simmons en The Lockean Theory of Rights (1994, capítulo 5) contempla varias interpretaciones plausibles del proviso, para concluir finalmente -contra Capella-, que

> Fairness, then, seems to generate a conflict. Fair aquisition may result in resources being closed out (or in the requirement that any reminder of them be left common, thereby eliminating rightful appropriation). But fairness also demands a share for later persons. Governmetns must settle such conflicts. But these conflicts need not be insuperable, since later persons can get their share by getting access to a living. Fairness does not necessarily require the redistributin of initially fair acquisitions. **It requires only that persons who cannot appropriate a share are not denied access to their share or room to exercise their rights of self-preservation and self-government. Where these latters goods are precluded, later persons must be allowed to appropriate the property of earlier ones**. This conception of a "fair share limit" on appropriation, holding, and transfer seems to me a plausible addition to the mixing argument.

Simmons tiene una argumentación propia sobre esto (aquí está interpretando a Locke), disponible [aquí](http://www.lse.ac.uk/philosophy/events/a-j-simmons-university-of-virginia-states-and-their-territories/) (dos partes).

Quizá Capella piense en el Proviso que plantea Nozick. (En [Zwolinski 2013a](http://www.libertarianism.org/blog/locke-nozick-justification-property) y  [Brennan 2013](http://bleedingheartlibertarians.com/2013/02/social-justice-and-the-legitimacy-of-property/) hay una discusión sobre diferencias). Supongo que Capella, aunque nunca le ha citado en sus escritos, habrá leído a Nozick.  Y Nozick dice:

> A theory of appropriation incorporating this Lockean proviso will handle correctly the cases (objections to the theory lacking the proviso) where someone appropriates the total supply of something necessary for life (For example, Rashdall's case of **someone who comes upon the only water in the desert** several miles ahead of others who also will come to it and appropriates it all.)
> 
> A theory which includes this proviso in its principle of justice in acquisition must also contain a more complex principle of justice in transfer. **Some reflection of the proviso about appropriation constrains later actions. If my appropriating all of a certain substance violates the Lockean proviso, then so does my appropriating some and purchasing all the rest from others who obtained it without otherwise violating the Lockean proviso**. If the proviso excludes someone's appropriating all the drinkable water in the world, it also excludes his purchasing it all. (More weakly, and messily, it may exclude his charging certain prices for some of his supply.) This proviso (almost?) never will come into effect; the more someone acquires of a scarce substance which others want, the higher the price of the rest will go, and the more difficult it will become for him to acquire it all. \[...\]
> 
> The total supply could not be permissibly appropriated by one person at the beginning. His later acquisition of it all does not show that the original appropriation violated the proviso (even by a reverse argument similar to the one above that tried to zip back from Z to A). R**ather, it is the combination of the original appropriation plus all the later transfers and actions that violates the Lockean proviso. Each owner's title to his holding includes the historical shadow of the Lockean proviso on appropriation**. This excludes his transferring it into an agglomeration that does violate the Lockean proviso and excludes his using it in a way, in coordination with others or independently of them, so as to violate the proviso by making the situation of others worse than their baseline situation. Once it is known that someone's ownership runs afoul of the Lockean proviso, there are stringent limits on what he may do with (what it is difficult any longer unreservedly to call) "his property." Thus a person may not appropriate the only water hole in a desert and charge what he will. Nor may he charge what he will if he possesses one, and unfortunately it happens that all the water holes in the desert dry up, except for his. This unfortunate circumstance, admittedly no fault of his, brings into operation the Lockean proviso and limits his property rights.  Similarly, an owner's property right in the only island in an area does not allow him to order a castaway from a shipwreck off his island as a trespasser, for this would violate the Lockean proviso. \[...\]
> 
> I believe that the free operation of a market system will not actually run afoul of the Lockean proviso. (Recall that crucial to our story in Part I of how a protective agency becomes dominant and a de facto monopoly is the fact that it wields force in situations of conflict, and is not merely in competition, with other agencies. A similar tale cannot be told about other businesses.) If this is correct, the proviso will not play a very important role in the activities of protective agencies and will not provide a significant opportunity for future state action. Indeed, were it not for the effects of previous illegitimate state action, people would not think the possibility of the proviso's being violated as of more interest than any other logical possibility. (Here I make an empirical historical claim; as does someone who disagrees with this.) This completes our indication of the complication in the entitlement theory introduced by the Lockean proviso.

Para rematar podemos ir al que se considera el teórico liberal en la línea de Nozick más famoso, Eric Mack. En [Mack 1995](https://docs.google.com/viewer?a=v&pid=sites&srcid=ZGVmYXVsdGRvbWFpbnxwcm9mZXNzb3JlcmljbWFja3xneDo2NTY2NzUwMTAxMzVhM2Zi), The Self-Ownership Proviso, A New and Improved Lockean Proviso encontramos ya en la segunda página el caso del náufrago que llega a la isla ya ocupada, y que el proviso intenta explicar por qué el que llega tiene derecho a estar en la isla, aunque ya esté apropiada. Y lo que es más, Mack plantea su Proviso como parte no de la teoría de adquisición de propiedad, sino de la autopropiedad: violar el proviso equivaldría a agredir contra la propiedad personal de otras personas.

El proviso, bien entendido es el que yo, Locke, Simmons, Nozick, Mack y Rallo han defendido, con diversos matices. Es Capella el que no ha entendido qué es el proviso Lockeano, yendo en contra de prácticamente todos los autores liberales de peso que han escrito sobre la cuestión.

La Stanford Encyclopedia of Philosophy recoge en su entrada sobre libertarianismo las diferentes formas en las que se ha interpretado el proviso y cuadra con lo que he presentado aquí. Cierto es que algunos autores como Rothbard rechazan el proviso, pero a) son una minoría y b) lo entienden mal (Entendido de forma simplista lleva a absurdos) En cualquier caso, echarme en cara interpretar el proviso como lo interpreta todo el mundo puede demostrar cierta falta de familiaridad con la literatura al respecto.

Capella puede rechazar el proviso y cambiarlo por otra cosa, claro está, pero no puede decir que el proviso Lockeano es lo que él dice que es, ni que generalmente se entienda así. Nótese que yo no he argumentado ni a favor ni en contra de la corrección del proviso, sólo de cuál es su interpretación general.

El lector interesado en leer más sobre diversas interpretaciones del proviso Lockeano puede leer [Widerquist 2010](http://www.publicreason.ro/pdfa/21).

Seguimos:

> Sobre que la sencillez es atractiva pero que no es el único requisito exigible a un sistema (todo debe ser tan simple como sea posible, pero no más simple), estoy completamente de acuerdo: pero no es eficiente complicar un sistema para obtener prácticamente los mismos resultados, que es parte esencial de mi crítica a Rallo.

Con esto estoy de acuerdo, con la salvedad de que depende de cómo de grande sea ese "prácticamente" y cuánta complejidad añada la nueva teoría. Un ejemplo en Física son la clásica y la relativista. Quizá no usemos la Física relativista en nuestro día a día en ingeniería, pero no por ello negamos la teoría de la relatividad. Esto sería equivalente a decir que en teoría tiene razón Rallo, pero que en la práctica es mejor implementar  leyes (funcionarán mejor) más simples (basadas en la ética que propone Capella) y que no contemplen esos casos raros que parece tenernos a Rallo y a mí obsesionados. Es una opción, que no discutiré aquí.

Otras éticas también tratan de ser pragmáticas. Los utilitaristas dicen, por ejemplo,

> The idea of practicality is often used to suggest a problem exists in the theory, when it fact it does not. For example:
> 
> "how far does one, under utilitarianism, have to research into the possibilities of maximally beneficent action, including prevention?"
> 
> The answer is simple, and entirely obvious: as far as it is useful to do so! That is, far enough so that we get the optimal trade-off between planning and implementing, so that we maximize our effectiveness as agents. The does imply that, in some cases, it may not be best to apply the felicific calculus at all: if the problem is one that we have faced many times before, and always reached the same conclusion; or if the case presents itself as an emergency, and isn't open to extended consideration; we can forego the calculus and act immediately. ([Common criticisms of utilitarianism](https://www.utilitarian.org/criticisms.html))

Las éticas deontológicas puras (que no prestan atención al riesgo) caen en absurdos, como bien puntualiza Huemer en [Lexical Priority and the Problem of Risk](http://onlinelibrary.wiley.com/doi/10.1111/j.1468-0114.2010.01370.x/epdf) (2010)

> Absolute deontological ethical theories (hereafter, ‘absolutist theories’) hold that certain kinds of action must be performed, or must be avoided, regardless of how good or bad the consequences may be.
> 
> Recent discussions have highlighted an ethical paradox that confronts such views. Suppose that it is always wrong, regardless of the consequences, to perform an action known to be of kind K. What should we say in general about cases in which a contemplated action, A, has a probability pof being of kind K? \[...\]
> 
> The most straightforward way of avoiding the problem of risk is to adopt a non-lexical (Archimedean) theory of practical reasons. This is a theory on which any reason can be outweighed by a sufficient quantity of  any kind of good. This includes most consequentialist theories as well as moderate deontological theories. It includes, for example, Bentham’s utilitarianism, as well as Ross’ pluralistic deontology.
> 
> As long as we avoid the claim of infinite weight for some practical reasons, there is a natural way of dealing with risk: if an action’s certainly satisfying C gives one a practical reason of some finite weight W, then the existence of a probability p of an action’s satisfying C gives one a practical reason of weight pxW (where the weight of a practical reason may be measured by the quantity of some simple, aggregative good required to outweigh that practical reason).
> 
> This is compatible with maintaining that justice, respect for individual rights, and other non-utilitarian values are of great importance, outweighed only in extreme circumstances. Though some deontologists would remain unsatisfied with this moderate position, it is unclear what evidence supports an absolutist position, as against the moderate position thatavoids the problem of risk.

## Intuitions all the way down

> Un punto especialmente grave del análisis de es cuando afirma que “la ética de la libertad (a mi entender) no castiga el robo directamente. Dicta que está mal. Estar mal es una razón para minimizarlo.” Pocos argumentos son tan flojos en ética como caer en el “está mal”: probablemente refleja que uno ya no tiene más ideas y está desesperado, o que en el fondo no comprende los problemas que está estudiando. Por mucho que la gente (incluso muchos liberales) utilice a menudo y dé gran importancia a expresiones solemnes, absolutas e indignadas como “eso está mal”, estas proposiciones son falaces: las cosas no están ni bien ni mal, ni son buenas o malas, sino que están permitidas o prohibidas, son obligatorias o no (siempre dentro de distintos sistemas normativos posibles en los cuales son correctas o no) y son buenas o malas para alguien o para algo. “Robar está mal” es la forma torpe de expresar que robar daña a la víctima del robo.

Esto sí es un fallo mío. Confundí _su_ ética de la libertad con ética de la libertad en sentido más amplio. Él tiene derecho a definirla como quiera. Ahora bien, lo que sigue a esa crítica muestra problemas de comprensión lectora. No sé cuánta filosofía moral o política ha leído Capella, pero en cualquier libro de estos temas se encuentra uno con estas expresiones sobre que esto o aquello están bien o mal, que para nada reflejan desesperación, falta de comprensión, absolutez, o indignación. Es una afirmación típicamente liberal aquella de "No todo lo malo debe estar prohibido". Capella no podría encontrarle sentido a algo tan común. ("No todo lo prohibido debe estar prohibido"?). Con "Ayudar a los demás está bien" Capella podría interpretarlo como "Promoved los valores ajenos!", pero cuando la gente dice "Ayudar a los demás está bien" no está emitiendo una orden. Simplemente se declara que 'estar bien' es una razón para hacer algo, no que implique que deba hacerse ese algo.  Tampoco puede encontrarle sentido a "Matar es peor que pegar un puñetazo" ("Matar está más prohibido que pegar un puñetazo"?). La prohibición es todo o nada, lo que varían son los castigos que se le ponen a lo prohibido.

"Robar está mal" es decir que se ubica en el acto de robar esa propiedad de maldad, en virtud de la cual se pedirá luego evitar o minimizar robos; no expresa que robar dañe a la víctima del robo. Acaso podrá decirse que robar está mal _porque_ daña a la víctima del robo.

Sin ánimo de entrar en debates meta-éticos en este artículo (Confieso que el prescriptivismo universal me parece erróneo), creo que una forma de naturalizar "mal" dentro del sistema de Capella es asignar que "mal" significa agresión, o violación de derechos de propiedad, falta de coordinación, o malas consecuencias. "Malo" hará referencia a aquello que prohíben los principios fundamentales del sistema ético en cuestión. Así que dentro de la ética de la libertad, Capella puede decir que "Está mal asesinar" queriendo decir que viola las normas de la ética de la libertad. Este es el movimiento que adoptan los subjetivistas "Está mal asesinar" significa para ellos "A mí me parece que asesinar no es deseable".

> Sobre mi afirmación de que uno puede hacer en su propiedad lo que quiera, le busca tres pies al gato con los típicos dilemas de matar a los invitados o ponerlos en peligro, o el caso del alquiler de un edificio que puede derrumbarse por su mal estado. Tal vez va a haber que insistir siempre en aquello de respetar la libertad y propiedad ajenas; y quizás haya que escribir con sumo cuidado para no abrir temas diferentes de los que se están discutiendo (que en este caso es la obligación de ayuda al necesitado). Y conviene no olvidar que las relaciones con invitados suelen ser informales (como no se pactan explícitamente condiciones de acceso conviene recurrir al sentido común o a normas de hospitalidad por defecto) mientras que los contratos son relaciones formales (es posible especificar que un bien alquilado no está exento de riesgos, como un edificio con algún problema, y el inquilino los asume).

Admito mi error: como comenté arriba, la ética de Capella sí admite de ciertas indefiniciones que el sistema jurídico ha de ir tratando, y mis ejemplos sí serían resolubles.

> La conclusión final de añade varios errores graves:
> 
> > La ética de la libertad es lo más parecido que tenemos a una ética correcta que cuadre en equilibrio reflexivo con nuestras intuiciones morales (descartando intuiciones espurias y sesgadas), pero debemos entender que el proyecto no está finalizado, y por tanto aún estamos muy lejos de dar unas reglas (casi) totalmente cerradas, como parece querer Capella. Rallo propone una formulación más humilde, y sí, abierta a ser abusada mediante excepciones y haciendo primar la libertad de unos frente a la propiedad de otros de forma recurrente. Pero esta ética al menos permite justificar la decisión correcta en casos aberrantes, y la de Capella (o Rothbard, pues la teoría es bastante próxima a la presentada por el economista austríaco) no.
> 
> Ningún sistema es perfecto, pero los hay mejores y peores o que cumplen o no algunos criterios objetivos (universalidad, simetría, funcionalidad). Los intuicionistas son peligrosos: no parecen darse cuenta de que quizás no todo el mundo tiene las mismas intuiciones, tal vez podemos tener intuiciones contradictorias o incompletas, y es posible que las intuiciones morales, o al menos algunas de ellas en algunas circunstancias, sean malas guías de conducta.

Ciertamente los hay mejores y peores, y que se pueda decir que cumplan o no unas u otras propiedades. Capella critica a los intuicionistas,  y lo hace bastante mal. ¿A qué se refiere Capella cuando habla de intuicionistas?

Entiendo que se refiere a los intuicionistas en meta-ética (Como Michael Huemer), pues en 2009, [comentó](http://www.albertesplugas.com/blog/2009/12/intuicionismo-%C3%A9tico.html) en el blog de Esplugas que

> El intuicionismo ético tiene graves problemas: por un lado sólo describe y pretende legitimar a partir de sentimientos; por otro, las intuiciones de personas diferentes pueden ser distintas. (Capella) No obstante, Paco, lo utilizamos constantemente en nuestra defensa "cotidiana" del liberalismo: planteamos analogías con casos concretos en los que _sabemos_ cómo se posiciona el interlocutor, apelamos a valores comunes, intentamos que sean coherente y extrapolen de un caso a otro etc. Un saludo (Esplugas) La coherencia, siendo necesaria, no es suficiente. Las intuiciones son aquello que no sabes cómo lo "sabes": resultados de procesos inconscientes, sólo sirven para decir "a mí me parece justo", pero no para buscar principios universales sólidos. Quizás la intuición está equivocada, tal vez fue correcta en su momento pero ya no... (Capella)

Siendo esto así, Capella no se ha preocupado de entender a estos intuicionistas, o lo ha hecho parcialmente y se ha hecho un lío con otros intuicionistas.

Primero, porque el intuicionismo es la tesis de que tenemos conocimiento de valores morales a través de la intuición. Intuición no es creencia o emoción, sino la apariencia (_seeming_) de que algo es el caso (verdadero). Por ejemplo, tengo la intuición de que tengo un ordenador delante de mí, porque me parece que lo tengo. En base a eso, creo justificado pensar que es cierto que tengo un ordenador delante de mí. Lo mismo para los valores morales. Dice [Huemer (2005)](https://www.amazon.com/Ethical-Intuitionism-M-Huemer/dp/1403989680):

> Reasoning sometimes changes how things seem to us. But there is also a way things seem to us prior to reasoning; otherwise, reasoning could not get started. The way things seem prior to reasoning we may call an 'initial appearance'. An initial, _intellectual_ appearance is an 'intuition'. That is, an intuition that _p_ is a state of its seeming to one that _p_ that is not dependent on inference from other beliefs and that results from thinking about _p_, as opposed to perceiving, remembering, or introspecting. An ethical intuition is an intuition whose content is an evaluative proposition. (§5.2)

Segundo, dice Capella que los intuicionistas ignoran que las intuiciones pueden ser diferentes. Muy equivocado: La cuestión del desacuerdo siempre ha estado reconocida por los intuicionistas [desde el principio](http://plato.stanford.edu/entries/intuitionism-ethics/#Dis).

Tercero, el intuicionista no pide que aceptes acríticamente tus intuiciones. ¡Todo lo contrario!. Sin ir más lejos, [Huemer (2008)](http://journals.cambridge.org/action/displayAbstract?fromPage=online&aid=1596900&fileId=S026505250808014X) argumenta:

> Intuitionists need not embrace anything close to common-sense morality. Instead, intuitionists may, and probably should, adopt revisionary ethical views, rejecting a wide range of commonly accepted, prephilosophical moral beliefs. Indeed, I believe intuitionists are better positioned than partisans of most alternative conceptions of ethics to motivate a rejection of common-sense morality. This fact derives partly from the staunch realism of ethical intuitionism, and partly from the evidence, often cited by critics of intuitionism, of nonrational biases contaminating common moral judgments. \[...\] A second argument for the unreliability of ethical intuition (on the assumption that objective moral truths exist) points to the influence of culture on individuals’ ethical intuitions, together with the wide variability of cultures \[...\] A third problem afflicting our moral intuitions is that of biases produced by evolutionary forces. Given the importance of ethical intuitions to human behavior, and presumably to our reproductive fitness, it is reasonable to speculate that natural selection might have favored predispositions toward certain kinds of intuitions over others. When we look at the content of most people’s moral intuitions, many of them indeed look suspiciously like the sorts of intuitions natural selection might produce, and rather unlike products of impartial, intellectual reflection. Most people would probably find each of the following evaluative claims plausible: (a) Individuals have much stronger obligations toward their own kin, especially their children, than toward others. (b) Loyalty and devotion to one’s own society, as above other societies, is a virtue. (In modern times, this virtue takes the form of patriotism.) (c) Human beings are far more important than any other species, and human interests count, morally, for vastly more, if not infinitely more, than animal interests. (d) Incest is inherently wrong, even in cases of mutual, informed consent. (e) Sexual promiscuity is desirable for a male, but highly undesirable for a female. It is difficult to explain why the above evaluations should be true, but it is easy to see why natural selection would have rewarded those who believed them. \[...\] Finally, as Walter Sinnott-Armstrong argues, our moral intuitions may be biased by emotions and personal interests. It is easy to see how our interests are affected by what evaluative propositions we and others accept. On the one hand, if I reject the moral significance of the distinction between killing and letting die, I may feel obliged to donate large amounts of money to the poor; if I accept that the interests of animals matter morally, I may feel obliged to give up some of my favorite foods. On the other hand, I stand to benefit from others’ acceptance of the obligation to assist others (particularly if I am poor) and of the obligation to refrain from harming others. These facts may bias me toward certain evaluative conclusions and away from others.  \[...\] When we put together the foregoing considerations, a cautious and critical intuitionist methodology emerges. While our ethical theory must be based on intuitions, we should not accept or reject an ethical principle solely on the basis of an uncritical appeal to a single intuition, nor should we assess an intuition’s probative value solely in terms of its subjective strength. Rather, we should screen intuitions according to the following criteria: (1) Seek a substantial body of ethical intuitions that fit together well, rather than placing great weight on any single intuition. (2) Eschew intuitions that are not widely shared, that are specific to one’s own culture, or that entail positive evaluations of the practices of one’s own society and negative evaluations of the practices of other societies. (3) Distrust intuitions that favor specific forms of behavior that would tend to promote reproductive fitness, particularly if these intuitions fail to cohere with other intuitions. (4) Distrust intuitions that differentially favor oneself, that is, that specially benefit or positively evaluate oneself, as opposed to others. (5) Distrust intuitions that line up with strong emotions one has about the things evaluated.

Quizá Capella se estuviese refiriendo a intuicionistas como aquellos que dicen que la moral se basa en emociones, como que al pensar en cuestiones morales, la gente siente reacciones emocionales y en función de eso, dice si algo es bueno o malo. Para estos, lo placentero y agradable sería equivalente a bueno, y lo triste, doloroso, asqueroso o raro sería lo malo.

Esta posición no es intuicionismo, sino una forma de [no-cognitivismo](https://artir.wordpress.com/2013/10/15/las-diferentes-visiones-meta-eticas/) (Pertenece a la misma familia meta-ética que la posición de Capella), el emotivismo. Esto fue lo que defendieron, por ejemplo, Hume y Adam Smith. Casualmente hace unos días hice un post sobre Humeanos, así que me cito:

> (Chris Dillow) I’ll confess that this is true for me; **my antipathy to inequality is, at root, an emotional one and my apparently rational arguments are the slave of that passion**. I suspect – though cannot prove – that the same is true of Dawkins. Reading _The God Delusion_ gives me the impression that Dawkins is motivated by an emotion of disgust at some of the effects of religion. I happen to share that feeling in many ways, but it is a feeling. (Noah Smith) **And as I see it, the [is-ought rule](https://en.wikipedia.org/wiki/Is%E2%80%93ought_problem) means that internally self-consistent ethical systems are no intrinsically better than self-contradictory ones**, so the whole exercise of trying to corral people’s beliefs into a periodic table yields little practical benefit. My own ethical system is sometimes libertarian, sometimes utilitarian, sometimes both, and sometimes neither; and\_I’m fine with that\_. Well, I could just reply that **it’s purely my moral intuition, and as a [Humean](http://plato.stanford.edu/entries/hume-moral/), I don’t need any other justification. In fact, any justification I give will open itself up to questions of “But why?”, until I finally just say “Because that’s just how I feel”**, or “Oh come ON!”. \[…\] I think of myself as a pragmatist. I don’t have a very self-consistent, well-defined ideology to myself. I can’t really–**I think of myself as a Humean \[David Hume\]. I have these _moral instincts_**–I think, ‘Oh, well, you know, this is too much a restriction on individual liberty’ and ‘I don’t like _that_.’ And then in some other case I’ll think, ‘Oh, look at these poor people,’ you know, ‘We’ve got to have the government do something to help those poor people.’ **And there’s not necessarily a rhyme or reason to that. It’s just sometimes I have one instinct and sometimes I have the other. And I think of those moral instincts as sort of the primitive, primal, basic elements of morality, and they don’t always work–they don’t always form a self-consistent whole.**

Esta gente creo que es lo que Capella tiene en mente cuando hace su crítica. Pero éstos no son intuicionistas!

Una tercera posibilidad es que se refiera a los seguidores en psicología moral de Jonathan Haidt (Social Intuitionists), que defienden una visión no-cognitivista emotivista (La emoción causa los juicios morales), y que la razón únicamente trata de racionalizar esas emociones. Haidt plantea esto como algo descriptivo. Capella parece rechazar la tesis fuerte de Haidt (Quizá sí sea posible imponer la razón sobre las emociones), pues en [La psicología moral y el liberalismo](https://www.juandemariana.org/ijm-actualidad/analisis-diario/la-psicologia-moral-y-el-liberalismo) dice:

> La moralidad es fundamentalmente emocional: se trata de sensaciones, emociones o sentimientos morales. La razón casi nunca es la fuente de las valoraciones éticas: los humanos primero tienen intuiciones morales (reacciones automáticas resultado de la actividad inconsciente del cerebro), y posteriormente argumentan y racionalizan para intentar defender su postura; el pensamiento desapasionado, imparcial o desinteresado es raro, y abundan los sesgos, los errores sistemáticos, el engaño (incluido especialmente el autoengaño) y la hipocresía. La argumentación ética es a menudo una cuestión de imagen, de reputación, de relaciones públicas, de estatus intelectual y social, de justificación propia y de manipulación de la conducta ajena. La moralidad es una adaptación evolutiva que sirve para unir y coordinar a los miembros de un grupo cooperativo y para diferenciarlos y separarlos de individuos de otros grupos contra los cuales pueden competir (selección de grupo en evolución multinivel): no se daña y se ayuda a los miembros de la comunidad (familia, tribu, clan, nación); se ignora, se discrimina o se lucha contra individuos de otros grupos. \[...\] El liberalismo es una filosofía moral y política que se basa en la libertad como no violencia, agresión o coacción. No obliga a ayudar a nadie; es individualista y universalista, no distingue entre grupos ni exige lealtad ni obediencia a ninguna autoridad; no se ocupa de temas relacionados con la pureza o la divinidad. Por eso resulta extraño e incluso inaceptable para la mayoría de las personas con fuertes instintos tribales, que creen que ayudar es un deber ineludible, o que sienten fuerte asco o indignación ante violaciones de ciertas normas relacionadas con temas sagrados. Independientemente de si los liberales son buenos o malos vendedores de sus ideas, estas son muy difíciles de popularizar.

De aquí entiendo que Capella piensa que la mayoría de la gente funciona así, pero es posible, si uno se toma el asunto en serio, llegar a una filosofía razonada, sin emociones, si se parte de ciertos principios. Si no, Capella no estaría tan lejos de los emotivistas que critica.

Cierro reiterando que, primero, mi propósito con esto no es defender el intuicionismo, sólo corregir lo que Capella piensa de ello. Y segundo, que el método que sigue Capella es justo lo que los intuicionistas dicen que debe hacerse: sus intuiciones son universalidad, funcionalidad, y simetría. Funcionalidad no es una intuición formal, sino algo relativamente vago que aglutina consideraciones deontológicas y consecuencialistas. Intuiciones las hay más formales y menos formales, y para el intuicionista hay que hacer lo que Rallo dice que hay que hacer: coger intuiciones, filtrarlas, intentar hacerlas coherentes y razonar con ellas: equilibro reflectivo.

> Las apariencias engañan, pero a veces el engaño está en quien lee, que se proyecta y aplica sesgos, y no en quien escribe. Yo no pretendo disponer de reglas casi totalmente cerradas: analizo las alternativas y muestro abiertamente los resultados para que puedan ser comprobados y criticados; si son correctos las críticas serán fallidas. En este caso las críticas son fallidas, lo que incrementa provisionalmente la evaluación de corrección. Como ejemplos de temas abiertos en mi presentación de la ética de la libertad está precisamente (y entre otros) el problema de la apropiación original por primer uso y sus límites: qué es primer uso, cuánto hay que usar un bien para apropiarse legítimamente de él (la historia real es mucho más compleja que la versión teórica limpia y sin conflictos de interpretación o aplicación).

"si son correctos las críticas serán fallidas." Esto qué quiere decir? Que siguen de los principios? Que los resultados son correctos? (Según qué criterio? No puede ser el de funcionalidad, pues sería un razonamiento circular). Esto tampoco es importante, así que tampoco vale la pena comentar más.

Capella admite que su presentación tiene temas abiertos: apropiación original siendo uno de ellos. Pero es que justo de ahí salen las excepciones que ponen el resto de autores liberales para los casos "raros".

> Lo de que la formulación de Rallo es más humilde suena a relaciones públicas: ¿quiere decir que no está seguro de si tiene razón o no? Rallo no dice que quizás merezca la pena plantearse la posibilidad de aplicar o estudiar excepciones a la norma de propiedad: con todas las limitaciones que pone, las defiende de forma bastante rotunda (y con los problemas ya señalados en mi crítica, véase también la segunda parte).

Con lo de humildad (otro uso desafortunado del lenguaje por mi parte) volvía a referirme a la diferencia entre una teoría que es más o menos estricta respecto a sus principios. Esto es algo que Capella acepta que es el caso y le critica,

> La introducción de excepciones requiere explicaciones o justificaciones muy fuertes ya que puede poner en peligro al sistema: si se aceptan esos casos particulares, ¿por qué no otros? ([Crítica a "Contra la Renta básica", 2015](https://www.juandemariana.org/ijm-actualidad/analisis-diario/critica-contra-la-renta-basica-de-juan-ramon-rallo-i))

Sigo:

> Lo de “justificar la decisión correcta” cae en el mismo error que el “eso está mal”: o sea que ya sabíamos lo que es correcto antes de empezar (en realidad son nuestras preferencias camufladas y las cosas que queremos prohibir) y el juego consiste en adaptar o forzar los argumentos hasta llegar a ello (razonamiento motivado hacia lo que nos gusta o interesa). Desgraciadamente este es el mal más fundamental y difícil de matar de la filosofía moral: creer que ya tenemos claro lo que está mal y lo que es correcto, sin saber si quiera qué significan esas dos importantes palabras en relación con otras más pedestres y realistas como normas y preferencias.

Remito al lector a mi discusión previa y la diferencia entre intuiciones más y menos abstractas, y la importancia del equilibrio reflexivo. Lo de deducir a partir de principios que parecen razonables está muy bien, pero no así si llevan a conclusiones absurdas. Ahí está Berkeley, que dijo demostrar que la materia no existía, y Descartes, que creyó haber concluído que Dios existía, a partir de un sólido y cuidadoso razonamiento.

Lo peor es que Capella no se da cuenta de que cae (y está bien que lo haga) en lo que critica, como veremos en la siguiente sección. Considera cosas como absurdas y rechazables para avanzar en su razonamiento lógico. A eso me refiero yo con lo de "justificar la decisión correcta". Cada vez que Capella [dice](http://newmedia.ufm.edu/gsm/index.php/Capellaetica) 'me parece razonable', 'es absurdo' y demás, está usando intuiciones.

Con esto termina mi crítica del artículo. Ahora explicaré los problemas lógicos de la ética de la libertad como tal.

## Formulando la ética de la libertad

Por paradójico que parezca, yo apenas he hablado de en qué consiste la ética de la libertad en este post. Tampoco Capella lo ha hecho en ninguno de sus escritos que tiene disponibles en la red. Siempre habla de normas universales, simétricas y funcionales, pero nunca ha hecho una deducción paso a paso de la ética de la libertad, ni ha explicado en qué consiste exactamente. Por suerte, las piezas necesarias para hacer tal cosa sí las ha ido desgranando con el tiempo, así que trataré aquí de presentar la mejor versión posible del argumento de Capella.

Si el lector quiere comprobar por sí mismo mis fuentes, puede ver los siguientes artículos, dándole más peso a los más recientes.

-   [Ética y Libertad](https://www.youtube.com/watch?v=uriif99PR_Y) (Conferencia UV-IJM 2014)
-   [Ética de la libertad](https://www.youtube.com/watch?v=l5UKF0efyFY) (Conferencia UFM 2014)
-   [Ética y eutanasia](http://www.ilustracionliberal.com/9/etica-y-eutanasia-francisco-capella.html) (Ilustración liberal, 2001)
-   [Libertad e igualdad](http://www.libertaddigital.com/opinion/ideas/libertad-e-igualdad-964.html) (Libertad Digital, 2001)
-   [Utilitarismo o iusnaturalismo](http://www.ilustracionliberal.com/9/utilitarismo-o-iusnaturalismo-francisco-capella.html) (Ilustración liberal, 2005)
-   [La libertad es beneficiosa](https://www.juandemariana.org/ijm-actualidad/analisis-diario/la-libertad-es-beneficiosa) (Instituto Juan de Mariana, 2015)
-   [Leyes, legalidad y legitimidad](https://www.juandemariana.org/ijm-actualidad/analisis-diario/la-libertad-es-beneficiosa) (Juan de Mariana, 2014)
-   [Anarquismo liberal sensato](https://www.juandemariana.org/ijm-actualidad/analisis-diario/anarquismo-liberal-sensato) (IJM 2013)
-   [La universalidad de las normas éticas](https://www.juandemariana.org/ijm-actualidad/analisis-diario/la-universalidad-de-las-normas-eticas) (IJM 2009)
-   [Libertad y asco](https://www.juandemariana.org/ijm-actualidad/analisis-diario/libertad-y-asco)(IJM 2007)
-   [Malas críticas contra el liberalismo](https://www.juandemariana.org/ijm-actualidad/analisis-diario/malas-criticas-contra-el-liberalismo) (IJM 2015)

### La ética de la libertad

¿Qué es la ética de la libertad que querría Capella justificar? Esto:

1.  Está prohibido iniciar agresiones (violar los derechos de propiedad ajenos, excepto cuando alguien ha violado previamente derechos de propiedad y se busca restitución)contra otros (Principio de No Agresión)
2.  Si A pacta X con B, B tiene la obligación de X (Contratos)
3.  Si A incumple una obligación respecto a B, A está obligado a realizar acciones que conduzcan a que la situación de B sea lo suficientemente parecida a la que tenía inicialmente. (Principio de Restitución)

Esta teoría necesita ser suplementada con una teoría de derechos de propiedad que regule quién tiene quién, para poder determinar qué es o no es una agresión:

1.  Los agentes, cuando comienzan a serlo, pasan a ser propietarios de sí mismos (Autopropiedad)
2.  Si Y no tiene propietario, un agente A puede apropiárselo llevando a cabo acciones de tipo Z (Apropiación originaria)

Luego la ética de la libertad sería una norma categórica (aplica a todo el mundo), más dos normas auxiliares, y dos axiomas que especifican qué es propiedad (No son normas, sino puntos de partida para las normas).

### Argumentando la ética de la libertad

¿Cómo argumenta Capella esta ética?

1.  Las normas deben ser universales en sentido fuerte
2.  Las normas deben ser simétricas
3.  Las normas deben ser funcionales
    1.  No deben llevar a consecuencias absurdas
    2.  Deben permitir la cooperación armoniosa entre los agentes
4.  Los operadores deónticos para la formación de normas son obligación y prohibición
5.  Las acciones a las que se aplican las normas pueden dividirse en: ayudar (hacer algo bueno para otros),agredir (hacer algo malo para otros) a los demás, y acciones neutras
6.  Por (4,5) las normas posibles son Prohibido ayudar, Obligatorio ayudar, Obligatorio agredir, Prohibido agredir
7.  Por (3) las normas anteriores salvo la última no son funcionales
8.  Por tanto, la única norma que cumple con las premisas es Prohibido Agredir

Esto plantea problemas: Tenemos primero la nebulosa premisa 3, que está abierta a interpretaciones varias, y luego varios saltos lógicos injustificados, y premisas escondidas. La noción de agresión depende de la idea de derechos de propiedad, que debe explicitarse. Luego faltarían por justificar los contratos y el principio de restitución.

Recordemos la norma "Si A tiene muchos más recursos que el resto de agentes, A está obligado a dar recursos al resto de agentes.". Tiene la forma "Si A cumple la condición C (tener muchos recursos), entonces A está obligado a X respecto al resto de agentes. "

El principio de contratos tiene la misma estructura "Si A cumple C (realiza la acción de pactar X con B), entonces A está obligado a X respecto a B". De igual manera, el principio de restitución podría exponerse. Si esto es así, o bien el criterio de universalidad rechaza ambas normas, o las acepta ambas. Yo veo más plausible aceptarlas ambas. Por tanto, la universalidad fuerte no podría salvar a esta norma.

Luego tenemos el problema de la autopropiedad, y la propiedad en general. ¿Cómo justificarla?

En el vídeo de la UFM, Capella admite que 'Prohibido agredir a los demás' es muy abstracto. ¿Qué es agredir? ¿Sólo el cuerpo? ¿Y si me roban? Dice que tenemos intereses sobre cosas. Nótese que cuando descarta la norma de 'Obligatorio ayudar a los demás', no intenta hacer un análisis para hacerla plausible (como sí hace con 'Prohibido agredir'), sino que dice que los que la defienden la descafeinan y cualifican. Pero él hace esto mismo. 'Obligatorio ayudar a los demás' tiene otros problemas, no obstante.

El principio de apropiación originaria lo argumenta de forma separada: "Antes no era de nadie. Luego no se lo robo a nadie" (Y las demás posibilidades son absurdas)

El principio de contratos lo argumenta por reducción al absurdo: Si no, sería disfuncional.

La autopropiedad la intenta justificar con otro argumento, que puede retrotraerse a Rothbard: que el resto de alternativas es absurdo. (Si el cuerpo de los demás es de otros, sería problemático)

En lo siguiente me apoyo en [Eabrusu 2013](http://ppe.sagepub.com/content/12/3/288). El argumento de Rothbard, y por extensión, el de Capella, es

> Let us ... concentrate on the question of a man’s ownership rights to his own body. Here are two alternatives: either we may lay down a rule that each man should be permitted (i.e. have the right to) the full ownership of his own body, or we may rule that he may not have such complete ownership. If he does, then we have the libertarian natural law for a free society as treated above. But if he does not, if each man is not entitled to full and 100 percent self-ownership, then what does that imply? It implies either one of two conditions: (1) the ‘communist’ one of Universal and Equal Other-ownership, or (2) Partial Ownership of One Group by Another – a system of rule by one class over another. These are the only logical alternatives to a state of 100 percent self-ownership for all ... Hence, no society which does not have full self-ownership for everyone could enjoy a universal ethic. For this reason alone, 100 percent self-ownership for every man is the only viable political ethic for mankind.

Rothbard considera que algo es absurdo si implica una contradicción lógica ('esto es un círculo cuadrado'), y no si meramente no es razonable. Luego su argumento funcionará no si las alternativas no son razonables, sino si son absurdas, o conceptualmente imposibles.

Como Zwolinski [argumenta,](http://bleedingheartlibertarians.com/2012/10/reading-the-ethics-of-liberty-part-4-rothbards-second-argument-for-self-ownership/) el argumento de Rothbard se basa en la asunción de que o se es propietario de algo o no se es. Pero esto es falso:

> Rothbard’s argument is based on the assumption you either own something or you don’t, and those are essentially the only two possibilities. “Ownership,” for Rothbard, means “full” ownership, and the unstated assumption is that there is some clear and determinate natural fact about what precisely that entails. And this is true even when Rothbard seems to complicate matters by allowing for “partial” ownership as in the case of universal communism. For what “partial” ownership seems to mean, in this case, is simply that you’re given an equal vote with your co-owners about how to exercise the rights of full ownership that you all jointly wield. \[...\] The problem is that ownership isn’t this simple. There isn’t any obvious natural fact regarding what it means to “own” something. Instead, ownership can mean different things – can have different moral implications regarding the rights and duties of the owner and others – in different situations. And so, as a result, there aren’t just \_three \_possible options regarding ownership of the self. There are \_infinitely many \_options. \[...\] Consider the ownership one might have in a piece of land. At its core, we might think, to own a piece of land is to have the right to use the land as one sees fit, and to exclude others from using it without one’s permission. But even if we grant this “core” concept of ownership, it leaves a great number of questions unanswered. What rights does one have to, say, the minerals located below the surface of one’s land, and how deep do one’s rights go? How high \_above \_one’s land do they go? High enough to prohibit planes from flying over it? Satellites from orbiting above it? Does your ownership give you the right to dam any rivers that might flow through your land and onto mine? If your land blocks the only path between my land and some valuable unowned resource, like an ocean, do I have the right to cross your land without permission to get there?

¿Qué otras posibilidades existen que Rothbard no ha considerado?

> We might own ourselves in this robust way except that others have a claim-right against us that we provide them with assistance in situations of dire emergency. We might own ourselves in the sense of being able to use and sell our labor pretty much as we see fit, but lack the liberty-right to sell body parts like kidneys or hearts. We might have the liberty-right to use and sell our labor, but not the claim-right to the full market value of our labor. And so on.

Estas otras posibilidades son universales y simétricas, así que el criterio adicional que Capella impone se cumple. La funcionalidad es lo que salvaría el argumento de Capella frente a Rothbard, pero ya hemos visto los problemas de la nebulosa funcionalidad.

Sobre el principio de apropiación originaria, volviendo a Eabrasu, un razonamiento similar al expuesto por Capella es presentado por Hoppe: Las alternativas serían que uno obtenga propiedad declarando verbalmente que la tiene, en vez de interactuando con las cosas, o que alguien puede apropiarse de algo sin consentimiento de su previo propietario. Estas normas podrá decirse que no son funcionales o absurdas, pero no imposibilidades lógicas. La de la apropiación ex-post, en concreto, si se modifica algo, es totalmente consistente con el proviso lockeano: 'Uno no puede apropiarse de algo previamente apropiado, excepto si se da la condición C'. Otras formulaciones del principio de apropiación originaria dan lugar a diversas filosofías políticas, que sus autores no dudarán en defender como funcionales (véase el artículo ya enlazado de Widerquist).

Capella puede defender estos principios no deduciéndolos del principio de no agresión, sino a partir de intuiciones de lo que es funcional. ¿Cómo van a ser pues libertad, propiedad y contratos lo mismo si lo uno no se puede deducir de lo otro?

### Errores en el argumento

Si ya he expuesto tantas normas universales, válidas y funcionales arriba, ¿Dónde exactamente falla el argumento de Capella?

Creo que los proponentes de todas las otras normas válidas atacarán la premisa 5, y la inferencia 6, o las falacias e inferencias tramposas. Reconstruyamos pues el argumento para hacerlo válido:

1.  Las normas deben ser universales en sentido fuerte
2.  Las normas deben ser simétricas
3.  Las normas deben ser funcionales
    1.  No deben llevar a consecuencias absurdas
    2.  Deben permitir la cooperación armoniosa entre los agentes
4.  Los operadores deónticos para la formación de normas son obligación y prohibición
5.  Las acciones a las que se aplican las normas pueden dividirse en: satisfacer mis preferencias (de primer orden), satisfacer las preferencias ajenas (ayudar), violar la propiedad ajena (agresión) y acciones neutras
6.  Propiedad ajena se define según la teoría de propiedad expuesta anteriormente
7.  No es posible establecer combinaciones o excepciones a las reglas anteriores.
8.  Por (4,5,6) las normas posibles son Prohibido/Obligado satisfacer mis preferencias, Prohibido/Obligado satisfacer preferencias ajenas, P/O violar la propiedad ajena, P/O acciones neutras
9.  Por (3) las normas anteriores salvo la última no son funcionales
10.  Por tanto, la única norma que cumple con las premisas es Prohibido Agredir

Este argumento es válido y la conclusión sigue si se aceptan sus premisas. Pero evidentemente, los no-liberales rechazarán la premisa 6, y casi todo el mundo rechazará la premisa 7. La premisa 7 es además necesario rechazarla si Capella luego quiere permitir que otras reglas cuadran con estas condiciones. El principio de contratos  y restitución (Prohibido agredir es prohibido agredir, bajo cualquier circunstancia, por cualquier razón) no serían justificables si se acepta este argumento. Se habría logrado establecer que está prohibido agredir, y que ninguna otra regla es aceptable. Se habría asumido además como premisa la teoría de propiedad liberal, cosa que sería rechazada por quienes rechacen la ética de la libertad, sin rechazar los criterios de universalidad, simetría y funcionalidad.

## Capella debate con sus críticos

A modo de coda o postre a este ensayo absurdamente largo, presento una suerte de debate socrático, donde un Capella ficticio (C) trata de argumentar a un utilitarista (U), a un rawlsiano (R) y a un socialdemócrata (S) su ética, y de cómo irían respondiendo estos otros personajes.

C: Aceptáis que la ética debe ser universal, simétrica y funcional?

U: Yup

R: Sí, por supuesto

C: Bien, entonces sabed que la única alternativa posible es la ética de la libertad. Tengo un argumento que lo demuestra.

U: Hm. Eso es difícil, pues 'Actúa tal que la satisfacción de preferencias, o bienestar, del mundo sea máxima' es universal, simétrica y funcional, mientras que la tuya deja preferencias intensas sin satisfacerse. Permite que exista gente con muchos recursos y otros con menos, lo cual no maximiza la utilidad. Además, falla en situaciones donde esos supuestos derechos que quieres conceder impiden maximizar la utilidad. Piensa en alguien que no quiera vender sus terrenos para construir una autopista, o en las patentes, si fuesen beneficiosas.

C: Será universal y simétrica, pero no es funcional. Es imposible o muy difícil medir utilidades intersubjetivamente, ni saber con toda precisión qué es lo que maximiza la utilidad.

U: Ciertamente, por eso hay que ser pragmáticos e intentar hacerlo lo mejor posible para lograrlo. Somos seres humanos limitados, tenemos que hacerlo lo mejor que podamos. Pero al menos intentamos tener una ética funcional que no dé lugar a situaciones como las que he descrito antes, que se dan por tu cabezonería en insistir en derechos absolutos. ¿No leíste a Bentham cuando declaró que los derechos naturales eran 'Sinsentidos sobre zancos'?

C: Veo que tenemos una diferencia irresoluble en nuestra concepción de funcionalidad. Pero si te demuestro que, empíricamente, tu ética no es funcional, ¿Cambiarías de idea?

U: Surely! De hecho, ya defiendo algo parecido a tu ética de la libertad, como hace [Scott Sumner](http://econlog.econlib.org/archives/2014/07/i_hurt_therefor.html). ¿No te convence más mi idea de funcionalidad? ¿Por qué aceptas esos derechos? J[osh Greene](https://www.amazon.com/Moral-Tribes-Emotion-Reason-Between/dp/1594202605), a quien has leído, argumenta que tus intuiciones deontológicas sobre derechos inquebrantables son respuestas emocionales. Lo racional, cuando lo piensas fríamente, es el utilitarismo. Tú mismo te has reído ([minuto 24:0](https://www.youtube.com/watch?v=uriif99PR_Y)0) de los deontólogos, y con razón!

C: No siento ninguna emoción cuando defiendo los derechos. Simplemente planteo condiciones, y exploro a dónde llevan. No acepto la ética de la libertad directamente, sino por una serie de argumentos racionales.

U: Sí, pero todo argumento comienza en alguna parte, y los tuyos no son excepción. Un ejemplo es tu concepción de funcionalidad, y tu insistencia en normas sencillas y excesivamente claras. Yo prefiero otra definición de funcionalidad, que aunque resulta en una norma más compleja que la tuya, cumple mejor las condiciones propuestas.

C: Tienes algún argumento para deducir el utilitarismo a partir de estos principios, más allá de decir que es compatible?

U: Sí, prueba con [este](http://raikoth.net/consequentialism.html) de Scott Alexander.

C: Ya hablamos.

R: Entonces me toca. Yo creo que la ética resulta de los principios aceptados tras un velo de ignorancia. Como los agentes no saben qué posición les va a tocar en sociedad, escogerán las mejores normas posibles, sin privilegiar a unos sobre otros, porque no saben dónde van a caer. A diferencia de la tuya, mi ética es funcional, pues permite redistribuir renta a los más desfavorecidos, mientras que tú les dejas morirse de hambre.

C: No todo el mundo elegiría lo mismo tras el velo. Podrían elegir la ética de la libertad si quisiesen.

R: Sí, eso se dice. Pero el argumento hace referencia a la ética que uno exige que sea universal, y en mi caso, para poder exigir que sea universal y aplique a todos, primero he debido de pasar por el filtro del velo, y lo que a mí  y a mucha otra gente nos parece es que tras el velo se elegiría eso. Permito un pluralismo razonable, como explica Rawls en Political Liberalism.

C: ¿No ves que la regla maximin no es universal en sentido fuerte, y además gravemente disfuncional. Imagina una sociedad donde todos sean ricos y uno tenga una riqueza de 1, y otra sociedad donde todos sean pobres y tengan una riqueza de 2. El maximin condena al absurdo, como apuntó Harsanyi.

R: Sí es universal. ¿Por qué iban unos agentes tras el velo a elegir una regla no universal en sentido fuerte? La regla no habla de ningún agente en particular, sólo de aquel que en cierto momento pueda estar peor. El maximin hay que entenderlo en sentido algo laxo. Rawls ya decía en A Theory of Justice que no debe aplicarse en estas situaciones donde conduce a absurdos. Rawls se equivocó cuando dedujo que su ética llevaba al socialismo de mercado o la planificación central, pero eso es porque tenía conocimientos empíricos erróneos. El Rawlsianismo moderno propone un Estado del bienestar liberal, o un Estado del bienestar mundial, según otros teóricos.

C: Insisto, tu norma está pensada para Estados, no para grupos de gente arbitrarios

R: Claro, pero de eso se trata. No necesariamente necesita Estados, el anarquismo rawlsiano es una posibilidad lógica. Pero no pretende ser una teoría de ética interpersonal, sino a nivel de la estructura fundamental de la sociedad, de cómo se configuran los derechos de propiedad.

C: Veo que no compartimos los mismos puntos de partida. Quizá con algún argumento pudiese ir un nivel por encima y convencerte de que te equivocas...

R: Podrías empezar por justificarme esa teoría de la propiedad que propones

C: ¿Aceptas que cada uno es dueño de su propio cuerpo?

R: Si eso es lo coherente con que el más desfavorecido esté mejor, sí. Normamente será el caso. Puedes ponerme ejemplos raros sobre [redistribución de riñones](http://www.albertesplugas.com/blog/2008/12/a-vueltas-con-la-redistribuci%C3%B3n-de-%C3%B3rganos.html), pero instituir esa regla sería desventajoso para el desfavorecido a largo plazo, así que la rechazo.

C: Vale. ¿Aceptas que si alguien interactúa con algo sin dueño de cierta manera se convierte en su propietario?

R: Sí, con reservas. Los derechos son constructos sociales que la sociedad (el conjunto de individuos) reconoce a sus miembros. Tras el velo, la teoría de  propiedad privada para cosas que no son el cuerpo que resulta no es tan extensa como tú supones. Ser propietario de algo significa que el resto de la sociedad te reconoce que puedes usar eso, pero sólo será así mientra sea consistente con el maximin. La teoría liberal siempre me ha parecido una forma de ratificar injusticias existentes a partir de algo tan arbitrario como quién llegó primero a un recurso.

S: Yo en cambio, acepto universalidad, simetría y funcionalidad, y que la única norma válida sí es no agredir

C: ¿Entonces por qué no eres liberal?

S: Considero que el Estado ha adquirido con el tiempo el derecho a mandar sobre la gente que ocupe un territorio, por usucapión. El Estado se ha arrogado derechos sobre la tierra que controla, y no habiendo puesto la gente resistencia, los ha ido adquiriendo. Puedes verlo como si fuese un club. La gente que está dentro acepta voluntariamente  lo que mande el Estado. Así, los impuestos, el Estado del Bienestar y demás normas asimétricas están justificadas en tanto la gente las acepta voluntariamente. Tendrías que darme un argumento para justificar tu teoría de la propiedad primero. O, aceptando tu teoría, ¿No debemos presuponer que si no se sabe quién era el propietario anterior, debe presumirse que el propietario actual es el propietario legítimo? Aplícale eso al Estado.

C: Pero eso lleva a conclusiones absurdas. Si el Estado quisiese expulsar mañana a una etnia de su territorio, o limitar la libertad de expresión, o cualquier cosa, le sería legítimo hacerlo, pues los afectados estarían en una gigantesca propiedad privada. ¿Qué queda de los derechos más básicos en esta situación?

S: Justo por eso no acepto una teoría de derecho de propiedad absoluta. No permitiría que el Estado haga eso, ni que lo haga algún particular en su propia propiedad.

## Conclusión

Por resumir:

-   La ética de la libertad de Capella consta de varios axiomas
-   La justificación de tres de ellos (autopropiedad ,homesteading, restitución) apela a intuiciones sobre lo que es funcional, no a una imposibilidad lógica. Tales intuiciones (En el caso del argumento de Rothbard) descansan sobre el mismo error que el argumento para el principio de no agresión: no listar de forma verdaderamente las normas posibles.
-   El argumento que se da para el principio de no agresión  entra en conflicto con las normas que rigen los contratos y la restitución: Si sólo son válidas las normas universalmente fuertes en el sentido que quiere Capella, no pueden aceptarse otras normas además del Principio de No Agresión.
-   Tal argumento requiere de premisas contenidas fuera de los criterios de funcionalidad: la teoría de propiedad liberal, y la imposición de que toda norma posible sea 'pura' y sin excepciones: las normas que considera Capella no son todas, sino sólo las combinaciones simples de los operadores deónticos y las acciones posibles.

Por tanto, no sólo la ética de la libertad depende de varias premisas cuestionables, que están más allá de criterios de funcionalidad, simetría y universalidad, sino que es internamente incoherente.

Por tanto, la ética de la libertad no sigue necesariamente si se aceptan criterios de funcionalidad, simetría y universalidad y se razona lógicamente a partir de ahí. Salvo que uno tenga un concepto de funcionalidad que sea idéntico a las consecuencias de la ética de la libertad, pero entonces se cae en una petición de principios.

## Apéndice: Universalidad fuerte

Capella habla de universalidad fuerte como 'Las normas no son universales respecto al agente, sino también respecto a los receptores de efectos de la acción.'. Esta noción parece bastante razonable. Querríamos excluir normas que privilegien a personas concretas. 'Yo tengo derecho a mandar sobre ti' (Pero tú no tienes derecho a mandar sobre mí). 'Yo puedo robar a los demás, (Pero los demás no pueden robarme a mí). El test de validez para la universalidad fuerte es ver si las normas resultantes de probar a intercambiar los sujetos referidos en la norma también se aceptan. Si proponemos 'Yo tengo derecho a X siempre que Y  sea amigo de Z', e.g. Yo tengo derecho a escribir en mi blog siempre que Pedro sea amigo de Pablo), debemos aceptar también que Pablo tiene derecho a escribir en su blog  siempre que yo sea amigo de Pablo, y que Pedro tiene derecho a escribir en su blog siempre que yo sea amigo de Pablo.

Imaginemos una norma que Capella ve claramente que la universalidad fuerte prohibe: 'Los ricos deben dar dinero a los pobres'. Como los que proponen esta norma no aceptan que 'Los pobres deben dar dinero a los ricos', no es universal en sentido fuerte. Pero imaginemos que la norma que se propone es 'Si la renta de X está por encima de un umbral, debe dar dinero a los varios Y que están debajo del umbral'. Esta norma dice lo mismo, prácticamente, pero sí es universalmente fuerte. Ya no se refiere a personas concretas. De igual manera, la norma no universal fuerte 'Los pelirrojos deben obedecer toda orden de los rubios' puede convertirse en universal fuerte si 'X debe obedecer a Y si X es pelirrojo e Y es rubio'.

Esta es una de las críticas que históricamente se le han hecho a Kant. Puede que no pueda pedir que 'Permitido robar' sea una máxima universal, pero sí puedo pedir que 'Permitido a mí robar a tal persona tal día tal objeto', pues ello no termina contradiciendo el objeto de mi acción.

Por esto Capella insiste tanto en que en las normas no figuren no sólo personas concretas, sino tampoco circunstancias o cualquier otra cosa concreta. Con esta universalidad máxima reforzada es posible eliminar los contraejemplos que he propuesto. Pero entonces tampoco podemos justificar los otros principios de la ética de la libertad a excepción del principio de no agresión. Pero sin poder justificar la teoría de propiedad (Se habla de quién es el primero, por ejemplo) , el principio se queda cojo: la propiedad no quedaría definida, y no sabríamos qué es y no es agresión.

## Obras citadas

Alesina, A. (2003). The size of countries: Does it matter?. _Journal of the European Economic Association_, _1_(2‐3), 301-316.

Alexander, S. (2014) You Kant dismiss universalizability. Slate Star Codex.

Alexander, S. (2015) Extremism in thought experiment is no vice. Slate Star Codex.

Alexander, S. (2011) The Consequentialism FAQ.

Brennan, J. (2007). Rawls’ Paradox. _Constitutional Political Economy_, _18_(4), 287-299.

Brennan, J. (2013) Social Justice and the Legitimacy of Property. Bleeding Heart Libertarians.

Caplan, B., Hanson, R. The Caplan-Hanson debate: Liberty vs Efficiency.

Caplan, B. (2009) Are grotesque hypotheticals cheap shots?. Econlog.

Eabrasu, M. (2013). Rothbard’s and Hoppe’s justifications of libertarianism A critique. _Politics, Philosophy & Economics_, _12_(3), 288-307.

Esplugas, A. (2009) Intuicionismo ético. Blog personal.

Greene, J. (2014). _Moral tribes: emotion, reason and the gap between us and them_. Atlantic Books Ltd.

Habyarimana, J., Humphreys, M., Posner, D. N., & Weinstein, J. M. (2007). Why does ethnic diversity undermine public goods provision?. _American Political Science Review_, _101_(04), 709-725.

Huemer, M. (2005). Ethical intuitionism. Basingstoke \[England\]: Palgrave Macmillan.

Huemer, M. (2008). Revisionary intuitionism. _Social Philosophy and Policy_, _25_(01), 368-392.

Huemer, M. (2010). Lexical priority and the problem of risk. _Pacific Philosophical Quarterly_, _91_(3), 332-351.

Long, R. T. (1994). 95.“Slavery Contracts and Inalienable Rights: A Formulation,” Formulations Vol. 2, No. 2(Winter).

Murray, D. Nozick, Robert: Political Philosopy. Internet Encyclopedia of Philosophy.

Rawls, J. (1980). Kantian constructivism in moral theory. _The journal of philosophy_, 515-572.

Rothbard, M. N., & Hoppe, H. H. (2003). _The ethics of liberty_. NYU Press.

Sumner, S. (2014) Beyond left and right. Econlog.

Stanford Encyclopedia of Philosophy

-   Intuitionism in Ethics
-   Kant's Moral Philosophy
-   Moral Cognitivism vs Non-Cognitivism

Utilitarian.org Common criticisms of utilitarianism.

Varden, H. (2010). Kant and Lying to the Murderer at the Door... One More Time: Kant's Legal Philosophy and Lies to Murderers and Nazis. _Journal of social philosophy_, _41_(4), 403-421.

Volokh, S. (2011) Asteroid defense and libertarianism. The Volokh Conspiracy.

Wikipedia. Entry on Yangism

Zwolinski, M. (2012) Reading the Ethics of Liberty, Part 4. Bleeding Heart Libertarians

Zwolinski, M. (2013) Locke and Nozick on the Justification of Property. Libertarianism.org.