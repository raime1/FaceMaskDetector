# Face Mask Detection

Este proyecto conciste en una aplicación para identificar si una persona tiene o no una mascarilla para la plataforma Jetson basado en una aplicación de Deepstream SDK. 

Esta aplicación se encuentra dentro de un contenedor Docker y utiliza Deepstream como base para la toma de imagenes de una camapara IP por medio del protocolo RTSP, la ejecución de la inferencia y la transmición de los datos mediante un stream RTSP. Se uso como base la aplicación de ejemplo [deepstream-imagedata-multistream](https://github.com/NVIDIA-AI-IOT/deepstream_python_apps/tree/master/apps/deepstream-imagedata-multistream) dada por Nvidia. 

## Iniciar aplicación
- El paquete `nvidia-docker` debe estar instalado. 
- Descargar o clonar el repositorio del proyecto
    ````
    $ https://github.com/raime1/FaceMaskDetector.git
    $ cd FaceMaskDetector
    ````
- Construir el contenedor Docker
    ````
    $ sudo docker build . -t mask_detection
    ````
- Iniciar el contenedor Docker creado
    ````
    sudo docker run --rm -it --gpus all \
        -v /home/$USER/videos:/videos \
        -e DISPLAY=$DISPLAY -v /tmp/.X11-unix/:/tmp/.X11-unix --net host \
        --name mask_detection-ds-container --hostname mask_detection \
        mask_detection bash
    ````
- Con el docker inicializado, ejecutar el script mask_detection.py e indicar la fuente de video RTSP con el siguiente formato 
    - `python3 mask_detection_app.py rtsp://<user>:<pass>@<camera-ip>`.
  Si la fuente de video no requiere credenciales se debe hacer de la siguiente manera
    - `python3 mask_detection_app.py rtsp://<camera-ip>`.
- Para poder visualizar los resultados se debe ejecutar la aplicación web que puede reproducir video RTSP
    ````
    sudo docker exec -it mask_detection-ds-container bash run_ui.sh
    ````
    Esta aplicación mostrara en el puerto 5000 una vista para ver los resultados del video.
