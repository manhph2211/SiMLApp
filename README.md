Text to speech Service
====

This repo aim to push a simple TTS model into production using torchserve and kubernetes. I also add Jenkine file for a simple CI/CD pipeline. I always aim to build clear template for you to even make better product :smile: Check out my previous [template](https://github.com/manhph2211/Image-Generation-App) in which I play with Jax, FastAPI, Streamlit and Docker-Compose if you're interested. 

![image](https://github.com/manhph2211/TTSSVC/assets/61444616/1cfdec96-fbba-4626-9751-c982163f3118)

Okay, get started with cloning this repo: 
```git clone https://github.com/manhph2211/TTSSVC && cd TTSSVC```

# Backend Service 

## Activate Environment

```
cd backend
conda create -n tts_app python=3.8
conda activate tts_app
```

## Install Dependencies

```
pip install torch==2.0.0+cu118 -f https://download.pytorch.org/whl/torch_stable.html
pip install -r requirements.txt
```

## Serve the WaveGlow speech synthesis model on TorchServe

* Generate the model archive for waveglow speech synthesis model:

   ```
   bash ./create_mar.sh
   ```

* Run torchserve API:
   ```
   torchserve --start --model-store model_store --models waveglow_synthesizer.mar --ts-config config.properties
   ```

* Check API and see the audio output:

   ```
   python client.py
   ```

## Deploy on Kubernetes

If you want to further deploy your app on Kubernetes Clusters, you can first install the Kubernetes command-line tool, kubectl, allows you to run commands against Kubernetes clusters.

Next, adjust the config files (kubeconfig.yaml, deployment.yaml, service.yaml) to yours

Then you need to build and push your image into dockerhub:

```
docker login
docker build -t app . 
docker tag app user/app 
docker push user/app
```


```
export KUBECONFIG=kubeconfig.yaml 
kubectl apply -f deployment.yaml 
kubectl apply -f service.yaml 
```

# MiddleWare Service

For this service, you need to install mongoDB and create your [mongoDB Atlas account](https://www.mongodb.com/atlas/database), adjust file `middleware/src/database/index.js` with your mongoDB atlas username and password. 

This uses expressjs to create TTS API gateway, authen API for login and for TTS audio dashboard API that connected the database. Simply run:
```
cd middleware
npm i  && npm start
```

# Frontend Service

I used React for developing my TTS demo curently supporting login, dashboard, tts ... :smiley:

Anyway, run this for the app: 

```
cd frontend
npm i  && npm start
```

