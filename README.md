# A SIMPLE WEB SERVICE USING PYTHON

# Flask Application Docker Setup

## This README provides a guide on how to set up a Flask application in a Docker container. It covers the initial Python setup, building a Docker image, running a Docker container, and troubleshooting common issues.

## Prerequisites
Docker installed on your machine. Visit Docker's official website for installation instructions.
Basic knowledge of Python and Flask framework.
Your Flask application files: app.py and requirements.txt.

# Step 1: Python Application Setup
Ensure you have a Flask application ready. Your project should at least include:

app.py: The main Python file containing your Flask application.
requirements.txt: A file listing all the necessary Python packages.

# Step 2: Docker Setup
Building the Docker Image
Create a Dockerfile in your project directory with the following content:

## Build your Docker image by running 
docker build -t newflask .

## Running the Docker Container
docker run -p 5000:5000 -d --name myflaskapp newflask

## Common Docker Commands
#### To list all containers (including inactive ones):
docker ps -a
#### To view logs of a container:
docker logs <container_id_or_name>


# Troubleshooting
If you encounter any issues with your Docker container, first check the logs using docker logs <container_id_or_name>. This often provides insights into what might be going wrong.

Ensure your Dockerfile is correctly set up and that all necessary files are included in your Docker image. If changes are made to the Dockerfile or your Flask application, remember to rebuild the Docker image and rerun the container.

# Step 3: Provisioning AWS ECR Repository with Terraform

To store your Docker images in an AWS ECR repository, you can use Terraform to provision the repository. This section assumes you have Terraform installed. If not, please refer to the Terraform documentation for installation instructions.

## Files
provider.tf: Specifies the Terraform provider configuration for AWS. You need to create this file with your AWS provider settings.
ecr.tf: Contains the Terraform configuration to provision an AWS ECR repository.
outputs.tf: Defines output variables that Terraform will display upon successful infrastructure provisioning.

## Setup Instructions
#### Initialize Terraform:Navigate to your Terraform configuration directory and run the following command to initialize Terraform. This step will download the necessary provider plugins.
terraform init

#### Run Terraform Plan: Execute the following command to see the planned infrastructure changes. This command does not apply the changes; it only shows what actions Terraform will perform.

terraform plan

#### Apply Terraform Configuration: To create the ECR repository, run:
terraform apply

# Step 4: Push the Image to the ECR repository

## Pushing the Image
### Tag Your Docker Image

1. First, tag your Docker image with the ECR repository URL. Replace <IMAGE NAME>, <your-account-id>, <your-region>, and <your-repository-name> with your specific details.

docker tag <IMAGE NAME>:latest <your-account-id>.dkr.ecr.<your-region>.amazonaws.com/<your-repository-name>:latest
This command tags your local Docker image to match the repository name in AWS ECR.

2. Authenticate Docker to Your ECR Repository

Before pushing the image, you need to authenticate your Docker client to your Amazon ECR registry. Use the AWS CLI to get the login password and pipe it to the Docker login command.

aws ecr get-login-password --region <your-region> | docker login --username AWS --password-stdin <your-account-id>.dkr.ecr.<your-region>.amazonaws.com
This command retrieves the authentication token using the AWS CLI, then uses it to authenticate your Docker client to the ECR repository.

3. Push the Image to ECR

Once authenticated, you can push your Docker image to your AWS ECR repository using the following command:

docker push <your-account-id>.dkr.ecr.<your-region>.amazonaws.com/<your-repository-name>:latest
This command uploads your tagged Docker image to the specified ECR repository.
