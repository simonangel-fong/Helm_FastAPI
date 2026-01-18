# Helm_FastAPI

A heml project of fastapi

## FastAPI App

```sh
cd fastapi

# create venv
python -m venv .venv

# activate venv
.venv\Scripts\activate.bat

# upgrade pip
python -m pip install --upgrade pip

# install fast
pip install "fastapi[standard]"

# test local
fastapi dev app/main.py

curl http://127.0.0.1:8000/
# {"message":"Hello World"}
```

---

## Docker Image

```sh
cd fastapi

# output the dependencies
pip freeze > ./requirements.txt

docker build -t helm-fastapi .

docker rm -f helm-fastapi
docker run -d --name helm-fastapi -p 8080:8080 helm-fastapi

# push to dockerhub
docker tag helm-fastapi simonangelfong/helm-fastapi-hello-world

docker push simonangelfong/helm-fastapi-hello-world
```

---

## Create Helm Chart

```sh
helm create helm
# Creating helm

vi helm/values.yaml
# update
# image.repository = simonangelfong/helm-fastapi-hello-world
# service.type = NodePort
# service.port = 8080

vi helm/template/deployment.yaml
# remove all probe

# pack helm chart
helm package helm-chart

helm repo index .


# push to github repo
git add .
git commit -m "add helm"

```

---

## Deploy with Helm Chart

```sh
helm repo add helm-fastapi https://simonangel-fong.github.io/Helm_FastAPI/
# "helm-fastapi" has been added to your repositories
helm repo update
# Hang tight while we grab the latest from your chart repositories...
# ...Successfully got an update from the "helm-fastapi" chart repository
# Update Complete. ⎈Happy Helming!⎈

helm search repo helm-fastapi
# NAME                    CHART VERSION   APP VERSION     DESCRIPTION
# helm-fastapi/helm       0.1.0                           A Helm chart for Kubernetes

helm install my-api helm-fastapi/helm
# NAME: my-api
# LAST DEPLOYED: Sun Jan 18 17:01:29 2026
# NAMESPACE: default
# STATUS: deployed
# REVISION: 1
# NOTES:
# 1. Get the application URL by running these commands:
#   export NODE_PORT=$(kubectl get --namespace default -o jsonpath="{.spec.ports[0].nodePort}" services my-api-helm)
#   export NODE_IP=$(kubectl get nodes --namespace default -o jsonpath="{.items[0].status.addresses[0].address}")
#   echo http://$NODE_IP:$NODE_PORT

export NODE_PORT=$(kubectl get --namespace default -o jsonpath="{.spec.ports[0].nodePort}" services my-api-helm)
export NODE_IP=$(kubectl get nodes --namespace default -o jsonpath="{.items[0].status.addresses[0].address}")

# confirm
curl http://$NODE_IP:$NODE_PORT
# {"message":"Hello World"}
```
