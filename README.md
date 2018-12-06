# JamfPro Kubernetes Manifests
JamfPro Kubernetes manifest examples

## Description
A set of Kubernetes manifests for creating a clustered JamfPro instance.  Includes the clustered Tomcat statefulset, a service, and an ingress.  Also includes a MySQL statefulset to get a test instance up and running quickly.

## Pre-Requisites
* A Kubernetes cluster
* Kubernetes storage classes for data persistence and log file persistence ( mysql-storage and log-storage )
* JamfPro manual install ROOT.war, unzipped and made into a data container image ( see Dockerfile as example )
* Ingress controller supporting cookie affinity annotations ( e.g. [HAProxy](https://github.com/jcmoraisjr/haproxy-ingress#affinity) )

## Other Notes
The log and mysql storage classes included in this repo are setup for local minikube usage and should be able to be utilized when testing locally.

Within the Tomcat manifest, there is hardcoded mysql server up check before proceeding with starting the JamfPro instance.  If using a different Kubernetes service or an external database persistence this will need to be modified/removed accordingly.

When upgrading a clustered JamfPro instance, the master node MUST be upgraded first to process any database migrations before upgrading the non-master node(s).  This can be accomplished by changing the statefulset update strategy to `OnDelete` 

-- OR -- 

Modify the `MASTER_NODE_NAME` to be the highest ordinal of the statefulset to allow a rolling update to function as expected.

We do not currently publicly publish JamfPro releases to be utilized as data images, you must download a manual ROOT.war from [JamfNation](https://www.jamf.com/jamf-nation/) to create the required data container image.


The Tomcat image found in the tomcat.yaml manifest *may* not be the most current, please follow releases in our other Github repo [JamfPro](https://github.com/jamf/jamfpro) to ensure your image is the most current.

## Usage
If running a local minikube startup should be as simple as creating a data image container availabe to your minikube instance and running from within this directory:
```
kubectl create -f .
```

## Data Image Creation Example
```
unzip ROOT.war
docker build -t jamfpro-build .
```
