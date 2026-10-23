# Modelado del Proyecto Integrador en UML: Eco-Runner

Este documento contiene la especificación de diseño UML para el videojuego Eco-Runner.

---

## 1. Diagrama de Casos de Uso General

Representa las interacciones principales del jugador dentro del juego.

```mermaid
graph TD
    Jugador((Jugador))

    subgraph Eco-Runner - Sistema de Juego
        CU1[Navegar por ubicaciones]
        CU2[Recolectar basura contra tiempo]
        CU3[Clasificar residuos en contenedores]
        CU4[Ganar monedas]
        CU5[Consultar progreso y temporizador]
    end

    Jugador --> CU1
    Jugador --> CU2
    Jugador --> CU3
    Jugador --> CU4
    Jugador --> CU5

    CU2 .-> CU3 : <<include>>
    CU3 .-> CU4 : <<include>>

classDiagram
    class Jugador {
        +int idJugador
        +string nombre
        +int monedasObtenidas
        +mover()
        +recolectarBasura(Basura b)
        +clasificarBasura(Basura b, ContenedorReciclaje c)
    }

    class Nivel {
        +string numeroNivel
        +float tiempoLimite
        +int metaResiduos
        +string ubicacion
        +iniciarNivel()
        +verificarTiempo()
        +finalizarNivel()
    }

    class Basura {
        +int idBasura
        +string tipo
        +desaparecer()
    }

    class ContenedorReciclaje {
        +int idContenedor
        +string tipoAceptado
        +validarResiduo(Basura b) bool
    }

    class SistemaRecompensa {
        +int multiplicadorPuntos
        +calcularMonedas(int residuosCorrectos) int
    }

    Jugador "1" -- "*" Basura : recolecta >
    Jugador "1" -- "*" ContenedorReciclaje : deposita en >
    Nivel "1" *-- "*" Basura : contiene
    Nivel "1" *-- "3" ContenedorReciclaje : posee
    SistemaRecompensa ..> Jugador : otorga monedas a
