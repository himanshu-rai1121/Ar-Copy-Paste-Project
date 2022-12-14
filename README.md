# AR Cut & Paste

An AR+ML prototype that allows cutting elements from your surroundings and pasting them in an image editing software.

Although only Photoshop is being handled currently, it may handle different outputs in the future.

Demo & more infos: [Thread](https://twitter.com/cyrildiagne/status/1256916982764646402)

## Modules

This prototype runs as 3 independent modules:

- **The mobile app**

  - Check out the [/app](/app) folder for instructions on how to deploy the app to your mobile.

- **The local server**

  - The interface between the mobile app and Photoshop.
  - It finds the position pointed on screen by the camera using [screenpoint](https://github.com/cyrildiagne/screenpoint)
  - Check out the [/server](/server) folder for instructions on configuring the local server

- **The object detection / background removal service**

  - For now, the salience detection and background removal are delegated to an external service
  - It would be a lot simpler to use something like [DeepLap](https://github.com/shaqian/tflite-react-native) directly within the mobile app. But that hasn't been implemented in this repo yet.

## Usage

### 1 - Configure Photoshop

- Go to "Preferences > Plug-ins", enable "Remote Connection" and set a friendly password that you'll need later.
- Make sure that your PS document settings match those in ```server/src/ps.py```, otherwise only an empty layer will be pasted.
- Also make sure that your document has some sort of background. If the background is just blank, SIFT will probably not have enough feature to do a correct match.

<!--
### 2) Setup the local server

```bash
virtualenv venv
source venv/bin/activate
pip install -r requirements.txt
``` -->

### 2 - Setup the external salience object detection service

#### Option 1: Set up your own model service (requires a CUDA GPU)

- As mentioned above, for the time being, you must deploy the
BASNet model (Qin & al, CVPR 2019) as an external HTTP service using this [BASNet-HTTP wrapper](https://github.com/cyrildiagne/basnet-http) (requires a CUDA GPU)

- You will need the deployed service URL to configure the local server

- Make sure to configure a different port if you're running BASNet on the same computer as the local service

#### Option 2: Use a community provided endpoint

A public endpoint has been provided by members of the community. This is useful if you don't have your own CUDA GPU or do not want to go through the process of running the servce on your own.

Use this endpoint by launching the local server with `--basnet_service_ip http://u2net-predictor.tenant-compass.global.coreweave.com`

### 3 - Configure and run the local server

- Follow the instructions in [/server](/server) to setup & run the local server.

### 4 - Configure and run the mobile app

- Follow the instructions in [/app](/app) to setup & deploy the mobile app.