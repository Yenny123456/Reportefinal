## Reportefinal

Proceso Funcion total <- AhorroPuro(anio)
		Si anio = 0 Entonces
			total <- 1000 // Inversión inicial
		Sino
			// El sistema recalcula todo desde el principio cada vez
			total <- AhorroPuro(anio - 1) * 1.10 
		FinSi
FinFuncion

Algoritmo BancoLento
    Definir anio Como Entero
    Escribir "Año a consultar (ejemplo 25):"
    Leer anio
    Escribir "Calculando (espere, estoy repitiendo cálculos)..."
    Escribir "Resultado: $", AhorroPuro(anio)
FinAlgoritmo

## Fibonacciahorropuro

Proceso Algoritmo FibonacciAhorroPuro
		Definir n, i, a, b Como Entero
		
		Escribir "Términos a generar:"
		Leer n
		
		a <- 0
		b <- 1
		
		Para i <- 1 Hasta n Hacer
			Escribir a
			
			// Ahorro de memoria: Actualizamos b usando a, y luego recuperamos el nuevo a
			// Esto evita usar una tercera variable 'temporal'
			b <- a + b
			a <- b - a
		FinPara
		
FinAlgoritmo

## CambioGreedy

Algoritmo CambioGreedy
		Definir monto, i Como Entero
		Dimension monedas[3]
		monedas[1]<-4; monedas[2]<-3; monedas[3]<-1 // Ordenadas de mayor a menor
		
		Escribir "Monto a cambiar:"
		Leer monto
		
		Escribir "Monedas entregadas (Greedy):"
		Para i<-1 Hasta 3 Hacer
			Mientras monto >= monedas[i] Hacer
				Escribir monedas[i]
				monto <- monto - monedas[i]
			FinMientras
		FinPara
FinAlgoritmo

## MedicionRendimientoCambio

Proceso Algoritmo MedicionRendimientoCambio
		Definir pruebas, montoMax, i, j, montoActual Como Entero
		Definir tInicial, tFinalGreedy, tFinalPD Como Real
		
		pruebas <- 5000 // Cantidad de veces que pediremos cambio
		montoMax <- 200 // Monto máximo para cada prueba
		
		// -----------------------------------------------------------
		// TEST 1: ENFOQUE GREEDY (Rápido, pero impreciso)
		// -----------------------------------------------------------
		tInicial <- Tiempo_Actual // Captura tiempo inicial
		
		Para i <- 1 Hasta pruebas Hacer
			montoActual <- Azar(montoMax) + 1
			// Lógica Greedy simplificada
			Mientras montoActual > 0 Hacer
				Si montoActual >= 4 Entonces montoActual <- montoActual - 4
				Sino 
					Si montoActual >= 3 Entonces montoActual <- montoActual - 3
					Sino montoActual <- montoActual - 1
					FinSi
				FinSi
			FinMientras
		FinPara
		
		tFinalGreedy <- Tiempo_Actual - tInicial
		
		// -----------------------------------------------------------
		// TEST 2: PROGRAMACIÓN DINÁMICA (Lento, pero exacto)
		// -----------------------------------------------------------
		tInicial <- Tiempo_Actual
		
		Para i <- 1 Hasta pruebas Hacer
			montoActual <- Azar(montoMax) + 1
			
			// El ahorro puro aquí es re-usar el mismo arreglo para no saturar memoria
			Dimension tabla[montoActual + 1]
			tabla[0] <- 0
			Para j<-1 Hasta montoActual Hacer tabla[j] <- 999 FinPara
			
			// Cálculo de DP
			Para j<-1 Hasta montoActual Hacer
				Si j >= 1 Entonces
					Si tabla[j-1] + 1 < tabla[j] Entonces tabla[j] <- tabla[j-1] + 1 FinSi
				FinSi
				Si j >= 3 Entonces
					Si tabla[j-3] + 1 < tabla[j] Entonces tabla[j] <- tabla[j-3] + 1 FinSi
				FinSi
				Si j >= 4 Entonces
					Si tabla[j-4] + 1 < tabla[j] Entonces tabla[j] <- tabla[j-4] + 1 FinSi
				FinSi
			FinPara
			// (Aquí no borramos la dimensión por limitaciones de PSeInt, 
			// pero en un lenguaje real se liberaría memoria).
		FinPara
		
		tFinalPD <- Tiempo_Actual - tInicial
		
		// -----------------------------------------------------------
		// RESULTADOS
		// -----------------------------------------------------------
		Escribir "--- Resultados para ", pruebas, " operaciones ---"
		Escribir "Tiempo Greedy: ", tFinalGreedy, " segundos."
		Escribir "Tiempo Prog. Dinámica: ", tFinalPD, " segundos."
		Escribir "La diferencia es de: ", tFinalPD / tFinalGreedy, " veces más lento."
		
FinAlgoritmo

## ReflexionFinal

Proceso
	Característica 	Memorización	Tabulación
	Enfoque	Descendente (Top-Down)	Ascendente (Bottom-Up)
	Estado	Almacena resultados en caché (Hash Map/Array)	Rellena una tabla (usualmente un Array)
	Eficiencia	Puede ser más lenta por la sobrecarga recursiva	Suele ser más rápida y eficiente en memoria

  ## Taller de Programacion Dinamica 

  Proceso Desde un punto de vista más técnico, la complejidad es específicamente O(??), donde ? (phi) es la proporción áurea (~1.618), pero se simplifica a O(2?) para indicar que es de orden exponencial.
	Aquí te detallo por qué ocurre ese "desastre" de eficiencia:
	Crecimiento Exponencial de Llamadas: Para calcular fib(n), necesitas fib(n-1) y fib(n-2). Esto genera un árbol binario de llamadas. La altura del árbol es n, y en cada nivel el número de llamadas aproximadamente se duplica.
		Superposición de Subproblemas: Como mencionaste, el algoritmo no tiene "memoria". Por ejemplo, para calcular fib(5), el sistema calcula fib(3) dos veces, fib(2) tres veces y fib(1) cinco veces. No recuerda que ya resolvió esos valores hace unos 

    ## Resumen Ejecutivo

    Proceso Seguridad de la Memoria: Como bien dices, al ser un proceso iterativo, no abres nuevos marcos de pila (stack frames). Esto evita el famoso error de Stack Overflow que suele ocurrir en el enfoque Top-Down (recursivo con memoización) cuando n es muy grande.
		Eficiencia Temporal y Espacial: La complejidad es O(n). De hecho, si solo necesitas el 100º número, ni siquiera necesitas una tabla completa; puedes usar solo tres variables para ir rotando los valores anteriores, reduciendo el espacio de O(n) a O(1).
				El reto de los Números Grandes: Un detalle importante es que el 100º número de Fibonacci es 354,224,848,179,261,915,075, un valor que supera por mucho el límite de un entero de 64 bits (uint

        ## Caso 2

        Proceso La programación dinámica es una técnica de optimización diseñada para resolver problemas complejos dividiéndolos en subproblemas más sencillos. A diferencia de otros enfoques, su esencia radica en evitar el trabajo redundante: cuando se resuelve un subproblema, el resultado se almacena (técnica conocida como memoización) para que, si vuelve a aparecer, se pueda consultar la respuesta instantáneamente. Este enfoque de "divide y vencerás con memoria" transforma algoritmos que de otro modo serían ineficientes y lentos en procesos rápidos y ejecutables.
					El impacto de esta técnica es masivo en el mundo tecnológico actual. En la bioinformática, es fundamental para el alineamiento de secuencias de ADN; en la economía, permite modelar decisiones financieras a largo plazo; y en la ingeniería de software, optimiza desde los motores de búsqueda hasta las rutas de GPS que calculan el trayecto más corto. Al reducir drásticamente el tiempo de cómputo, la programación dinámica permite que sistemas modernos procesen volúmenes de datos gigantescos que antes se c

          ## 

          Proceso Fallo Greedy: Desde el 5, mira el 2 y el 10. Elige el 2 porque es el menor local. Pero al elegir el 2, se ve obligado a tomar el 100 después. Total: 107.
	
FinProceso

## La Falacia Greedy

La respuesta corta es: Sí, casi siempre. En la computación moderna, la memoria RAM es barata y el tiempo de CPU (y del usuario) es extremadamente caro.
Aquí tienes los argumentos técnicos y de negocio para justificar ese intercambio de Memoria (Espacio) por Tiempo:
1. El Costo de Oportunidad (User Experience)
20 minutos no es "latencia", es una interrupción del flujo de trabajo.
En milisegundos: El usuario percibe la acción como instantánea. Esto permite interactividad, APIs en tiempo real y sistemas reactivos.
En 20 minutos: El usuario se va a tomar café, pierde el contexto o, peor aún, el proceso se vuelve un "batch" nocturno que retrasa la toma de decisiones.
2. La Economía de Recursos (CPU vs. RAM)
Mantener un proceso corriendo al 100% de CPU durante 20 minutos es mucho más costoso que ocupar unos cuantos megabytes o gigabytes de RAM en reposo.
Consumo energético: El uso intensivo de CPU calienta los servidores y consume electricidad constante.
Escalabilidad: Si 100 usuarios piden el cálculo y cada uno tarda 20 minutos, tu servidor colapsará. Si tarda milisegundos, puedes atender a miles con la misma infraestructura.

  ##
  


