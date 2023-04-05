### Section Docker

Pour build l'image, la push sur DockerHub, il faut réaliser les commandes ci-dessous (en partant du principe que l'on est dans l'emplacement du fichier Dockerfile): 

```bash
docker build -t hanryos/ex05:latest .
docker login
docker push hanryos/ex05:latest
```

Si l'on veut la tester, on peut le faire via la commande suivante: 

```bash
docker run -d -p 80:3000 -e "STORY_FOLDER=story" hanryos/ex05:latest
```

---

### Section K8s

Pour pouvoir lancer notre image sur Kubernetes, il nous faut avoir un cluster. Dans notre cas, nous utiliserons minikube:

```bash
minikube start --driver docker
```

Puis il va nous falloir appliquer nos fichiers de ressources à notre cluster pour changer son état et permettre:

* La création dans les nodes de nos pods faisant tourner les conteneurs avec notre image
* La création d'un service de type **LoadBalancer** permettant d'atteindre l'une des 5 replicas en cas de crash / restart de l'un de nos pods.

```bash
kubectl apply -f .\ressources-files\deployment.yml,.\ressources-files\service.yml
minikube service ex05-deployment
```