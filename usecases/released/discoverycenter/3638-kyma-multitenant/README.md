# Steps for running Kyma based multitenant application using jenkins pipeline

The [btp-setup-automator](https://github.com/SAP-samples/btp-setup-automator) is an open source project to help developers setting-up their SAP BTP accounts quickly via various command line interfaces.

The script will create a subaccount with the necessary entitlements and deploy the Easy Franchise application in the SAP BTP Kyma Environment. Furthermore a consumer subaccount will be created and the EasyFranchise Application will be subscribed. The following services and subscriptions are created:

- Subaccount kyma-base-mission-automation
- Subaccount kymacustomer
- Authorization and Trust Management Service with broker plan
- Destination Service with lite plan
- SAP HANA Cloud with hana plan
- Kyma Runtime with azure plan

## Pre-Requisites

To use the tooling you first need to finish the following tasks:

- Get a [productive SAP BTP account](https://account.hana.ondemand.com/#/home/welcome) where you can make use of the service plans.
- [Install a Docker engine](https://docs.docker.com/desktop/)

> ⚠ NOTE: Be aware of the terms of Docker for usage in enterprises. For details see this [link](https://www.docker.com/blog/updating-product-subscriptions/).


## Steps


1. Open a VS code on [Node](https://gkerefappscicd.jaas-gcp.cloud.sap.corp/computer/windowskymanode/).

2. Run the following command on terminal to pull the Docker image from the GitHub repository and start a container based upon it.

 ```bash
docker container run --rm -it --name "btp-setup-automator" "ghcr.io/sap-samples/btp-setup-automator:latest"
```
3. You should now see the Docker container up and running,In VS Code open the command palette (Windows: `Ctrl+Shift+P` ; Mac: `Cmd+Shift+P`) and select the `Remote Containers: Attach to Running Container...`

4. Then look for the container by name (`btp-setup-automator`) and selecting it:

5. Now run the main script `btpsa` with the following command in conatiner:

```bash
./btpsa -parameterfile 'https://raw.githubusercontent.com/VanitaDhanagar/btp-setup-automator/main/usecases/released/discoverycenter/3638-kyma-multitenant/parameters.json' -globalaccount '93951304-9109-44bc-ac3f-53c3ac8b309b' -myemail 'sap-cf-e2etests@sap.com' -mypassword 'Initial234!'
```

The btp-setup-automator script will now prepare your SAP BTP account to cover the discovery center mission. You can have a look at the [usecase.json](https://github.com/VanitaDhanagar/btp-setup-automator/blob/main/usecases/released/discoverycenter/3638-kyma-multitenant/usecase.json) and [parameters.json](https://github.com/VanitaDhanagar/btp-setup-automator/blob/main/usecases/released/discoverycenter/3638-kyma-multitenant/parameters.json) for more details about the used services and configuration parameters (e.g. DB Password for SAP HANA Cloud).

6. On successfull creation subaccounts from btp-setup-automator ,download the kubeconfig file from "kyma-base-mission-automation" subaccount and upload it to jenkins crendetial id "kubeconfigkymabase"

7. Run the job :https://gkerefappscicd.jaas-gcp.cloud.sap.corp/job/Kyma_Multitenant_Factory/

> Note: Kindly observe on “Delete Kyma-instances and apps“ stage ,if required login to kyma for authentication with sap-cf-e2etests@sap.com user on [Node](https://gkerefappscicd.jaas-gcp.cloud.sap.corp/computer/windowskymanode/)
