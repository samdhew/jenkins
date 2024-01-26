# Jenkins DSL Groovy Scripts for ABC Web Application Deployment

## Overview

This Jenkins DSL Groovy script facilitates the automated deployment of the ABC Web Application to multiple production (PRD) nodes. The script orchestrates tasks such as package download, copying to servers, deployment, and health checks. It is designed to work seamlessly with Jenkins for streamlined continuous deployment.

## Functions

### `displayInfo(title)`

Displays information with a timestamp, including release details, package information, and deployment status.

### `downloadPackage()`

Downloads the ABC Web Application package from an S3 bucket, removing previous packages if they exist.

### `copyToServer(server)`

Copies the downloaded package to the specified production (PRD) server using SSH. It also extracts the package on the server and lists the contents.

### `deployOnServer(server)`

Deploys the ABC Web Application on the specified PRD server, stopping Tomcat, updating the current deployment, and starting Tomcat again.

### `healthCheck(server)`

Performs a health check on the deployed application by monitoring the HTTP response status code. It waits for a successful response (status code 200) before proceeding.

## Pipeline Definition

The Jenkins pipeline comprises the following stages:

1. **Release Info:**
   - Displays information about the release, package, and related details.

2. **Package Download:**
   - Downloads the ABC Web Application package from an S3 bucket if it's a new release.

3. **PRD Node-1 Copy:**
   - Copies the package to the first production (PRD) node.

4. **PRD Node-1 Deploy:**
   - Deploys the ABC Web Application on the first PRD node after user confirmation.

5. **PRD Node-1 HealthCheck:**
   - Performs a health check on the first PRD node.

6. **PRD Node-2 Copy:**
   - Copies the package to the second PRD node if the first PRD deployment is successful.

7. **PRD Node-2 Deploy:**
   - Deploys the ABC Web Application on the second PRD node after user confirmation.

8. **PRD Node-2 HealthCheck:**
   - Performs a health check on the second PRD node.

## Parameters

- **release_version:**
  - Type: Validating String
  - Default Value: ''
  - Regex: /^[0-9]+.[0-9]+.[0-9]+$/
  - Description: Example - 9.4.1

- **new_release:**
  - Type: Boolean
  - Default Value: true
  - Description: If true, the package will be downloaded from the S3 bucket. If false, the package should reside on the cluster. Default is 'True'.

## Usage

1. Configure Jenkins with the necessary plugins and credentials.
2. Create a new Jenkins pipeline job and link it to the Git repository containing these DSL scripts.
3. Configure job parameters as needed, specifying the release version and whether it's a new release.
4. Run the pipeline, monitoring the progress on the Jenkins dashboard.
