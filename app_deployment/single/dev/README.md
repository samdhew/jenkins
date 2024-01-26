# Jenkins DSL Groovy Scripts for ABC Web Application

## Overview

This Jenkins DSL Groovy script automates the deployment process of the ABC Web Application. It covers stages such as release information, package download, copying to the server, deployment, and server health checks.

## Functions

### `displayInfo(title)`

Displays information with a timestamp. Used to print release details, package information, and status.

### `downloadPackage()`

Performs the download of the ABC Web Application package. Checks for a new release, removes previous packages, and downloads the package from an S3 bucket.

### `copyToServer(server)`

Copies the downloaded package to the specified server using SSH. Extracts the package on the server and lists the contents.

### `deployOnServer(server)`

Deploys the ABC Web Application on the specified server. Stops Tomcat, updates the current deployment, and starts Tomcat again.

### `healthCheck(server)`

Performs a health check on the deployed application by monitoring the HTTP response status code. Waits for a successful response (status code 200) before proceeding.

## Pipeline Definition

The Jenkins pipeline is defined with the following stages:

1. **Release Info:**
   - Displays information about the release, package, and related details.

2. **Package Download:**
   - Downloads the ABC Web Application package from an S3 bucket if it's a new release.

3. **Package Copy:**
   - Copies the downloaded package to the specified server using SSH.

4. **Package Deploy:**
   - Deploys the ABC Web Application on the specified server after user confirmation.

5. **Server HealthCheck:**
   - Performs a health check on the deployed application to ensure it is running successfully.

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
