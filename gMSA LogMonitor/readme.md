# gMSA Log Monitor sample

This is a sample application that uses gMSA and has the LogMonitor tool enabled for troubleshooting. Here's a detailed view on the files:

## LogMonitor.exe

This file comes from the [official repo](https://github.com/microsoft/windows-container-tools/tree/master/LogMonitor). I downloaded it in case you want to use it for building your image. See that in this sample, I still download the official binaries from GitHub - this is for learning purposes.

## LogMonitorConfig.json

This files is used to specify what Log Monitor should monitor. It specifies the sources for regular IIS logs.

## SampleApp.yaml

This is the file useed to deploy your app into Kubernetes. There's one change needed for your environment. You need to change the [image](https://github.com/vrapolinario/LogMonitorSamples/blob/9e4f3b48fa5c4f9d87786c772db2436300d3acd0/gMSA%20LogMonitor/SampleApp.yaml#L24) to your final image registry.

## default.aspx

This is the file used on the container iamge as the website that shows up as the sample application. It's a simple file with no code structure at all. It simply checks the variables for the authenticated user and shows it on your browser.

## dockerfile

Docker file used to build the image locally. You can use the command "docker build -t gmsalmsample:v1 ." to build it. After that, you should be able to push the image to a registry that your Kubernetes cluster has access to.

# run.ps1

I created this file so the IIS configuration could be implemented. I was not able to run them directly from the dockerfile. This also serves as a leraning for running PowerShell scripts from your dockerfile now. :)

# The process:

1. Pull all files on this repo to a local folder on your machine.
2. From an elevated PowerShell session, change the context of the session to the folder on which the files reside, then run the docker build command docker: "build -t gmsalmsample:v1 ."
3. Push the new image to the same registry your Kubernetes cluster has access to. More detilas on how  to [login and push image](https://docs.microsoft.com/en-us/azure/container-registry/container-registry-get-started-powershell#log-in-to-registry) and how to [give access to AKS clusters on ACR](https://docs.microsoft.com/en-us/azure/aks/cluster-container-registry-integration?tabs=azure-cli).
4. From kubectl, deploy the SampleApp.yaml to your cluster using: "kubectl apply -f SampleApp.yaml"
5. Check the IP address of the Load Balancer created: "kubectl get service"
6. Open a browser and type the IP address to open the app: "http://<IP address>/app"
7. Open the logs on your AKS cluster to see the logs from the Log Monitor tool, now integrated to your AKS cluster (this doesn't happen by default).
