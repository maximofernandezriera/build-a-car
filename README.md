# build-a-car
https://www.codewars.com/kata/5832d6e2565e120ae60000bb

```
public class Car {
    // Propiedades públicas que representan las partes del coche.
    public CarBody body;
    public CarChassis chassis;

    // Constructor que recibe el largo total y el número de puertas.
    public Car(int length, int doors) {
        // Validación: el largo mínimo debe ser 7.
        if (length < 7) {
            throw new IllegalArgumentException("El largo mínimo es 7.");
        }
        // El espacio disponible para las puertas en la segunda capa es (length - 3).
        int interiorLength = length - 3;
        // Calculamos el máximo de puertas que caben (sin solaparse, cada puerta ocupa 2 caracteres).
        int maxDoors = (int) Math.ceil(interiorLength / 2.0);
        if (doors < 1) {
            throw new IllegalArgumentException("Debe haber al menos 1 puerta.");
        }
        if (doors > maxDoors) {
            throw new IllegalArgumentException("Número de puertas demasiado grande para el largo dado.");
        }
        
        // Creamos los objetos que componen el coche.
        body = new CarBody();
        chassis = new CarChassis();
        // La propiedad 'component' de 'body' contiene la primera y segunda capa.
        body.component = buildBody(length, doors);
        // La propiedad 'component' de 'chassis' contiene la tercera capa.
        chassis.component = buildChassis(length);
    }

    // Construye el cuerpo del coche (primera y segunda capa).
    private String buildBody(int length, int doors) {
        // La primera capa: un espacio seguido de guiones bajos. 
        // Según la especificación, su longitud es: length - 2.
        String top = " " + repeat('_', length - 3) + "\n";
        // La segunda capa: se construye con el método buildDoorsLine.
        String middle = buildDoorsLine(length - 1, doors) + "\n";
        return top + middle;
    }

    // Construye la línea de las puertas (segunda capa).
    // La línea tiene 'len' caracteres: empieza con '|' y termina con '\' .
    private String buildDoorsLine(int len, int doors) {
        char[] line = new char[len];
        line[0] = '|';
        line[len - 1] = '\\';
        // Rellenamos el interior con espacios.
        for (int i = 1; i < len - 1; i++) {
            line[i] = ' ';
        }
        // El interior disponible para puertas es de longitud (len - 2).
        int interior = len - 2;
        // Para evitar solapamientos, los bloques de puertas ("[]") ocuparán posiciones válidas:
        // se pueden colocar únicamente en posiciones pares (0, 2, 4, …) siempre que quepan dos caracteres.
        java.util.ArrayList<Integer> posValidas = new java.util.ArrayList<>();
        for (int i = 0; i <= interior - 2; i += 2) {
            posValidas.add(i);
        }
        // Se asignan las puertas alternando: la primera se coloca al frente (derecha),
        // la segunda al fondo (izquierda), y así sucesivamente.
        for (int i = 0; i < doors; i++) {
            int pos;
            if (i % 2 == 0) { // Frente: tomamos la última posición válida.
                pos = posValidas.remove(posValidas.size() - 1);
            } else { // Fondo: tomamos la primera posición válida.
                pos = posValidas.remove(0);
            }
            // Colocamos el bloque "[]" en el interior.
            // Recordemos que el interior comienza en el índice 1 de la línea.
            int idx = pos + 1;
            line[idx] = '[';
            line[idx + 1] = ']';
        }
        return new String(line);
    }

    // Construye el chasis (tercera capa) con el marco, las ruedas y el faro.
    private String buildChassis(int length) {
        // El patrón mínimo (para un coche sin ejes adicionales) es: "-o--o-'"
        // Cuando el largo aumenta, se añaden ejes extra:
        // El tercer eje se añade en length = 12 y a partir de ahí se agregan cada 2 caracteres adicionales.
        int extraAxles = (length < 12) ? 0 : ((length - 12) / 2 + 1);
        
        // Usamos un arreglo de caracteres para construir la línea, de tamaño 'length'.
        char[] line = new char[length];
        // Inicialmente, rellenamos todo con guiones.
        for (int i = 0; i < length; i++) {
            line[i] = '-';
        }
        // El último carácter es el faro.
        line[length - 1] = '\'';
        // Colocamos las ruedas fijas para el patrón mínimo:
        // una rueda izquierda en la posición 1 y una derecha en la posición (length - 3).
        int leftWheel = 1;
        int rightWheel = length - 3;
        line[leftWheel] = 'o';
        line[rightWheel] = 'o';
        
        // Si hay ejes adicionales, se añaden alternando hacia el interior del chasis,
        // comenzando por el lado izquierdo (parte trasera).
        int currentLeft = leftWheel;
        int currentRight = rightWheel;
        for (int i = 0; i < extraAxles; i++) {
            if (i % 2 == 0) {
                if (currentLeft + 2 < currentRight) {
                    currentLeft += 2;
                    line[currentLeft] = 'o';
                }
            } else {
                if (currentRight - 2 > currentLeft) {
                    currentRight -= 2;
                    line[currentRight] = 'o';
                }
            }
        }
        return new String(line);
    }

    // Método auxiliar que repite el carácter 'c' n veces.
    private String repeat(char c, int n) {
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < n; i++) {
            sb.append(c);
        }
        return sb.toString();
    }
}

// Clases auxiliares que representan las partes del coche.
// Cada una tiene una propiedad pública 'component' que almacena la cadena resultante.
class CarBody {
    public String component;
}

class CarChassis {
    public String component;
}
```
