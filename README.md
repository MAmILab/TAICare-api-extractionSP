# tapo-api

Código en Rust para extraer información de enchufes inteligentes TAPO P110 y almacenarla en una base de datos MongoDB Atlas para permitir su correlación con Actividades de la Vida Diaria y su implicación en la detección de patrones anormales[^1]. 

## Instalación y ejecución en Rasberry Pi

Si se instala en una Raspberry Pi limpia, tras actualizar e instalar todas las librerías, hará falta tener instalado "Rust", "open-ssl" y "arp-scan" en el dispositivo. 

```bash
sudo apt update
sudo apt upgrade

curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
sudo apt install libssl-dev
sudo apt install arp-scan
```
Tener instalado "git" y clonar el repositorio.

```bash
sudo apt-get install git
sudo git clone https://github.com/MAmILab/TAICare-api-extractionSP.git
```

Para su ejecución, habrá que establecer las variables de entorno "TAPO_USERNAME" (nombre de usuario con el que se registran los dispositivos en la aplicación Tapo - https://play.google.com/store/apps/details?id=com.tplink.iot&hl=en), "TAPO_PASSWORD" (contraseña con la que se registran los dispositivos en la aplicación Tapo), "MONGODB_URI" (URI de la BBDD - añadir permiso de acceso IP Raspberry Pi en MongoDB Atlas), "USE_DOCKER". 

```bash
export TAPO_USERNAME=
export TAPO_PASSWORD=
export MONGODB_URI=
export USE_DOCKER='false'

cargo build
cargo run --release
```

## Ejecución desde un paquete contenedor Docker

En caso de querer ejecutar esta API utilizando Docker para una mayor independencia del hardware, se utilizará la siguiente imagen:

```bash
docker pull -a taicareuser/taicare-docker-hub-respository
docker run -e TAPO_USERNAME='nombre_usuario' -e TAPO_PASSWORD='password' -e MONGODB_URI='mongodb_uri' -e USE_DOCKER='true' --net=host taicareuser/taicare-docker-hub-respository
```
[!NOTE]
El "docker engine" para el sistema operativo de la Raspberry Pi debe estar instalado.

De igual manera, para esta solución tambien habrá que consultar la URI de MongoDB y cerciorarse de que la IP desde la que se trabaja está añadida a la lista de permitidas en MongoDB Atlas.

[^1]Aplicación creada por @Adri-Sanchez-Miguel como parte del proyecto: TAICare (TED2021-2021-130296A-100 financiado por MICIU/AEI /10.13039/501100011033 y por la Unión Europea NextGenerationEU/PRTR).
