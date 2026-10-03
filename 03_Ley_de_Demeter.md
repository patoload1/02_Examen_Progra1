# 03 — Ley de Demeter (semana 13)

**Tiempo sugerido:** 30 minutos · **15 puntos**

Un método solo debe hablar con: el propio objeto, sus atributos, sus parámetros y los objetos que él mismo crea. No encadene getters (`a.getB().getC().hacer()`).

---

## Ejercicio 1. Detectar violaciones (6 pts)

Marque cada fragmento como **cumple** o **viola** la Ley de Demeter. Si viola, subraye la cadena.

```cpp
// (a) dentro de Taller::cerrarOrden
double total = orden->getCliente()->getCuenta()->getSaldo();

// (b) dentro de Taller::cerrarOrden
double total = orden->saldoDelCliente();

// (c) dentro de Clinica::atender
medico->registrarConsulta(paciente, diagnostico);

// (d) dentro de Hospital::resumen
int n = piso->getAla()->getSala(3)->getCama(2)->getPaciente()->getEdad();
```

---

## Ejercicio 2. Refactorizar (9 pts)

Se tiene:

```cpp
class Direccion {
    string provincia, canton;
public:
    string getProvincia() const;
    string getCanton() const;
};

class Cliente {
    string nombre;
    Direccion* direccion;          // el Cliente posee la dirección
public:
    Direccion* getDireccion() const;
};

class Envio {
public:
    // VIOLACIÓN: el Envío conoce la estructura interna de Cliente
    bool esLocal(Cliente* c) const {
        return c->getDireccion()->getProvincia() == "Alajuela";
    }
};
```

1. **(3 pts)** Escriba el método que debe existir en `Cliente` para que `Envio` no viole Demeter.
2. **(3 pts)** Reescriba `Envio::esLocal` usando solo ese método.
3. **(3 pts)** ¿`Envio` debe recibir `Direccion*` como parámetro para leer la provincia, o debe pedírselo a `Cliente`? Justifique en tres líneas (diga cuál opción respeta “díselo, no lo preguntes”).
