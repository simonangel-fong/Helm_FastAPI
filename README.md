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
helm repo add helm-fastapi https://github.com/simonangel-fong/Helm_FastAPI
helm repo update
```