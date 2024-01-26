# Jenkins DSL Groovy Scripts for ABC Web Application Deployment

## Overview

This Jenkins DSL Groovy script automates the deployment process of the ABC Web Application across different environments, including development (DEV), certification (CERT), and staging (STG) environments. The script performs actions such as package download, copying to servers, deployment, and health checks.

## Functions

### `displayInfo(title)`

Displays information with a timestamp, including release details, package information, and deployment status.

### `downloadPackage()`

Downloads the ABC Web Application package from an S3 bucket, removing previous packages if they exist.

### `copyToServer(server)`

Copies the downloaded package to the specified server using SSH. It also extracts the package on the server and lists the contents.

### `deployOnServer(server)`

Deploys the ABC Web Application on the specified server, stopping Tomcat, updating the current deployment, and starting Tomcat again.

### `healthCheck(server)`

Performs a health check on the deployed application by monitoring the HTTP response status code. It waits for a successful response (status code 200) before proceeding.

## Pipeline Definition

The Jenkins pipeline consists of the following stages:

1. **Release Info:**
   - Displays information about the release, package, and related details.

2. **Package Download:**
   - Downloads the ABC Web Application package from an S3 bucket if it's a new release.

3. **DEV Copy:**
   - Copies the downloaded package to the development (DEV) server.

4. **DEV Deploy:**
   - Deploys the ABC Web Application on the DEV server after user confirmation.

5. **DEV HealthCheck:**
   - Performs a health check on the DEV server.

6. **CERT Copy:**
   - Copies the package to the certification (CERT) server if the DEV deployment is successful.

7. **CERT Deploy:**
   - Deploys the ABC Web Application on the CERT server after user confirmation.

8. **CERT HealthCheck:**
   - Performs a health check on the CERT server.

9. **STG Node-1 Copy:**
   - Copies the package to the first staging (STG) server if the CERT deployment is successful.

10. **STG Node-1 Deploy:**
    - Deploys the ABC Web Application on the first STG server after user confirmation.

11. **STG Node-1 HealthCheck:**
    - Performs a health check on the first STG server.

12. **STG Node-2 Copy:**
    - Copies the package to the second staging (STG) server if the first STG deployment is successful.

13. **STG Node-2 Deploy:**
    - Deploys the ABC Web Application on the second STG server after user confirmation.

14. **STG Node-2 HealthCheck:**
    - Performs a health check on the second STG server.

<!--
15. **PRD Job Initiate:**
    - Initiates the PRD deployment job if all previous deployments are successful (commented out).
-->

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
