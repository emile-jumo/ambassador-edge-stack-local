# The Ambassador Edge Stack Local Deployment
## Prerequisites 
1. minikube: `brew install minikube`
2. helm: 
	```
	curl https://raw.githubusercontent.com/kubernetes/helm/master/scripts/get-helm-3 > get_helm.sh
	chmod 700 get_helm.sh
	./get_helm.sh
	```
3. `edgectl`:
	```
	curl -fLO https://metriton.datawire.io/downloads/darwin/edgectl
	chmod a+x edgectl
	mv edgectl /usr/local/bin
	```

## How to run the ambassador stack locally
1. `minikube start --vm-driver=hyperkit`
2. From the [official guide](https://www.getambassador.io/user-guide/getting-started/#install-the-ambassador-edge-stack) from Ambassador:
	```
	kubectl apply -f https://www.getambassador.io/yaml/aes-crds.yaml && \
	kubectl wait --for condition=established --timeout=90s crd -lproduct=aes && \
	kubectl apply -f https://www.getambassador.io/yaml/aes.yaml && \
	kubectl -n ambassador wait --for condition=available --timeout=90s deploy -lproduct=aes
	```
3. Use `minikube tunnel` to expose the load balancer.
4. Get your cluster IP: `kubectl get -n ambassador service ambassador -o json | jq '.spec.clusterIP'`
5. Go to `http://<yourClusterIp>` to view the dashboard. Note that the certificate is not loaded in the browser and therefore the browser will complain that the site is unsafe to visit.
6. Create a service: `kubectl apply -f services/hello-world.yaml`
7. Create a mapping: `kubectl apply -f mappings/hello-world-backend.yaml`
8. `curl -lK https://<yourClusterIp>/helloworld/`

## Useful links
[kube-ps1](https://github.com/jonmosco/kube-ps1) is very useful to have the current Kubernetes context and namespace configured in your terminal.