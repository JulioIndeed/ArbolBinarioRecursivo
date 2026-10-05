## Preguntas sobre el árbol binario de búsqueda

El árbol inicial se construye insertando, en este orden, las claves `50, 30, 20, 40, 70, 60, 80`.

### Estructura y recorridos

1. **¿Qué propiedad debe cumplir todo árbol binario de búsqueda?**  
   Para cada nodo, las claves menores se encuentran en su subárbol izquierdo y las mayores en su subárbol derecho. Esta regla se cumple recursivamente en todos los subárboles.

2. **¿Cuál es la raíz del árbol construido?**  
   La raíz es `50`, el primer valor insertado.

3. **¿Qué nodos son hojas?**  
   Las hojas son `20`, `40`, `60` y `80`; no tienen hijos.

4. **¿Qué valores pertenecen a los subárboles izquierdo y derecho de 50?**  
   El subárbol izquierdo contiene `30, 20, 40`; el derecho contiene `70, 60, 80`.

5. **¿Qué secuencia se obtiene con el recorrido inorden?**  
   `20 30 40 50 60 70 80`. El recorrido inorden visita primero el subárbol izquierdo, luego el nodo y finalmente el subárbol derecho.

### Método de búsqueda

- **¿Por qué no es necesario recorrer todos los nodos para buscar una clave?**  
  En cada nodo, la comparación con la clave buscada permite descartar uno de los dos subárboles y continuar solo por el que podría contenerla.

- **Si se busca 40, ¿qué nodos se visitan y en qué orden?**  
  `50 → 30 → 40`. En 50 se continúa a la izquierda y en 30, a la derecha.

- **Si se busca 90, ¿qué condición permitirá concluir que no existe?**  
  La búsqueda llega a una referencia `null` tras seguir `50 → 70 → 80` y luego intentar avanzar a la derecha. Ese caso indica que la clave no está en el árbol.

- **¿Qué valor booleano debe regresar el caso base cuando el nodo actual es `null`?**  
  `false`, porque no se encontró la clave.

- **¿Qué ocurriría si el árbol no respetara la regla menor-izquierda y mayor-derecha?**  
  La búsqueda podría descartar la rama equivocada y no encontrar una clave que sí está en el árbol.

### Método de eliminación

- **¿Por qué la eliminación requiere más casos que la búsqueda?**  
  Además de localizar el nodo, debe conservar la estructura y la propiedad del árbol. El tratamiento depende de si el nodo tiene cero, uno o dos hijos.

- **¿Qué debe ocurrir si la clave que se desea eliminar no existe?**  
  El árbol debe permanecer sin cambios.

- **¿Por qué eliminar un nodo hoja es el caso más sencillo?**  
  Basta con reemplazar la referencia de su padre (o la raíz, si corresponde) por `null`; no hay subárboles que reconectar.

- **Si un nodo tiene solamente un hijo, ¿por qué puede devolverse directamente la referencia a ese hijo?**  
  El hijo ocupa el lugar del nodo eliminado y conserva sus propios subárboles, sin perder nodos ni alterar el orden.

- **¿Por qué el menor valor del subárbol derecho es un candidato adecuado para sustituir a un nodo con dos hijos?**  
  Es mayor que todos los valores del subárbol izquierdo y es el menor de los que estaban a la derecha; por ello puede ocupar el lugar del nodo y mantener el orden.

- **Después de copiar el valor sustituto, ¿por qué todavía es necesario eliminarlo de su ubicación original?**  
  Para evitar que el mismo valor quede duplicado en dos nodos.

- **¿Qué riesgo existiría si se eliminara un nodo con dos hijos sin reconectar correctamente sus subárboles?**  
  Se podrían perder nodos al dejar subárboles desconectados, o romper la propiedad de orden del árbol.

- **¿Por qué eliminar la raíz puede modificar la variable `raiz` del árbol?**  
  La raíz también puede ser el nodo eliminado. Por eso `eliminar` asigna a `raiz` el nodo que devuelve la operación recursiva: el hijo que la reemplaza o `null`.

- **¿Qué propiedad debe seguir cumpliendo el árbol después de cualquier eliminación?**  
  Para cada nodo, todos los valores del subárbol izquierdo deben ser menores y todos los del derecho mayores; además, los subárboles deben seguir conectados correctamente.

### Método auxiliar: encontrar el valor mínimo

- **¿Hacia qué dirección debes desplazarte para encontrar el mínimo?**  
  Hacia la izquierda, siguiendo los hijos izquierdos.

- **¿Qué condición indica que ya encontraste el nodo mínimo?**  
  El nodo actual no tiene hijo izquierdo (`izquierdo == null`).

- **¿Cuál es el mínimo del subárbol cuya raíz es 70 en el árbol inicial?**  
  `60`.

### Diagrama del Árbol:

        50
       /  \
      30   70
     / \   / \
   20  40 60 80
  
### Reflexiones Finales:

¿Cómo ayuda el recorrido inorden a comprobar que el ABB conserva su estructura?
El recorrido inorden ayuda a comprobar que el ABB conserva su estructura porque lee primero el subárbol izquierdo, luego la raíz y después el derecho. Si el árbol está bien formado, la secuencia que se obtiene debe estar ordenada y confirma que cada nodo está colocado en el lugar correcto.

El caso de eliminación que se me hizó más difícil fue cuando el nodo tiene dos hijos, porque hay que conservar ambos lados del subárbol y mantener la propiedad del ABB. Se tiene que buscar el menor valor del subárbol derecho, se copia su valor al nodo a eliminar y luego se elimina ese valor en su posición original. Se me hizo más complicado porque se reordenan las referencias sin perder nodos ni romper la estructura.

¿Qué papel cumple la recursividad en los métodos de búsqueda y eliminación?
En la búsqueda, la comparación con la raíz permite decidir si continuar por la izquierda o por la derecha. En la eliminación, la recursividad también hace que cada caso se resuelva localmente y después se retorne la referencia correcta al padre. Esto hace que el código sea más simple y que cada problema se reduzca a un caso menor, hasta llegar a una hoja o un valor nulo.

¿Qué aprendiste sobre el cambio de referencias entre nodos al eliminar elementos?
Aprendí que para eliminar un nodo no solo se puede quitar, se tienen que reasignar referencias y conservar la relación entre padre, hijo y orden de los datos. Tanto el valor del nodo como las conexiones cambian. Si esas referencias no se actualizan bien, el árbol puede quedar incompleto, con subárboles desconectados o con claves duplicadas. 

Si tuvieras que explicar a un compañero la diferencia entre buscar y eliminar en un ABB, ¿qué le dirías? 
Buscar solo lee y compara valores hasta encontrar el valor que buscas o terminar el recorrido. En cambio, en la eliminación se reestructura el árbol además de localizar el nodo, para que el árbol siga funcionando.
