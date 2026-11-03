# Lab 3 — Running hello-app on Kubernetes with Terraform

**Name:** ARSHADULLA SAIT  
**Roll number:** 261100680022

## Part 1: First terraform plan

Created a Kubernetes Deployment and LoadBalancer Service using Terraform.

Plan: 2 to add, 0 to change, 0 to destroy.

## Part 1: Terraform output URL

http://localhost:8080

## Part 2: Image update from v1 to v2

Changed the container image from shripada/hello-app:v1 to shripada/hello-app:v2.

Terraform detected an in-place Deployment update.

Plan: 0 to add, 1 to change, 0 to destroy.

The apply completed successfully, and three replicas were running with image v2.

## Part 3: Detecting manual changes

Manually scaled the Deployment from three replicas to five.

Terraform detected the difference and planned to restore the configured replica count.

Plan: 0 to add, 1 to change, 0 to destroy.

After applying, the Deployment returned to three replicas.

I also deleted the Service manually. Terraform detected the missing Service and planned to recreate it.

Plan: 1 to add, 0 to change, 0 to destroy.

The Service was successfully restored.

## Part 4: Reusable module with DEV and PROD

Created the reusable module in modules/hello-app.

DEV:
- Namespace: dev
- Service port: 8080
- Replicas: 3

PROD:
- Namespace: prod
- Service port: 8090
- Replicas: 3

Both environments were successfully created using Terraform. The kind cluster showed the LoadBalancer external IP as pending.

## Break it: Deleted Terraform state

Backed up the DEV Terraform state and deleted terraform.tfstate.

Terraform attempted to create the existing namespace and returned this error:

Error creating namespace: namespaces "dev" already exists

Deleted the DEV namespace and applied Terraform again.

Recovery result:

Apply complete! Resources: 3 added, 0 changed, 0 destroyed.

The PROD environment was not deleted.

## Break it: Wrong Service selector

Changed the Service selector from hello-app to helo-app.

The EndpointSlice initially showed no endpoints. After inspecting it, I identified the incorrect selector and corrected it to hello-app.

The final EndpointSlice showed three Pod IP addresses, confirming that the Service was working correctly again.

## Screenshot

Save the browser screenshot showing the DEV and PROD hello-app pages as screenshot.png in the repository root.
