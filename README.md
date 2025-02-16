##Installation of ISTIO: Using the Minikube
#Pre-requisites Minikube installed and configured.

1. download the istio tar file from the following link:

curl https://github.com/istio/istio/releases/download/1.17.1/istio-1.17.1-linux-arm64.tar.gz

or wget https://github.com/istio/istio/releases/download/1.17.1/istio-1.17.1-linux-arm64.tar.gz

download the Tar file and extract it tar -xvzf istio-1.17.1-linux-arm64.tar.gz [or if you are using ui nevigate to the folder and double click to extra>


2. istioctl path determination:

 - istioctl would be located in /bin folder in the downloaded folder.

 - a environmental variable for that file has to be created.

 - go to the /bin folder and check the pwd and note it down.

 - create env ie: export PATH=$PATH:(pwd-content)/bin.

 - check whether istio is accessable or not user istioctl command to get the details.

[Note!-The Path environment created is specfic for that terminal session only if the session closes the path would be lost.
it should be added to the .bashrc file to make it permant]


3. Executing it in minikube cluster:

 - minikube start --cpus 3 --memory 8192 [Note!- your hardware limits and execute accordingly]

 - kubectl get ns [get namespaces]

 - istioctl install

   - Would install all the required services for istio under namespace istio-system

   - kubectl get all -n istio-system

   - kubectl get pods -n istio-system [two pods mainly istiod & istio-ingress-gateway would be up and running]


4. Default namespace labelling to trigger istio envoy injection:

  - By default all the services would be deployed in the default namespaces if we do not specify any specific namespaces.

  - istio would be triggered if the namespaces labeled as istio-injection=enabled, then any services / resources created would

    trigger istio automatically and a side-car container would be created along with the main services.

  - kubectl get ns default --show-labels [to see the labels of default namespace]

  - kubectl label namespace default istio-injection=enabled

  - check the label kubectl get ns default --show-labels


5. Deploying Micro-Services to the cluster to be monitored:

  - Deploy Micro-services which would be monitored using istio. for this example use google microservices.

  - giturl: https://github.com/AbhiGT997/microservices-demo.git

  - cd to release folder and deploy the manifest file there.

    - kubectl apply -f kubernetes-manifests.yaml

    - all the services would be deployed automatically.

    - kubectl get all -o wide.

  - Hence two pods would be created for every microservices, ie istio would inject a envoy side-car container to the pods.
  
  
 6. Data vizulization in Istio using addons:

  - For data vizulizations istio supports addons for vizulizations tools like prometheus, grafana, kiali, jaeger etc.

  - navigate to downloaded istio folder ie: istio-1.17.1

  - there would be samples/addons folder containing all the required manifest file for the data vizulization tools mentioned.

  - deploy by using:

    kubectl apply -f istio-1.17.1/samples/addons [would deploy all the services accordingly]

  - kubectl get svc -n istio-system [would list out all the services deployed]


7. Accessing the services using port-forwarding:

  - kubectl port-forward svc/<service-name> -n istio-system <specified port> [Note!- specified port would be specific for that services]

  - for eg: kubectl port-forward svc/grafana -n istio-system 3000

  - then a localport address would pop up using that address the services can be accessed.
