# Run Google Colab locally in a Jupyter notebook
A simple way to run YOLO is in Google Colab however this can unstable when running in cloud. To run it locally in a Jupyter notebook, follow the steps below.

*Note: Google Colab expects to be run in Linux so will likely not work in Windows.*

## Setup
Install jupyter
```
pip install jupyter
```
Install Jupyter server extension for using a WebSocket to proxy HTTP traffic
```
pip install jupyter_http_over_ws
```
Enable the extension
```
jupyter serverextension enable --py jupyter_http_over_ws
```

## Start
Start jupyter notebook server
```
jupyter notebook \
    --NotebookApp.allow_origin='https://colab.research.google.com' \
    --port=8888 \
    --NotebookApp.port_retries=0
```
Once the server has started, it will print a message with the initial backend URL used for authentication. You’ll need a copy of this in the next step:
```
To access the notebook, open this file in a browser:
    file:///C:/Users/Spartan/AppData/Roaming/jupyter/runtime/nbserver-20888-open.html
Or copy and paste one of these URLs:
    http://localhost:8888/?token=3db147cc33b2ae3148877cbcb42d964f9d5c8207821a9cf2
or http://127.0.0.1:8888/?token=3db147cc33b2ae3148877cbcb42d964f9d5c8207821a9cf2
```
In Colab, click the “Connect” button and select “Connect to local runtime”. Enter the URL you just copied and click “Connect”:
```
http://localhost:8888/?token=3db147cc33b2ae3148877cbcb42d964f9d5c8207821a9cf2
```