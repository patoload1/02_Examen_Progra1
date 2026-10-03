# 06 — Simulacro integrador (semana 16)

**Universidad Nacional · Escuela de Informática · EIF-201 Programación I**  
**Práctica de examen** · Tiempo: **3 horas** · Total: **100 puntos**  
**Fecha de referencia del examen:** domingo 8 de noviembre, 1:00 p.m.

Manuscrito. Individual. Sin computadora, sin STL. Se evalúa sintaxis, lógica, memoria y diseño.

## Materia

Herencia, listas enlazadas, Ley de Demeter, matrices automáticas y dinámicas, polimorfismo, `dynamic_cast` y colecciones polimórficas. El contenido es acumulativo (punteros, `new`/`delete`, encapsulamiento).

---

## Dominio: taller mecánico «Rueda Viva»

- Un **Vehiculo** es abstracto: `placa` (`string`), `kilometraje` (`int`). Método puro `double costoRevision() const`. Método puro `string categoria() const`. Destructor virtual.
- **Automovil** es un vehículo: `int cilindros`. Costo de revisión: `15000 + cilindros * 2000`. Categoría `"Automovil"`.
- **Motocicleta** es un vehículo: `bool altoCilindraje` (true si la cilindrada supera 250 cc, el dato ya viene calculado). Costo: `8000` si no es alto cilindraje; `12000` si lo es. Categoría `"Motocicleta"`.
- El **Taller** posee una **lista enlazada** de `Vehiculo*` (el taller es dueño de cada vehículo). Nodo: `Vehiculo* dato` y `Nodo* sig`. Solo se guarda la cabeza.
- El taller también posee la **agenda del día**: matriz **dinámica** de enteros `cupos[bahias][franjas]` (bahías = filas, franjas horarias = columnas). Cada celda guarda cuántos vehículos están anotados (inicia en 0). Las dimensiones se reciben en el constructor (`bahias > 0`, `franjas > 0`).

```cpp
class Taller {
private:
    string nombre;
    Nodo* cabeza;          // lista de Vehiculo*
    int** cupos;           // matriz dinámica
    int bahias;
    int franjas;
public:
    // métodos de las partes II, III y IV
};
```

---

## Parte I. Trazado (15 pts · 3 pts c/u)

Use la jerarquía del dominio (constructores que solo imprimen el nombre de la clase, como en el archivo 01). Escriba la salida de:

```cpp
Vehiculo* v = new Automovil("ABC123", 40000, 4);
cout << v->categoria() << endl;
cout << v->costoRevision() << endl;
Motocicleta* m = dynamic_cast<Motocicleta*>(v);
cout << (m ? "moto" : "no-moto") << endl;
delete v;
```

Suponga que `Automovil` y `Motocicleta` imprimen en su destructor `"~Automovil"` y `"~Motocicleta"`, y `Vehiculo` imprime `"~Vehiculo"`, y que los destructores son virtuales. Liste las líneas en orden.

---

## Parte II. Lista polimórfica del taller (35 pts)

1. **(8 pts)** Constructor de `Taller` (nombre, bahías, franjas): cabeza en `nullptr`, crea la matriz `cupos` en ceros. Destructor: libera **cada vehículo y cada nodo** de la lista, y libera la matriz (cada fila y luego el arreglo de filas).
2. **(7 pts)** `bool ingresar(Vehiculo* v)`: rechaza `nullptr` y placa repetida (recorra la lista y compare `getPlaca()`). Inserta al inicio. No viola Demeter: la comparación de placa es responsabilidad de `Vehiculo`, no de un objeto interno del vehículo.
3. **(6 pts)** `bool retirar(string placa)`: busca, desenlaza, hace `delete` del vehículo y del nodo, retorna `true` si lo encontró.
4. **(7 pts)** `double totalRevisiones() const`: suma `costoRevision()` de todos los vehículos **sin** preguntar el tipo (polimorfismo).
5. **(7 pts)** `int contarMotocicletas() const`: cuenta solo con `dynamic_cast<Motocicleta*>`. `string masCaro() const`: retorna la **placa** del vehículo con mayor `costoRevision()`. Lista vacía → `""`. Empate → el más cercano a la cabeza.

---

## Parte III. Agenda (matriz) (20 pts)

1. **(8 pts)** `bool anotar(int bahia, int franja)`: si los índices son válidos, incrementa `cupos[bahia][franja]` y retorna `true`. Si no, `false` (no accede fuera de rango).
2. **(6 pts)** `int totalBahia(int bahia) const`: suma la fila. Fila inválida → `0`.
3. **(6 pts)** `int franjaMasOcupada() const`: retorna el **índice de columna** cuya suma es mayor. Empate → el menor índice. Si no hay franjas, retorne `-1`.

---

## Parte IV. Ley de Demeter (10 pts)

Hoy alguien escribió, dentro de `main`:

```cpp
double saldo = taller->getCabeza()->dato->getDueno()->getCuenta()->getSaldo();
```

1. **(4 pts)** Explique por qué viola la Ley de Demeter (quién conoce a quién).
2. **(6 pts)** Proponga la firma de **un** método de `Taller` que resuelva la necesidad (“obtener el saldo del dueño del primer vehículo”) sin que `main` recorra nodos ni cuentas. No implemente clases `Dueno` ni `Cuenta` que no existan en el enunciado: basta la firma y una línea de justificación. Si el dato del dueño **no** está en el modelo, diga qué atributo faltaría y en qué clase viviría para no romper Demeter.

---

## Parte V. Diseño corto (20 pts)

Responda en prosa breve (no código largo).

1. **(5 pts)** ¿`Automovil` **es un** `Vehiculo` o el `Taller` **tiene** vehículos? Nombre la relación de cada caso (herencia / composición).
2. **(5 pts)** ¿Por qué `Vehiculo` no debe poder hacer `new Vehiculo(...)`? ¿Qué palabra del lenguaje lo impide?
3. **(5 pts)** Si `retirar` hace `delete v` y el destructor de `Vehiculo` **no** es virtual, ¿qué destructores se ejecutan al retirar un `Automovil`? ¿Qué recurso de la derivada puede quedar sin liberar?
4. **(5 pts)** La agenda podría haber sido `int cupos[8][12]` automático. Dé una razón para preferir la matriz dinámica del enunciado y una razón para preferir la automática.

---

**Fin del simulacro**
