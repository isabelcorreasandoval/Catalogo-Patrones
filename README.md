# Catálogo de Patrones de Diseño - Ingeniería de Software II

## Integrantes
* Isabel Correa Sandoval
* Irving Mezo ÁLvarez
* Valeria Rosales Arévalo
* Jorge Arauz Estrada


---

## Singleton
*Categoría: Creacional
* Propósito: Controla el acceso a un recurso compartido asegurando que solo exista una instancia activa en todo el ciclo de vida del software.
* **Estructura UML:**
```mermaid
classDiagram
    class Singleton {
        -static instance: Singleton
        -Singleton()
        +static getInstance() Singleton
    }
```  

Java 
```
public class Singleton {
    private static Singleton instance;
    private Singleton() {} // Constructor privado
    
    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

Python
```
class Singleton:
    _instance = None
    def __new__(cls):
        if cls._instance is None:
            cls._instance = super(Singleton, cls).__new__(cls)
        return cls._instance
```



---

## Factory Method
* Categoría: Creacional
* Propósito: Define una interfaz para crear objetos en una superclase, pero permite que las subclases alteren el tipo de objetos que se crearán según la necesidad.
* **Estructura UML:**
```mermaid
classDiagram
    class Creador {
        +metodoFabrica() Producto
    }
    class CreadorConcreto {
        +metodoFabrica() Producto
    }
    class Producto {
        <<interface>>
        +operacion()
    }
    Creador <|-- CreadorConcreto
    Producto <.. CreadorConcreto
 ```

Java
 ```
interface Producto { void operacion(); }

class ProductoConcreto implements Producto {
    public void operacion() { System.out.println("Lógica de producto en Java"); }
}

abstract class Creador {
    public abstract Producto fabricar();
}

class CreadorConcreto extends Creador {
    public Producto fabricar() { return new ProductoConcreto(); }
}
 ```

Python
 ```
from abc import ABC, abstractmethod

# Interfaz del Producto
class Notificacion(ABC):
    @abstractmethod
    def enviar(self):
        pass

# Producto Concreto
class EmailNotificacion(Notificacion):
    def enviar(self):
        print("Enviando notificación por Email...")

# Creador (Factory)
class FabricaNotificaciones(ABC):
    @abstractmethod
    def crear_notificacion(self) -> Notificacion:
        pass

# Creador Concreto
class FabricaEmail(FabricaNotificaciones):
    def crear_notificacion(self) -> Notificacion:
        return EmailNotificacion()

# Uso
fabrica = FabricaEmail()
noticia = fabrica.crear_notificacion()
noticia.enviar()

 ```

---

## Observer
* Categoría: Comportamiento
* Propósito: Define una suscripción automática para que múltiples objetos reaccionen al instante cuando un componente central cambia su estado.
* **Estructura UML:**
```mermaid
  classDiagram
    class Sujeto {
        -observadores: List
        +suscribir(o)
        +notificar()
    }
    class Observador {
        <<interface>>
        +actualizar()
    }
    Sujeto --> Observador
 ```

Java 

 ```
import java.util.*;

interface Observador { void actualizar(String m); }

class Sujeto {
    private List<Observador> lista = new ArrayList<>();
    public void suscribir(Observador o) { lista.add(o); }
    public void notificar(String msg) { lista.forEach(o -> o.actualizar(msg)); }
}

 ```

Python

 ```

class Sujeto:
    def __init__(self):
        self._observadores = []

    def suscribir(self, obj):
        self._observadores.append(obj)

    def notificar(self, mensaje):
        for o in self._observadores:
            o.actualizar(mensaje)

 ```

---

## Adapter
* Categoría: Estructural
* Propósito: Funciona como un puente entre dos interfaces incompatibles, permitiendo que clases que normalmente no podrían trabajar juntas lo hagan mediante un objeto "traductor".
* **Estructura UML:**
 ```mermaid
classDiagram
    class Target {
        <<interface>>
        +request()
    }
    class Adapter {
        -adaptee: Adaptee
        +request()
    }
    class Adaptee {
        +specificRequest()
    }
    Target <|-- Adapter
    Adapter --> Adaptee
```

Java 
```

// Interfaz que espera el cliente
interface ConectorSD { void leerDatos(); }

// Clase antigua o incompatible
class TarjetaMicroSD {
    public void lecturaRapida() { 
        System.out.println("Leyendo datos desde MicroSD..."); 
    }
}

// El Adaptador
class AdaptadorSD implements ConectorSD {
    private TarjetaMicroSD microSD = new TarjetaMicroSD();
    
    public void leerDatos() {
        microSD.lecturaRapida();
    }
}

```

Python
```
class TarjetaMicroSD:
    def lectura_rapida(self):
        print("Leyendo datos desde MicroSD...")

class AdaptadorSD:
    def __init__(self, microsd):
        self.microsd = microsd

    def leer_datos(self):
        # Traduce la petición al método que la clase entiende
        self.microsd.lectura_rapida()

# Uso
dispositivo = AdaptadorSD(TarjetaMicroSD())
dispositivo.leer_datos()

```

---

## Strategy
* Categoría: Comportamiento
* Propósito: Define una familia de algoritmos y los encapsula en clases separadas, permitiendo que el algoritmo que usa un objeto pueda cambiar en tiempo de ejecución según la necesidad.
**Estructura UML:**
``` mermaid
classDiagram
    class Contexto {
        -estrategia: Estrategia
        +setEstrategia(e)
        +ejecutar()
    }
    class Estrategia {
        <<interface>>
        +algoritmo()
    }
    class EstrategiaConcretaA {
        +algoritmo()
    }
    class EstrategiaConcretaB {
        +algoritmo()
    }
    Contexto o-- Estrategia
    Estrategia <|-- EstrategiaConcretaA
    Estrategia <|-- EstrategiaConcretaB
```

Java 
```
// Interfaz de la estrategia
interface EstrategiaPago { void pagar(int monto); }

// Estrategias concretas
class PagoTarjeta implements EstrategiaPago {
    public void pagar(int m) { System.out.println("Pagando " + m + " con Tarjeta"); }
}

class PagoPaypal implements EstrategiaPago {
    public void pagar(int m) { System.out.println("Pagando " + m + " con PayPal"); }
}

// Clase que usa la estrategia
class Carrito {
    private EstrategiaPago metodo;
    public void setMetodo(EstrategiaPago e) { this.metodo = e; }
    public void procesar(int m) { metodo.pagar(m); }
}

```

Python 
```
class PagoTarjeta:
    def pagar(self, monto):
        print(f"Pagando {monto} con Tarjeta")

class PagoPaypal:
    def pagar(self, monto):
        print(f"Pagando {monto} con PayPal")

class Carrito:
    def __init__(self, estrategia):
        self.estrategia = estrategia

    def procesar(self, monto):
        self.estrategia.pagar(monto)

# Uso: c = Carrito(PagoTarjeta()); c.procesar(100)

```

---

## Proxy
* Categoría: Estructural
* Propósito: Proporciona un sustituto o intermediario para otro objeto con el fin de controlar el acceso a él, permitiendo añadir lógica adicional como seguridad o carga perezosa.
**Estructura UML:**
``` mermaid
classDiagram
    class Sujeto {
        <<interface>>
        +peticion()
    }
    class SujetoReal {
        +peticion()
    }
    class Proxy {
        -sujetoReal: SujetoReal
        +peticion()
    }
    Sujeto <|-- SujetoReal
    Sujeto <|-- Proxy
    Proxy --> SujetoReal
```

Java
```
interface Internet { void conectar(String url); }

class InternetReal implements Internet {
    public void conectar(String url) { System.out.println("Conectando a: " + url); }
}

class ProxyInternet implements Internet {
    private InternetReal internet = new InternetReal();
    private static List<String> bloqueados = Arrays.asList("sitio-prohibido.com");

    public void conectar(String url) {
        if(bloqueados.contains(url)) System.out.println("Acceso Denegado");
        else internet.conectar(url);
    }
}

```

Python
```
class InternetReal:
    def conectar(self, url):
        print(f"Conectando a: {url}")

class ProxyInternet:
    def __init__(self):
        self.internet_real = InternetReal()
        self.bloqueados = ["sitio-prohibido.com"]

    def conectar(self, url):
        if url in self.bloqueados:
            print("Acceso Denegado por el Proxy")
        else:
            self.internet_real.conectar(url)

# Uso
red = ProxyInternet()
red.conectar("google.com")

```

## Abstract Method
*Categoría: Creacional
* Propósito: Proporciona una interfaz para crear familias de objetos relacionados sin especificar sus clases concretas.
* **Estructura UML:**
``` mermaid

classDiagram
    class AbstractFactory {
        <<interface>>
        +createProductA()
        +createProductB()
    }
    class ConcreteFactory1 {
        +createProductA()
        +createProductB()
    }
    AbstractFactory <|-- ConcreteFactory1

```

Java
```
interface AbstractFactory {
    Button createButton();
}
class WinFactory implements AbstractFactory {
    public Button createButton() { return new WinButton(); }
}

```

Python
```
from abc import ABC, abstractmethod

class AbstractFactory(ABC):
    @abstractmethod
    def create_button(self):
        pass

class WinFactory(AbstractFactory):
    def create_button(self):
        return WinButton()

```

## Prototype
**Categoría:** Creacional

**Propósito:** Permite copiar objetos existentes sin que el código dependa de sus clases, delegando el proceso de clonación al propio objeto que se está clonando.

### Estructura UML (Mermaid)
```mermaid
classDiagram
    class Prototipo {
        <<interface>>
        +clonar() Prototipo
    }
    class PrototipoConcreto {
        -campo
        +PrototipoConcreto(prototipo)
        +clonar() Prototipo
    }
    Prototipo <|-- PrototipoConcreto

```
java
```
// Interfaz
interface Prototipo extends Cloneable {
    Prototipo clonar();
}

// Clase Concreta
class Rectangulo implements Prototipo {
    private int ancho;
    private int alto;

    public Rectangulo(int w, int h) {
        this.ancho = w;
        this.alto = h;
    }

    // Lógica de clonación
    public Prototipo clonar() {
        return new Rectangulo(this.ancho, this.alto);
    }

    public String toString() {
        return "Rectangulo " + ancho + "x" + alto;
    }
}

// Uso
// Rectangulo r1 = new Rectangulo(10, 20);
// Rectangulo r2 = (Rectangulo) r1.clonar();
```
python
```
import copy

class Prototipo:
    def clonar(self):
        return copy.deepcopy(self)

class Rectangulo(Prototipo):
    def __init__(self, ancho, alto):
        self.ancho = ancho
        self.alto = alto

    def __str__(self):
        return f"Rectangulo {self.ancho}x{self.alto}"

# Uso
r1 = Rectangulo(10, 20)
r2 = r1.clonar()
# r2 es un objeto nuevo con los mismos valores
```

## Builder
*Categoría: Creacional
* Propósito: Permite construir objetos complejos paso a paso, permitiendo crear diferentes representaciones usando el mismo código.
* **Estructura UML:**

```mermaid
classDiagram
    class Director {
        -builder: Builder
        +construct()
    }
    class Builder {
        <<interface>>
        +buildPart()
    }
    class ConcreteBuilder {
        +buildPart()
        +getResult()
    }
    Director --> Builder
    Builder <|-- ConcreteBuilder
```

java
```
class PizzaBuilder {
    private String masa;
    public PizzaBuilder setMasa(String m) { this.masa = m; return this; }
    public Pizza build() { return new Pizza(masa); }
}
```
python
```
class PizzaBuilder:
    def __init__(self):
        self.masa = None
    def set_masa(self, masa):
        self.masa = masa
        return self
    def build(self):
        return Pizza(self.masa)
```

## Composite
*Categoría: Estructural
* Propósito: Permite componer objetos en estructuras de árbol para representar jerarquías de parte-todo.
* **Estructura UML:**
```mermaid
classDiagram
    class Component {
        <<interface>>
        +execute()
    }
    class Leaf {
        +execute()
    }
    class Composite {
        -children: List~Component~
        +add(Component)
        +execute()
    }
    Component <|-- Leaf
    Component <|-- Composite
    Composite o-- Component
```

java
```
interface Component { void execute(); }
class Composite implements Component {
    private List<Component> children = new ArrayList<>();
    public void execute() { children.forEach(Component::execute); }
}
```
python
```
class Component:
    def execute(self): pass

class Composite(Component):
    def __init__(self):
        self.children = []
    def execute(self):
        for child in self.children: child.execute()

```


## Decorator
*Categoría: Estructural
* Propósito: Permite añadir funcionalidades a objetos dinámicamente envolviéndolos en objetos decoradores.
* **Estructura UML:**
```mermaid
classDiagram
    class Component {
        <<interface>>
        +execute()
    }
    class Decorator {
        -wrappee: Component
        +execute()
    }
    Component <|-- Decorator
    Decorator o-- Component
```

java
```
class Decorator implements Component {
    protected Component component;
    public Decorator(Component c) { this.component = c; }
    public void execute() { component.execute(); }
}
```
python
```
class Decorator(Component):
    def __init__(self, component):
        self._component = component
    def execute(self):
        self._component.execute()
```

## Command
*Categoría: Comportamiento
* Propósito: Transforma una solicitud en un objeto independiente que contiene toda la información sobre la misma.
* **Estructura UML:**
```mermaid
classDiagram
    class Command {
        <<interface>>
        +execute()
    }
    class ConcreteCommand {
        -receiver: Receiver
        +execute()
    }
    class Invoker {
        -command: Command
        +setCommand()
    }
    Command <|-- ConcreteCommand
    Invoker --> Command

```

java
```

interface Command { void execute(); }
class SimpleCommand implements Command {
    public void execute() { System.out.println("Comando ejecutado"); }
}

```

python
```
class Command:
    def execute(self): pass

class SimpleCommand(Command):
    def execute(self):
        print("Comando ejecutado")

```



## State
*Categoría: Comportamiento
* Propósito: Permite que un objeto altere su comportamiento cuando su estado interno cambia.
* **Estructura UML:**
```mermaid
classDiagram
    class Context {
        -state: State
        +request()
    }
    class State {
        <<interface>>
        +handle()
    }
    class ConcreteState {
        +handle()
    }
    Context o-- State
    State <|-- ConcreteState

```

java
```
interface State { void handle(Context ctx); }
class Context {
    private State state;
    public void setState(State s) { this.state = s; }
    public void request() { state.handle(this); }
}
```

python
```
class State:
    def handle(self, context): pass

class Context:
    def __init__(self, state):
        self._state = state
    def request(self):
        self._state.handle(self)


```

## Bridge
*Categoría: Estructural
* Propósito:Desacopla una abstracción de su implementación para que ambas puedan variar independientemente (separa la jerarquía lógica de la jerarquía física).
* **Estructura UML:**
```mermaid
classDiagram
    class Abstraccion {
        -implementador: Implementador
        +operacion()
    }
    class Implementador {
        <<interface>>
        +procesar()
    }
    class ImplementadorConcretoA {
        +procesar()
    }
    Abstraccion o-- Implementador
    Implementador <|-- ImplementadorConcretoA


```

java
```
// Implementador (Interfaz)
interface Dispositivo {
    void encender();
    void apagar();
}

// Implementaciones Concretas
class TV implements Dispositivo {
    public void encender() { System.out.println("TV encendida"); }
    public void apagar() { System.out.println("TV apagada"); }
}

class Radio implements Dispositivo {
    public void encender() { System.out.println("Radio encendida"); }
    public void apagar() { System.out.println("Radio apagada"); }
}

// Abstracción
abstract class ControlRemoto {
    protected Dispositivo dispositivo;

    public ControlRemoto(Dispositivo d) {
        this.dispositivo = d;
    }

    public abstract void botonPower();
}

class ControlAvanzado extends ControlRemoto {
    public ControlAvanzado(Dispositivo d) { super(d); }

    public void botonPower() {
        System.out.println("Enviando señal remota...");
        dispositivo.encender();
    }
}

```

python
```
# Implementador
class Dispositivo:
    def encender(self): 
        pass

class TV(Dispositivo):
    def encender(self):
        print("TV encendida")

class Radio(Dispositivo):
    def encender(self):
        print("Radio encendida")

# Abstracción
class ControlRemoto:
    def __init__(self, dispositivo):
        self.dispositivo = dispositivo

    def boton_power(self):
        print("Enviando señal...")
        self.dispositivo.encender()

# Uso
tv = TV()
control = ControlRemoto(tv)
control.boton_power()


```

## Facade
*Categoría: Estructural
* Propósito: Proporciona una interfaz unificada y simplificada a un conjunto de interfaces en un subsistema, ocultando la complejidad a los clientes.
* **Estructura UML:**
```mermaid
classDiagram
    class Fachada {
        -sistemaA: SubsistemaA
        -sistemaB: SubsistemaB
        +operacionSimple()
    }
    class SubsistemaA {
        +operacionA()
    }
    class SubsistemaB {
        +operacionB()
    }
    Fachada --> SubsistemaA
    Fachada --> SubsistemaB


```

java
```
// Subsistemas complejos
class Luces {
    void on() { System.out.println("Luces ON"); }
}
class Audio {
    void on() { System.out.println("Audio ON"); }
}
class Proyector {
    void on() { System.out.println("Proyector ON"); }
}

// Fachada
class CineEnCasa {
    private Luces luces = new Luces();
    private Audio audio = new Audio();
    private Proyector proyector = new Proyector();

    public void verPelicula() {
        System.out.println("Preparando cine...");
        luces.on();
        audio.on();
        proyector.on();
    }
}

```

python
```
# Subsistemas
class Luces:
    def encender(self):
        print("Luces ON")

class Audio:
    def encender(self):
        print("Audio ON")

# Fachada
class CineEnCasa:
    def __init__(self):
        self.luces = Luces()
        self.audio = Audio()

    def ver_pelicula(self):
        print("Preparando modo cine...")
        self.luces.encender()
        self.audio.encender()

# Uso
cine = CineEnCasa()
cine.ver_pelicula()


```
## Flyweight
*Categoría: Estructural
* Propósito: Permite mantener una gran cantidad de objetos soportando el uso compartido eficiente de datos comunes (estado intrínseco) para reducir el consumo de memoria RAM.
* **Estructura UML:**
```mermaid
classDiagram
    class FlyweightFactory {
        -cache: Map
        +getFlyweight(key)
    }
    class Flyweight {
        -estadoIntrinseco
        +operacion(estadoExtrinseco)
    }
    FlyweightFactory o-- Flyweight


```

java
```
import java.util.HashMap;
import java.util.Map;

// Flyweight (Datos compartidos)
class TipoArbol {
    private String nombre; // Intrinseco
    private String color;  // Intrinseco

    public TipoArbol(String n, String c) {
        this.nombre = n;
        this.color = c;
    }

    public void dibujar(int x, int y) { // (x,y) es extrínseco
        System.out.println("Arbol " + nombre + " en " + x + "," + y);
    }
}

// Factory
class FabricaArboles {
    private static Map<String, TipoArbol> tipos = new HashMap<>();

    public static TipoArbol getTipo(String nombre) {
        if (!tipos.containsKey(nombre)) {
            tipos.put(nombre, new TipoArbol(nombre, "Verde"));
            System.out.println("Creando nuevo tipo: " + nombre);
        }
        return tipos.get(nombre);
    }
}

```

python
```
# Flyweight
class TipoArbol:
    def __init__(self, nombre):
        self.nombre = nombre  # Estado compartido

    def dibujar(self, x, y):
        print(f"Dibujando {self.nombre} en ({x}, {y})")

# Factory
class FabricaArboles:
    _tipos = {}

    @staticmethod
    def get_tipo(nombre):
        if nombre not in FabricaArboles._tipos:
            FabricaArboles._tipos[nombre] = TipoArbol(nombre)
            print(f"-> Creando tipo: {nombre}")
        return FabricaArboles._tipos[nombre]

# Uso
t1 = FabricaArboles.get_tipo("Pino")
t1.dibujar(10, 20)
t2 = FabricaArboles.get_tipo("Pino")  # Reusa el objeto existente
t2.dibujar(50, 60)


```
## Chain of Responsibility
*Categoría: Comportamiento
* Propósito: Pasa una solicitud a lo largo de una cadena de manejadores. Cada manejador decide si procesa la solicitud o la pasa al siguiente manejador de la cadena.
* **Estructura UML:**
```mermaid
classDiagram
    class Manejador {
        -siguiente: Manejador
        +setSiguiente(m)
        +manejarPedido(p)
    }
    class SoporteBasico {
        +manejarPedido(p)
    }
    class SoporteTecnico {
        +manejarPedido(p)
    }
    Manejador <|-- SoporteBasico
    Manejador <|-- SoporteTecnico
    Manejador o-- Manejador


```

java
```
abstract class Manejador {
    protected Manejador siguiente;

    public void setSiguiente(Manejador m) {
        this.siguiente = m;
    }

    public abstract void manejar(String problema);
}

class SoporteBasico extends Manejador {
    public void manejar(String problema) {
        if (problema.equals("basico")) {
            System.out.println("Resuelto por Soporte Básico");
        } else if (siguiente != null) {
            siguiente.manejar(problema);
        }
    }
}

class SoporteExperto extends Manejador {
    public void manejar(String problema) {
        System.out.println("Resuelto por Soporte Experto: " + problema);
    }
}

// Uso:
// SoporteBasico basico = new SoporteBasico();
// SoporteExperto experto = new SoporteExperto();
// basico.setSiguiente(experto);
// basico.manejar("complejo");

```

python
```
class Manejador:
    def __init__(self):
        self.siguiente = None

    def set_siguiente(self, manejador):
        self.siguiente = manejador
        return manejador

    def manejar(self, nivel):
        if self.siguiente:
            self.siguiente.manejar(nivel)

class SoporteBasico(Manejador):
    def manejar(self, nivel):
        if nivel < 10:
            print("Soporte Básico resuelve el problema.")
        elif self.siguiente:
            print("Básico no puede, pasando al siguiente...")
            self.siguiente.manejar(nivel)

class SoporteJefe(Manejador):
    def manejar(self, nivel):
        print("El Jefe resuelve el problema.")

# Uso
basico = SoporteBasico()
jefe = SoporteJefe()

basico.set_siguiente(jefe)
basico.manejar(20)


```

---

## Interpreter

* **Categoría:** Comportamiento
* **Propósito:** Define una representación de la gramática de un lenguaje junto con un intérprete para procesar expresiones.

**Estructura UML:**

```mermaid
classDiagram
    class AbstractExpression {
        <<interface>>
        +interpret(Context context)
    }
    class TerminalExpression {
        +interpret(Context context)
    }
    class NonTerminalExpression {
        -AbstractExpression child
        +interpret(Context context)
    }
    AbstractExpression <|-- TerminalExpression
    AbstractExpression <|-- NonTerminalExpression

```

**Java**

```java
interface Expression {
    int interpret();
}

class Number implements Expression {
    private int number;
    public Number(int number) { this.number = number; }
    public int interpret() { return number; }
}

```

**Python**

```python
class Expression:
    def interpret(self):
        pass

class Number(Expression):
    def __init__(self, value):
        self.value = value
    def interpret(self):
        return self.value

```

---

## Iterator

* **Categoría:** Comportamiento
* **Propósito:** Permite recorrer elementos de una colección sin exponer su estructura interna.

**Estructura UML:**

```mermaid
classDiagram
    class Iterable {
        <<interface>>
        +createIterator() Iterator
    }
    class Iterator {
        <<interface>>
        +hasNext() boolean
        +next() Object
    }
    Iterable <|.. ConcreteCollection
    Iterator <|.. ConcreteIterator
    ConcreteCollection ..> ConcreteIterator

```

**Java**

```java
import java.util.Iterator;

class NameRepository implements Iterable<String> {
    public String names[] = {"Juan", "Ana", "Luis"};
    public Iterator<String> iterator() {
        return new Iterator<String>() {
            int i = 0;
            public boolean hasNext() { return i < names.length; }
            public String next() { return names[i++]; }
        };
    }
}

```

**Python**

```python
class MyCollection:
    def __init__(self, items):
        self.items = items
    def __iter__(self):
        return iter(self.items)

```

---

## Mediator

* **Categoría:** Comportamiento
* **Propósito:** Centraliza la comunicación compleja entre objetos para evitar dependencias directas.

**Estructura UML:**

```mermaid
classDiagram
    class Mediator {
        <<interface>>
        +notify(sender, event)
    }
    class ConcreteMediator {
        -ComponentA ca
        -ComponentB cb
        +notify(sender, event)
    }
    Mediator <|.. ConcreteMediator

```

**Java**

```java
class Mediator {
    public void notify(String event) {
        System.out.println("Reaccionando a: " + event);
    }
}

```

**Python**

```python
class Mediator:
    def notify(self, sender, event):
        print(f"Mediador recibe {event} de {sender}")

```

---

## Memento

* **Categoría:** Comportamiento
* **Propósito:** Permite capturar y restaurar el estado previo de un objeto.

**Estructura UML:**

```mermaid
classDiagram
    class Originator {
        -state: String
        +save() Memento
        +restore(m: Memento)
    }
    class Memento {
        -state: String
        +getState() String
    }
    Originator ..> Memento

```

**Java**

```java
class Memento {
    private final String state;
    public Memento(String state) { this.state = state; }
    public String getState() { return state; }
}

```

**Python**

```python
class Memento:
    def __init__(self, state):
        self._state = state
    def get_state(self):
        return self._state

```

---

## Template Method

* **Categoría:** Comportamiento
* **Propósito:** Define el esqueleto de un algoritmo, delegando pasos específicos a las subclases.

**Estructura UML:**

```mermaid
classDiagram
    class AbstractClass {
        +templateMethod()
        #step1()*
        #step2()*
    }
    AbstractClass <|-- ConcreteClass

```

**Java**

```java
abstract class Game {
    abstract void start();
    abstract void end();
    public final void play() {
        start();
        end();
    }
}

```

**Python**

```python
class AbstractClass:
    def template_method(self):
        self.step_one()
        self.step_two()
    def step_one(self): raise NotImplementedError()
    def step_two(self): print("Paso común")

```

---

## Visitor

* **Categoría:** Comportamiento
* **Propósito:** Agrega nuevas operaciones a una estructura de objetos sin modificarlos.

**Estructura UML:**

```mermaid
classDiagram
    class Visitor {
        <<interface>>
        +visit(ElementA)
    }
    class Element {
        <<interface>>
        +accept(Visitor v)
    }
    Visitor <|.. ConcreteVisitor
    Element <|.. ElementA

```

**Java**

```java
interface Visitor { void visit(ElementA el); }
interface Element { void accept(Visitor v); }
class ElementA implements Element {
    public void accept(Visitor v) { v.visit(this); }
}

```

**Python**

```python
class Visitor:
    def visit_element_a(self, el): print("Visitando A")

class ElementA:
    def accept(self, visitor): visitor.visit_element_a(self)

```







  
