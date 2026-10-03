# 04 — Matrices automáticas y dinámicas (semanas 13 y 14)

**Tiempo sugerido:** 70 minutos · **35 puntos**  
Sin STL. Matriz dinámica = arreglo de punteros a filas (`T**`).

---

## Ejercicio 1. Tres estrategias de un tablero 8×8 (15 pts)

La clase `Casilla` ya existe (constructor por defecto, destructor, `setMarca(char)`, `getMarca()`).

Implemente **solo atributos, constructor y destructor** de `Tablero` en cada versión. Capacidad fija 8×8.

1. **(5 pts)** Matriz **automática** de casillas **automáticas**: `Casilla celdas[8][8];`
2. **(5 pts)** Matriz **dinámica** de casillas **automáticas**: `Casilla** celdas;` con `new Casilla*[8]` y cada fila `new Casilla[8]`. Destructor con `delete[]` por fila y luego del arreglo de filas.
3. **(5 pts)** Matriz **dinámica** de casillas **dinámicas**. Cada celda es un `Casilla*` creado con `new Casilla()`, así que el atributo es `Casilla***` (arreglo de filas, cada fila un arreglo de punteros). Inicialice las 64 celdas en el constructor. En el destructor: `delete` de cada objeto, `delete[]` de cada fila y `delete[]` del arreglo de filas.

---

## Ejercicio 2. Clase `MatrizEnteros` (12 pts)

```cpp
class MatrizEnteros {
private:
    int** datos;
    int filas;
    int columnas;
    void liberar();
public:
    MatrizEnteros(int f, int c);          // todo en 0; f>0 y c>0
    ~MatrizEnteros();
    void set(int i, int j, int v);        // ignora si está fuera de rango
    int get(int i, int j) const;          // retorna 0 si está fuera de rango
    int sumaFila(int i) const;            // 0 si la fila no existe
    int contarMayoresQue(int umbral) const;
};
```

Implemente los seis métodos. `set`/`get` no deben provocar acceso inválido.

---

## Ejercicio 3. Fuga (8 pts)

El destructor siguiente está incompleto. Reescríbalo bien y explique, en dos líneas, qué queda fugado y qué provocaría un doble `delete` si se invirtiera el orden.

```cpp
MatrizEnteros::~MatrizEnteros() {
    delete[] datos;          // solo esto
}
```

Suponga que el constructor hizo:

```cpp
datos = new int*[filas];
for (int i = 0; i < filas; i++)
    datos[i] = new int[columnas];
```
