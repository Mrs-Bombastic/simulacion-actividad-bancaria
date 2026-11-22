# Simulación de cajeros bancarios — Actividad didáctica 2-M3

Proyecto académico de **simulación de eventos discretos en Python** para evaluar el funcionamiento de los cajeros del Banco de Colombia y comparar alternativas de distribución entre **retiros** y **consignaciones o pagos**.

## Objetivo

Estimar, mediante **30 réplicas de jornadas de ocho horas (480 minutos)**, los tiempos de atención y de espera, la cantidad de usuarios por tipo, el desempeño individual de los cajeros y la conveniencia de asignar cajas exclusivas o incorporar una adicional.

## Descripción del caso

El banco dispone inicialmente de tres cajeros. El **70 %** de los usuarios realiza retiros y el **30 %** restante hace consignaciones o pagos. Los usuarios se clasifican en **rápidos, normales, lentos y muy lentos**. Los tiempos medios y las probabilidades de cada categoría proceden de las tablas del enunciado de la actividad.

Se comparan cuatro escenarios:

1. **3 mixtos:** todos los cajeros realizan ambas operaciones.
2. **1 retiro + 2 pagos:** un cajero exclusivo para retiros y dos para pagos.
3. **2 retiros + 1 pago:** dos cajeros exclusivos para retiros y uno para pagos.
4. **4 cajeros (2+2):** dos cajeros para retiros y dos para pagos.

## Tecnologías y requisitos

- **Python 3.9 o superior**.
- **Google Colab**, opcional, para ejecutar el notebook desde el navegador.
- El programa utiliza únicamente bibliotecas estándar de Python (`random`, `csv`, `collections`, `pathlib` y `math`); **no requiere instalar paquetes adicionales**.

## Estructura del repositorio

```text
simulacion-cajeros-banco/
├── README.md
├── simulacion_banco.py
├── simulacion_banco_colab.ipynb
├── resultados_replicas.csv
├── resultados_cajeros.csv
├── resultados_usuarios.csv
└── Informe_Simulacion_Cajeros_Banco.pdf   # Agregar el informe final
```

> Los archivos CSV se generan nuevamente al ejecutar el programa. El PDF se incorpora una vez finalizado el informe académico.

## Cómo ejecutar el proyecto

### Opción 1: Google Colab

1. Abrir [Google Colab](https://colab.research.google.com/).
2. Seleccionar **Archivo → Subir cuaderno**.
3. Subir `simulacion_banco_colab.ipynb`.
4. Ejecutar las celdas del notebook.
5. Revisar la salida **MEDIA DE LAS 30 RÉPLICAS** y descargar los CSV si se requieren como evidencia.

### Opción 2: Python local

1. Descargar o clonar el repositorio.
2. Abrir una terminal en la carpeta que contiene `simulacion_banco.py`.
3. Ejecutar:

```bash
python simulacion_banco.py
```

El programa muestra las estadísticas de los cuatro escenarios y guarda los tres archivos CSV en la misma carpeta del archivo Python. Si se pega el código directamente en una celda de Colab, los archivos se guardan en el directorio de trabajo del cuaderno.

## Metodología

- **Horizonte:** 480 minutos por réplica.
- **Número de réplicas:** 30.
- **Semilla:** `20261122 + número de réplica`, para reproducibilidad.
- **Operación:** retiro con probabilidad 0,70 y pago con probabilidad 0,30.
- **Categoría:** se selecciona conforme a las probabilidades condicionadas a la operación.
- **Llegadas y servicios:** se generan tiempos exponenciales con las medias definidas en el enunciado.
- **Asignación:** cada cajero tiene un servidor y una cola FIFO; el usuario se dirige al cajero habilitado con la menor espera proyectada.
- **Comparación:** se utiliza la misma secuencia de usuarios para todos los escenarios de una misma réplica.
- **Cierre:** dejan de generarse llegadas al minuto 480. Se distinguen los usuarios que terminaron su atención de quienes quedan pendientes.

**Aclaración del modelo:** los parámetros de llegada de las tablas no definen explícitamente una tasa global. Se adopta una **única secuencia de llegadas**, seleccionando primero la clase del usuario y después su intervalo entre llegadas. Esto forma una mezcla de distribuciones exponenciales: cada cajero tiene una cola de un solo servidor, pero el proceso agregado **no es un M/M/1 homogéneo estricto**. Los indicadores de espera y atención se calculan sobre usuarios cuyo servicio terminó dentro de la jornada.

## Resultados

Promedios obtenidos en las **30 réplicas**, comprobados con la ejecución en Google Colab:

| Escenario | Llegadas | Atendidos | Pendientes al cierre | Espera promedio |
|---|---:|---:|---:|---:|
| 3 cajeros mixtos | 203,933 | 202,133 | 1,800 | 0,472 min |
| 1 retiro + 2 pagos | 203,933 | 200,333 | 3,600 | 5,346 min |
| 2 retiros + 1 pago | 203,933 | 201,000 | 2,933 | 2,496 min |
| 4 cajeros (2+2) | 203,933 | 202,267 | 1,667 | 0,541 min |

En el escenario de **tres cajeros mixtos**, el cajero **3** obtuvo el menor tiempo medio de servicio (**2,987 min**) y el cajero **1** el mayor (**3,496 min**). Estos promedios reflejan la composición aleatoria de usuarios atendidos, no velocidades intrínsecas distintas entre cajeros.

La **réplica 16** registró la menor cantidad de llegadas, con **181 usuarios**.

## Archivos de resultados

| Archivo | Contenido |
|---|---|
| `resultados_replicas.csv` | Totales por categoría, usuarios atendidos y pendientes, tiempos de espera y otros indicadores por escenario y réplica. |
| `resultados_cajeros.csv` | Estadísticas individuales de los cajeros: atención, espera, ocupación y usuarios terminados. |
| `resultados_usuarios.csv` | Registros de cada usuario simulado: operación, tipo, llegada, inicio, finalización, tiempo de servicio y espera. |

## Conclusiones

Los **tres cajeros mixtos** presentan el menor tiempo medio de espera de los escenarios evaluados (**0,472 min**). Por tanto, si el banco permite atención indistinta de retiros y pagos, esta es la alternativa recomendada. Si exige exclusivamente tres cajeros separados por operación, la distribución de **dos para retiros y uno para pagos** obtiene una espera global menor que la configuración inversa (**2,496 frente a 5,346 min**), aunque los usuarios de pago pueden experimentar esperas mayores.

No se justifica un cuarto cajero **solo por el tiempo promedio de espera** al compararlo con el escenario mixto. Para una decisión operativa real también sería necesario valorar los costos y validar los supuestos con datos históricos.


## Integrantes

- [Eulices Morales]
- [Andres Jaramillo]
- [Karen Vergara]

**Asignatura:** [simulacion]  
**Institución:** [IU Digital]  
**Docente:** [Julian Loaiza]
