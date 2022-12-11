# PPO LuxAI

## Introducción

La noche es oscura y está llena de terrores. Dos equipos deben luchar contra la oscuridad, recoger recursos y avanzar a través de los días. El día se convierte en una carrera desesperada para reunir los recursos que te permitan atravesar la inminente noche mientras haces crecer tu ciudad. Planifica y expande con cuidado: cualquier ciudad que no produzca suficiente luz será consumida por la oscuridad.

El Desafío Lux AI es una competición en la que los competidores diseñan agentes para abordar un problema de optimización, recolección de recursos y asignación de múltiples variables en un escenario 1v1 contra otros competidores.

![](https://github.com/Lux-AI-Challenge/Lux-Design-2021/raw/master/assets/daynightshift.gif)

## Para empezar

Necesitarás Node.js versión 12 o superior. Consulta las instrucciones de instalación [aquí](https://nodejs.org/en/download/), puedes simplemente descargar la versión recomendada.

Abre la línea de comandos, e instala el diseño de la competición con

```
git clone https://github.com/glmcdona/LuxPythonEnvGym.git
cd LuxPythonEnvGym
python setup.py install
```

Puedes ignorar las advertencias que aparezcan, son inofensivas. Para ejecutar una partida desde la línea de comandos (CLI), simplemente ejecute

```
python train.py
```

La partida se ejecutará y almacenará los registros de errores y una repetición en una nueva carpeta. Los registros almacenados en los errores incluirán toda la salida de errores y cualquier cosa impresa en el error estándar por su agente. Puede ver la repetición almacenada en la carpeta de repeticiones utilizando el [visualizador](https://2021vis.lux-ai.org/). 

## Objetivo
Tener la mayor cantidad de CityTiles al final del juego, lo cual está determinado por las condiciones de victoria. 

### Condiciones de victoria

Después de 360 turnos, el ganador será el equipo que tenga más CityTiles en el mapa. Si hay un empate, gana el equipo que tenga más unidades en el tablero. Si sigue habiendo un empate, la partida se marca como un empate.

Una partida puede terminar antes de tiempo si un equipo ya no tiene más unidades o CityTiles. Entonces el otro equipo gana.

Para conocer las condiciones del entorno y las restricciones viste este [enlace](https://www.lux-ai.org/specs-2021), junto con la documentación de la API vea este [documento](https://github.com/Lux-AI-Challenge/Lux-Design-2021/tree/master/kits)

## Un poco de teoría

### Proximal Policy Optimization (PPO) 
Es una familia de algoritmos de aprendizaje por refuerzo libre de modelos desarrollada por OpenAI en 2017. Los algoritmos PPO son métodos de gradiente de políticas, lo que significa que buscan en el espacio de políticas en lugar de asignar valores a pares estado-acción.

Los algoritmos PPO tienen algunos de los beneficios de los algoritmos de optimización de políticas de región de confianza (TRPO), pero son más simples de implementar, más generales y tienen una mejor complejidad de muestreo

Existen dos variantes principales de PPO: PPO-Penalty y PPO-Clip.

- PPO-Penalty resuelve aproximadamente una actualización con restricciones KL como TRPO, pero penaliza la divergencia KL en la función objetivo en lugar de convertirla en una restricción dura, y ajusta automáticamente el coeficiente de penalización a lo largo del entrenamiento para que se escale adecuadamente.

- PPO-Clip no tiene un término de divergencia KL en el objetivo y no tiene ninguna restricción. En su lugar, se basa en un recorte especializado en la función objetivo para eliminar los incentivos para que la nueva política se aleje de la antigua.

Para mas detalle ver [aquí](https://spinningup.openai.com/en/latest/algorithms/ppo.html)


## Agente
El repositorio cuenta con un agente que utiliza PPO, implementado en la biblioteca [stable baseline](https://stable-baselines3.readthedocs.io/en/master/modules/ppo.html)

- **agent_policy :** Este agente para cada turno puede tomar alguna de las acciones que se encuentra en el [documento](https://www.lux-ai.org/specs-2021) este consta con una politica MLP al cual se le pueden establecer learning_rate, gamma, gae_lambda. Este se entreno durante 3.700.000 step  

## Observaciones y apuntes 

Durante el entrenamiento del agente se realizaron diferentes checkpoints de su entrenamiento a los 1.233.333, 2.466.666 y 3.700.000 step utilizando dos semillas diferentes para establecer el tablero de juego, las semillas tiene el valor de 0 y 2. A partir del entrenamiento de se puede apreciar la evolucion de el comportamiento aprendido el cual se asemeja a un comportamiento greedy esto se debe principalmente a que el oponente no es un agente activo.

Como trabajo futuro realizar un aprendizaje de este mismo agente frente al modelo entrenado a los 3.700.000 step para medir su desepeño contran si mismo y ademas constar con un agente activo.