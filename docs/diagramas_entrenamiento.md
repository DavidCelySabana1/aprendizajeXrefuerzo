# Esquemas del entrenamiento

> **Propósito:** explicar de forma sencilla el recorrido de una observación
> hasta la actualización del agente.
>
> **Alcance:** implementación de `mountain_car`, sin incluir detalles internos
> de Gymnasium o PyTorch.
>
> **Fuente:** `src/mountain_car/agents/qlearning.py` y
> `src/mountain_car/agents/dqn.py`.
>
> **Fecha:** 2026-09-20. Los resultados numéricos se generan en
> `notebooks/comparacion_agentes.ipynb`.

## Q-Learning tabular

```mermaid
flowchart LR
    A[Estado observado<br/>posición y velocidad] --> B[Convertir a una celda<br/>de la tabla]
    B --> C{¿Explorar?}
    C -->|Sí| D[Elegir acción al azar]
    C -->|No| E[Elegir la acción<br/>con mejor valor Q]
    D --> F[Ejecutar acción en el entorno]
    E --> F
    F --> G[Recibir recompensa y<br/>nuevo estado]
    G --> H[Calcular objetivo:<br/>recompensa + futuro estimado]
    H --> I[Actualizar Q de la<br/>celda y acción]
    I --> B
```

## DQN

```mermaid
flowchart LR
    A[Estado observado] --> B[Red neuronal Q]
    B --> C[Valores para las 3 acciones]
    C --> D{¿Explorar?}
    D -->|Sí| E[Elegir una acción y<br/>mantenerla por un bloque]
    D -->|No| F[Elegir el valor Q<br/>más alto]
    E --> G[Ejecutar acción]
    F --> G
    G --> H[Guardar transición<br/>en memoria]
    H --> I[Tomar un grupo de<br/>transiciones]
    I --> J[Red objetivo calcula<br/>el valor futuro]
    J --> K[Comparar valor actual<br/>contra objetivo]
    K --> L[Ajustar la red principal]
    L --> M[Copiar la red cada<br/>cierto número de episodios]
    M --> B
```

### Leyenda

- `Estado`: posición y velocidad actuales del automóvil.
- `Recompensa`: `-1` por cada paso; menos negativo significa que llegó antes.
- `Red principal`: aprende con las experiencias.
- `Red objetivo`: sirve como referencia más estable para calcular el futuro.
