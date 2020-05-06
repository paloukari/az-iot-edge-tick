# TICK on the Azure IoT Edge

To deploy the TICK stack on the Azure IoT Edge, you need to build and push the module containers, and push the deployment manifest provided in this repo.

> This repo is based on the official [TICK sandbox repo](https://github.com/influxdata/sandbox) and has been lightly modified to make it compatible with the VS Code development extension structure for Azure IoT Edge.

## To build and push and deploy

There are two different ways to build push and deploy this stack on the edge:

### 1. Using `docker-compose` and `az cli`

You can use the provided `docker-compose.yml` file to build and push the module containers:

Clone this repo:

``` bash
git clone https://github.com/paloukari/az-iot-edge-tick
cd az-iot-edge-tick
```

Edit .env file to set your container registry:

``` bash
vim .env
```

Build and push the containers:

``` bash
docker-compose build
docker-compose push
```

Edit the generated deployment manifest to match the pushed container names, registry and registry credentials:

``` bash
vim config/deployment.amd64.json
```

Push the manifest:

``` bash
az iot edge set-modules --device-id [device id] --hub-name [hub name] --content ./config/deployment.amd64.json
```

### 2. Using VS Code

This repo is compatible with the VS Code Azure IoT Edge extension structure.

1. Clone this repo and open the project:

``` bash
git clone https://github.com/paloukari/az-iot-edge-tick
cd az-iot-edge-tick
code .
```
> Make sure you have the **Azure IoT Edge extension** installed.

2. Edit .env file and set your container registry and credentials.

3. Hit F1 and type Azure IoT Edge: *Generate IoT Edge deployment manifest*
   
4. Hit F1 and type Azure IoT Edge: *Build and push IoT Edge Solution*

5. In the Azure IoT Hub VS Code pane, right click on a device and select *Create deployment for single device*

## Fire up the IoT Edge Device on your local environment and test the TICK stack

Get a device connection string and run
``` 
docker run -it -d --privileged -p 8888:8888 -e connectionString='YOUR DEVICE CONNECTION STRING' toolboc/azure-iot-edge-device-container
```
> This command will deploy and start the TICK stack inside a single container

Launch Chronograf at: http://localhost:8888