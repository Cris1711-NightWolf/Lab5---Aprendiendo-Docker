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

