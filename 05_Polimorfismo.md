# 05 — Polimorfismo, dynamic_cast y colecciones polimórficas (semana 15)

**Tiempo sugerido:** 70 minutos · **40 puntos**  
Sin STL. Destructor de la base **virtual**. `dynamic_cast` solo si el cast tiene éxito (`!= nullptr`).

---

## Ejercicio 1. Jerarquía abstracta (12 pts)

Un parque cobra entrada según el tipo de visitante. `Visitante` es **abstracta** (no se puede instanciar).

- `Visitante`: `nombre` (`string`). Método puro `double tarifa() const`. Método puro `string tipo() const`. Destructor virtual.
- `Adulto`: tarifa fija `2500`. `tipo()` retorna `"Adulto"`.
- `Estudiante`: tiene `bool universidadPublica`. Si es true, tarifa `500`; si no, `1000`. `tipo()` retorna `"Estudiante"`.
- `AdultoMayor`: tarifa `0`. `tipo()` retorna `"AdultoMayor"`.

1. **(4 pts)** Declare `Visitante` con al menos un método `= 0`.
2. **(6 pts)** Implemente las tres derivadas con `override`.
3. **(2 pts)** Escriba una función libre `double cobrar(const Visitante& v)` que retorne `v.tarifa()` sin preguntar el tipo.

---

## Ejercicio 2. Colección polimórfica (18 pts)

`Parque` guarda hasta 100 visitantes dinámicos en un **arreglo automático de punteros**:

```cpp
class Parque {
private:
    Visitante* visitas[100];
    int cantidad;
public:
    Parque();
    ~Parque();
    bool registrar(Visitante* v);     // rechaza nullptr y si está lleno
    double recaudacion() const;       // suma tarifa() de todos
    int contarEstudiantes() const;    // solo con dynamic_cast<Estudiante*>
    int contarPorTipo(string t) const;// usa el método virtual tipo(), NO dynamic_cast
};
```

1. **(4 pts)** Constructor (`nullptr`, `cantidad = 0`) y destructor (libera cada visitante; el destructor virtual de la base garantiza el destructor correcto de la derivada).
2. **(4 pts)** `registrar`.
3. **(4 pts)** `recaudacion` (recorrido polimórfico, sin `if` por tipo).
4. **(3 pts)** `contarEstudiantes` con `dynamic_cast`.
5. **(3 pts)** `contarPorTipo` con `tipo()`. Explique en dos líneas cuándo conviene `dynamic_cast` y cuándo basta el método virtual.

---

## Ejercicio 3. Trazado polimórfico (10 pts)

Indique qué imprime. Justifique la tercera línea (por qué no dice `"Base"`).

```cpp
class Base {
public:
    virtual ~Base() { cout << "~Base "; }
    virtual void id() const { cout << "Base" << endl; }
};
class Der : public Base {
public:
    ~Der() { cout << "~Der "; }
    void id() const override { cout << "Der" << endl; }
};

int main() {
    Base* p = new Der();
    p->id();
    Der* d = dynamic_cast<Der*>(p);
    if (d) cout << "si" << endl; else cout << "no" << endl;
    Base* q = new Base();
    Der* e = dynamic_cast<Der*>(q);
    if (e) cout << "si" << endl; else cout << "no" << endl;
    delete p;
    delete q;
    cout << endl;
    return 0;
}
```
