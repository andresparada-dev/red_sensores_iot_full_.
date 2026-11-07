# Criterios de Decisión: Estructuras de Datos en la Red de Sensores

Este documento es la parte más importante de tu entrega. No basta con que el código funcione; debes justificar **por qué** elegiste una colección de Java sobre otra.

Responde a las siguientes preguntas basándote en la teoría de nodos y el rendimiento del JCF.

### 1. El dilema del Historial
Para el `historialEventos`, podrías usar un `ArrayList` o un `LinkedList`. 
- Si la plataforma registra miles de eventos por segundo y necesitas eliminar los más antiguos frecuentemente para ahorrar memoria, ¿cuál de las dos colecciones es más eficiente y por qué? (Pista: piensa en el costo de eliminar el primer elemento).

- Respuesta: LinkedList, ya que un arreglo al quitar el primero, corre todos los demás una posición, lo que con muchos datos es mucho trabajo y costo, pero usando LinkedList es solo quitar el primero es solo mover el enlace de primeras al segundo nodo, mucho más fácil y menos trabajo.

### 2. Navegación Bidireccional
En el `navegadorAlertas`, utilizamos un `Deque` (Double Ended Queue). 
- ¿Qué ventaja ofrece un `Deque` sobre una `Queue` simple para implementar la funcionalidad de "Atrás" y "Adelante"?
- Respuesta: una Queue solo deja sacar por un lado. Un Deque deja meter y sacar por los dos extremos, lo que permite mayor control y poder hacer más fácil eliminación o agregación.
- ¿Cómo se relaciona esto con el concepto de un `NodoDoble`?
- Respuesta: Un Deque se parece a una lista hecha con NodoDoble, porque el NodoDoble tiene dos enlaces, siguiente y anterior, para moverse en los dos sentidos. El Deque ofrece eso mismo, moverse hacia adelante y hacia atrás, pero ya hecho por Java, sin tener que manejar los enlaces a mano.

### 3. El Ciclo de Monitoreo
Para la `colaMonitoreo`, implementaste una rotación (sacar del frente y meter al final).
- ¿Por qué una `Queue` es la estructura natural para este problema?
- Respuesta: Porque la Queue recibe los datos en orden de llegada. Cada sensor que se consulta vuelve al final de la fila y espera hasta que le toque otra vez, así todos pasan por su turno.
- ¿Qué pasaría con la equidad del sistema si usaras una `Stack` (Pila) en su lugar?
- Respuesta: Con una Stack, el último que entra es el primero que sale. Si sacas el de arriba y lo vuelves a poner arriba, siempre sale el mismo sensor, y los de abajo nunca serían consultados.

### 4. Topología de Red (El Grafo)
Representaste la red de sensores como un `Map<String, List<String>>`.
- ¿Por qué usamos un `Map` y no un arreglo bidimensional (matriz de adyacencia) considerando que la mayoría de los sensores solo están conectados a 2 o 3 vecinos?
- Respuesta: Una matriz de adyacencia guarda una casilla por cada pareja posible de sensores. Se desperdicia memoria pero el Map guarda solo las conexiones que existen.
- ¿Cómo se traduce esta estructura de `Map` a la idea de un `NodoGrafo` con una lista de adyacencias?
- Respuesta: Cada clave del Map es como un nodo del grafo, y su List de vecinos son sus enlaces. En una lista simple cada nodo tiene un enlace; en un grafo, cada nodo tiene una lista de enlaces.

### 5. Buffer de Ingesta y Rendimiento
El `bufferIngesta` procesa datos en orden FIFO.
- Si el volumen de datos aumenta drásticamente y necesitas procesar las lecturas en paralelo usando múltiples hilos, ¿seguiría siendo adecuado un `ArrayDeque` o deberías cambiar a una `ConcurrentLinkedQueue`? Justifica tu respuesta basándote en la seguridad de hilos (Thread-safety).
- Respuesta:  Yo opino que con un solo hilo, ArrayDeque está bien y es más rápida. Con varios hilos, hay que cambiar a ConcurrentLinkedQueue para la seguridad de hilos por quesi dos hilos sacan una lectura al mismo tiempo, los dos pueden sacar la misma lectura, o una lectura se puede perder, y la cola puede quedar dañada. ConcurrentLinkedQueue está hecha para que varios hilos metan y saquen al mismo tiempo sin chocar, y cada lectura sale una sola vez.
