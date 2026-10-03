# 01 — Herencia (semana 9)

**Tiempo sugerido:** 50 minutos · **30 puntos**

---

## Ejercicio 1. Trazado (10 pts)

Indique **exactamente** lo que imprime el programa, línea por línea, en el orden de ejecución.

```cpp
#include <iostream>
using namespace std;

class A {
protected:
    int x;
public:
    A(int v) : x(v) { cout << "A(" << x << ")" << endl; }
    virtual ~A() { cout << "~A" << endl; }
    virtual int valor() const { return x; }
};

class B : public A {
    int y;
public:
    B(int a, int b) : A(a), y(b) { cout << "B(" << y << ")" << endl; }
    ~B() { cout << "~B" << endl; }
    int valor() const override { return x + y; }
};

class C : public B {
    int z;
public:
    C(int a, int b, int c) : B(a, b), z(c) { cout << "C(" << z << ")" << endl; }
    ~C() { cout << "~C" << endl; }
    int valor() const override { return B::valor() + z; }
};

int main() {
    A* p = new C(2, 3, 4);
    cout << p->valor() << endl;
    delete p;

    B b(1, 5);
    A a = b;                 // copia por valor
    cout << a.valor() << endl;
    return 0;
}
```

Además, en una frase: ¿qué problema de diseño muestra la línea `A a = b;`?

---

## Ejercicio 2. Jerarquía manuscrita (16 pts)

Una clínica modela personal. **No** use STL.

- `Empleado` (base): `cedula` (`string`), `nombre` (`string`), `salarioBase` (`double`). Constructor de tres parámetros. Destructor virtual. Método virtual `double calcularPago() const` que retorna `salarioBase`. Método virtual `string rol() const` que retorna `"Empleado"`.
- `Medico` **es un** `Empleado`: agrega `especialidad` (`string`) y `guardias` (`int`). `calcularPago()` retorna `salarioBase + guardias * 25000`. `rol()` retorna `"Medico"`.
- `Enfermero` **es un** `Empleado`: agrega `turno` (`string`, `"dia"` o `"noche"`). Si el turno es `"noche"`, `calcularPago()` retorna `salarioBase * 1.15`; si no, retorna `salarioBase`. `rol()` retorna `"Enfermero"`.

Implemente declaración (`.h` conceptual) e implementación (`.cpp` conceptual) de las tres clases:

1. **(6 pts)** Atributos, especificadores de acceso y constructores encadenados (la derivada llama a la base en la lista de inicialización).
2. **(6 pts)** `calcularPago()` y `rol()` con `override`.
3. **(4 pts)** Destructores: el de la base debe ser `virtual`. Explique en dos líneas qué falla si no lo es y se hace `delete` de un `Medico` a través de un `Empleado*`.

---

## Ejercicio 3. Es-un falso (4 pts)

Marque cuáles relaciones **no** deben modelarse con herencia y diga por qué en una línea cada una.

1. `Motor` es un `Automovil`.
2. `CuentaAhorro` es una `Cuenta`.
3. `ListaPacientes` es un `Paciente`.
4. `Rectangulo` es una `Figura`.
