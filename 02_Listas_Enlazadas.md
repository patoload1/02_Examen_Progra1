# 02 — Listas enlazadas (semanas 10 y 11)

**Tiempo sugerido:** 70 minutos · **40 puntos**  
Sin STL. Cada nodo se crea con `new` y se libera con `delete`.

---

## Ejercicio 1. Lista de enteros (16 pts)

Implemente la clase `ListaEnteros` (lista simple, solo puntero a cabeza).

```cpp
struct NodoInt {
    int dato;
    NodoInt* sig;
    NodoInt(int d) : dato(d), sig(nullptr) {}
};

class ListaEnteros {
private:
    NodoInt* cabeza;
public:
    ListaEnteros();
    ~ListaEnteros();
    void insertarInicio(int v);          // O(1)
    void insertarFinal(int v);           // recorre hasta el último
    bool eliminar(int v);                // elimina la primera ocurrencia
    int contar() const;
    bool contiene(int v) const;
};
```

1. **(3 pts)** Constructor (cabeza en `nullptr`) y destructor (libera **todos** los nodos, lista vacía incluida).
2. **(3 pts)** `insertarInicio`.
3. **(4 pts)** `insertarFinal` (lista vacía y lista con elementos).
4. **(4 pts)** `eliminar`: caso cabeza, caso medio/final y valor inexistente (`false`).
5. **(2 pts)** `contar` y `contiene`.

---

## Ejercicio 2. Lista de objetos con dueño (16 pts)

Un laboratorio guarda muestras. La lista **es dueña** de cada `Muestra*` (composición).

```cpp
class Muestra {
private:
    string codigo;
    double concentracion;
    bool contaminada;
public:
    Muestra(string, double, bool);
    ~Muestra();
    string getCodigo() const;
    double getConcentracion() const;
    bool getContaminada() const;
};

struct NodoM {
    Muestra* dato;     // objeto dinámico
    NodoM* sig;
};
```

Implemente `RegistroMuestras`:

1. **(4 pts)** Atributos (`NodoM* cabeza`), constructor y destructor. El destructor debe hacer `delete` de la `Muestra` **y** del nodo. No doble `delete`.
2. **(4 pts)** `bool insertar(Muestra* m)`: rechaza `nullptr` y código repetido. Inserta **al inicio**.
3. **(4 pts)** `bool eliminar(string codigo)`: si existe, libera muestra y nodo, reenlaza y retorna `true`.
4. **(4 pts)** `int contarContaminadas() const` y `Muestra* buscarMayorConcentracion() const` (retorna `nullptr` si la lista está vacía; en empate, la primera que encuentre desde la cabeza).

---

## Ejercicio 3. Casos borde (8 pts)

Para la lista del ejercicio 1, indique el estado de `cabeza` y si hay fuga, después de cada secuencia. Escriba la lista resultante de izquierda (cabeza) a derecha.

| # | Secuencia | Lista resultante | ¿Fuga? |
|---|-----------|------------------|--------|
| a | `eliminar(7)` sobre lista vacía | | |
| b | insertar inicio 3, luego 1, luego 4; `eliminar(1)` | | |
| c | insertar inicio 8; `eliminar(8)` | | |
| d | insertar final 2, insertar final 2; `eliminar(2)` una sola vez | | |
