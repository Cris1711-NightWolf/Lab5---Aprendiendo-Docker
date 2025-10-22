# Lab 5: Aprendiendo Docker - Implementación de Entornos Robóticos

## 📋 Descripción del Laboratorio
Este laboratorio demuestra la implementación de entornos robóticos utilizando Docker, integrando herramientas como ROS, MoveIt, Gazebo y PyBullet. Se desarrollan simulaciones con robots TurtleBot3 (incluyendo sistemas LIDAR+SLAM) y un cuadrúpedo, explorando su potencial en el contexto del Internet de las Cosas (IoT).

## 🚀 Ejemplos Implementados

### 1. Robot Sencillo con ROS + MoveIt + Gazebo
- Configuración básica de TurtleBot3 en Docker
- Control de movimiento mediante teclado
- Simulación en Gazebo con RViz

### 2. Robot con LIDAR + SLAM (TurtleBot3 + gmapping)
- Implementación de sistema de mapeo simultáneo
- Integración de sensor LIDAR virtual
- Navegación autónoma en entorno simulado

### 3. Simulación de Cuadrúpedo con PyBullet
- Control de robot cuadrúpedo complejo
- Simulación física avanzada
- Containerización de entorno PyBullet

## 🛠️ Paso a Paso - Comandos Explicados

### Instalación de Docker
# Actualizar sistema
sudo apt update && sudo apt upgrade -y

# Instalar dependencias
sudo apt install apt-transport-https ca-certificates curl gnupg lsb-release

# Añadir clave GPG oficial
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

# Añadir repositorio
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Instalar Docker Engine
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io

## Ejemplo B - Robot LIDAR + SLAM 

### Construccion de la Imagen Docker

mkdir turtlebot3_lidar_slam && cd turtlebot3_lidar_slam

docker build -t turtlebot3_slam .

### Ejecucion con soporte grafico

xhost +local:docker

ocker run -it --rm \
    --env="DISPLAY" \
    --env="QT_X11_NO_MITSHM=1" \
    --volume="/tmp/.X11-unix:/tmp/.X11-unix:rw" \
    turtlebot3_slam

### Lanzamiento de la Simulacion Slam

source /opt/ros/noetic/setup.bash

roslaunch turtlebot3_gazebo turtlebot3_world.launch

roslaunch turtlebot3_slam turtlebot3_slam.launch slam_methods:=gmapping

roslaunch turtlebot3_navigation turtlebot3_navigation.launch

## 🔧 Mejoras Implementadas en el Sistema LIDAR+SLAM

### Optimizaciones Realizadas

1. Configuración de Parámetros gmapping

- Ajuste de maxUrange para mejor detección de obstáculos
- Optimización de particles para mayor precisión en localización
- Configuración de delta para resolución de mapa adecuada

2. Integración con Docker

- Montaje de volumen para persistencia de mapas generados
- Configuración de red Docker para comunicación entre nodos ROS
- Optimización de layers para reducir tamaño de imagen

3. Mejoras de Rendimiento

- Uso de roscore centralizado dentro del contenedor
- Configuración de QoS en topics ROS
- Optimización de recursos de GPU para procesamiento LIDAR

## 📦Subida a Docker Hub

### Proceso de publicacion

1. Preparacion de la imagen

docker images

docker tag turtlebot3_slam tu_usuario/turtlebot3_slam:latest

docker images

2. Login a Docker Hub

docker login

Username: tu_usuario
Password: ********

3. Subir la imagen

docker push tu_usuario/turtlebot3_slam:latest

docker search tu_usuario/turtlebot3_slam

### Uso de la imagen publicada

docker run -it --rm \
    --env="DISPLAY" \
    --volume="/tmp/.X11-unix:/tmp/.X11-unix:rw" \
    tu_usuario/turtlebot3_slam:latest


------------

## 🎯 Trayectorias Desarrolladas para el Robot LIDAR+SLAM

### Estrategias de Navegacion

1. Mapeo Completo de Entorno

- Exploración sistemática con cobertura total

- Evitación reactiva de obstáculos

- Optimización de ruta para eficiencia energética

2. Navegacion por Waypoints

- Definición de puntos de interés

- Planificación de trayectorias óptimas

- Corrección en tiempo real con SLAM

3. Segumiento de Objetivos Dinamicos

- Actualización continua del mapa

- Replanificación adaptativa

- Integración con sistemas IoT externos

### 👥 Autores

- Cristian David Losada

- Yojan Arley Contreras

### Institucion

Universidad Santo Tomás - Digitales 3
