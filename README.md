# skillbadge4

# Task 1: Create the production environment

First of all, navigate to *Deployment Manager* in the Console to check the deployment status of **kraken-jumphost**.

![screen](https://github.com/ashwinraiyani/skillbadge4/blob/main/40.png)

Click on **kraken-jumphost**,  on left side you will see VM instance and near by SSH button click on it.

On SSH Screen type *cd /work/dm* 

Type *vi prod-network.yaml*, and replace SET_REGION to us-east1. (Press Escape, type *:wq* press enter)

Type *gcloud deployment-manager deployments create prod-network --config prod-network.yaml*

Type *gcloud config set compute/zone us-east1-b*

Type *gcloud container clusters create kraken-prod --num-nodes 2 --network kraken-prod-vpc --subnetwork kraken-prod-subnet*

Type *gcloud container clusters get-credentials kraken-prod*

Type *cd /work/k8s*

Type for F in $(ls *.yaml); do kubectl create -f $F; done

Type *kubectl get services*


Wait for 2 minutes  **Task Completed check the Progress** 

# Task 2: Setup the Admin instance

On SSH Screen Type *gcloud config set compute/zone us-east1-b*  

Type *gcloud compute instances create kraken-admin --network-interface="subnet=kraken-mgmt-subnet" --network-interface="subnet=kraken-prod-subnet"*


Click on **Navigation menu > Monitoring.**

Click Alerting from the left menu, then click Create Policy.

Click Add Condition, and then set up the Metrics with the following parameters:

**Fields	Options**
Resource Type: 	GCE VM Instance

Metric:	CPU Utilization 

Filter:	Choose **instance name** and select **kraken-admin**

Scrolldown -- Enter 0.5 in Thresold for 1 minute

Click ADD, Click Next, In Notification channel select **Manage Notificaiton channels**

 May new browser will open up, Find Email - Click on *Add New*
 
 Copy username from Qwiklabs and enter into email id place
 
 Copy ProjectID from Qwiklabs and enter into display name
 
 Click Save
 
Give Alert Name if ask 

Click Save.



**Task Completed check the Progress**
# Task 3: Verify the Spinnaker deployment

use cloudshell (click on right corner ">_" icon)


Type *gcloud config set compute/zone us-east1-b*

Type *gcloud container clusters get-credentials spinnaker-tutorial*

Type *DECK_POD=$(kubectl get pods --namespace default -l "cluster=spin-deck" -o jsonpath="{.items[0].metadata.name}")*

Type *kubectl port-forward --namespace default $DECK_POD 8080:9000 >> /dev/null &*


Click the Web Preview icon at the top of the Cloud Shell window and select Preview on port 8080, to open the Spinnaker user interface.

![screen](https://github.com/ashwinraiyani/skillbadge4/blob/main/41.jpg)

Click on applications-> Click on sample 

Now perform 3 steps as per below screen

![screen](https://github.com/ashwinraiyani/skillbadge4/blob/main/42.png)

Now comeback to cloudshell

Type *gcloud config set compute/zone us-east1-b*

Type *gcloud source repos clone sample-app*

Type *cd sample-app*

Type *touch a*

Type *git config --global user.email "$(gcloud config get-value account)"*

Type *git config --global user.name "Student"*

Type *git commit -a -m "change"*

Type *git tag v1.0.1*

Type *git push --tags*


**Wait for few minutes to get reflect in marks**

**Task Completed check the Progress**

