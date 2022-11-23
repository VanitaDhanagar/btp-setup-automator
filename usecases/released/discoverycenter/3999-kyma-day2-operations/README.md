# Instructions for running Kyma based multitenant application with day 2 operations

The [btp-setup-automator](https://github.com/SAP-samples/btp-setup-automator) is an open source project to help developers setting-up their SAP BTP accounts quickly via various command line interfaces.
The current script was designed to setup the account and spin up the application which is used by the Discovery Center Mission, [Extend a Kyma based multitenant application with day 2 operations](https://discovery-center.cloud.sap/missiondetail/3999/4206/).

The script will create a subaccount with the necessary entitlements and deploy the Easy Franchise application in the SAP BTP Kyma Environment. Furthermore a consumer subaccount will be created and the EasyFranchise Application will be subscribed. The following services and subscriptions are created:

- Subaccount kyma-day2-operation-automation
- Subaccount day2Customer
- Authorization and Trust Management Service with broker plan
- Destination Service with lite plan
- SAP HANA Cloud with hana plan
- Kyma Runtime with azure plan
- Continuous Integration and Delivery with default plan

## Pre-Requisites

To use the tooling you first need to finish the following tasks:

- Get a [productive SAP BTP account](https://account.hana.ondemand.com/#/home/welcome) where you can make use of the service plans.
- [Install a Docker engine](https://docs.docker.com/desktop/)

> ⚠ NOTE: Be aware of the terms of Docker for usage in enterprises. For details see this [link](https://www.docker.com/blog/updating-product-subscriptions/).

## Changes the configuration

The name of the subaccount is preconfigured to "kyma-day2-operation-automation" and US20 as region.

```bash
./btpsa -parameterfile 'usecases/released/discoverycenter/3999-kyma-day2-operations/parameters.json' -globalaccount '<your global account subdomain as shown in the SAP BTP cockpit>' -myemail '<your email address>' -region 'region for your subaccount'
```

## Instructions


1. **strongly recommended** that you install and setup **MS Visual Studio Code** (VS Code), too:

- [Install VS Code](https://code.visualstudio.com/download) - this will be your development environment.
- Install the VS Code [Dev Containers](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers) extension for connecting to and using Docker containers.

2. Open a VS code on [Node](https://gkerefappscicd.jaas-gcp.cloud.sap.corp/computer/windowskymanode/).

3. Run the following command on terminal to pull the Docker image from the GitHub repository and start a container based upon it.

 ```bash
docker container run --rm -it --name "btp-setup-automator" "ghcr.io/sap-samples/btp-setup-automator:latest"
```
3. you should now see the Docker container up and running.

4. open the command palette (Windows: `Ctrl+Shift+P` ; Mac: `Cmd+Shift+P`) and select the `Remote Containers: Attach to Running Container...`

5. Then look for the container by name (`btp-setup-automator`) and selecting it:


2. Now run the main script `btpsa` with the following command in conatiner:

```bash
./btpsa -parameterfile 'https://raw.githubusercontent.com/VanitaDhanagar/btp-setup-automator/main/usecases/released/discoverycenter/3999-kyma-day2-operations/parameters.json' -globalaccount '93951304-9109-44bc-ac3f-53c3ac8b309b' -myemail 'sap-cf-e2etests@sap.com' -mypassword 'Initial234!'
```

> Note: Before the deployment starts you can choose between two options (Mission-start and Mission-end).Please choose the Mission-end option and further for pop-up Login to kyma with user(sap-cf-e2etests@sap.com) and pass 

The btp-setup-automator script will now prepare your SAP BTP account to cover the discovery center mission. You can have a look at the [usecase.json](https://github.com/VanitaDhanagar/btp-setup-automator/blob/main/usecases/released/discoverycenter/3999-kyma-day2-operations/usecase.json) and [parameters.json](https://github.com/VanitaDhanagar/btp-setup-automator/blob/main/usecases/released/discoverycenter/3999-kyma-day2-operations/parameters.json) for more details about the used services and configuration parameters (e.g. DB Password for SAP HANA Cloud).

3. On successfull creation subaccounts from btp-setup-automator ,download the kubeconfig file from "kyma-day2-operation-automation" subaccount and upload it to jenkins crendetial id "kubeconfigday2"

4. Run the job :https://gkerefappscicd.jaas-gcp.cloud.sap.corp/job/Kyma_Day2Operations_Factory

5. Kindly observe on “Delete Kyma-instances and apps“ stage where you have to login to kyma authentication with sap-cf-e2etests@sap.com user on [Node](https://gkerefappscicd.jaas-gcp.cloud.sap.corp/computer/windowskymanode/)
