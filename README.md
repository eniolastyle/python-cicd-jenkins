#Jenkins CI/CD Running Python Script

This README file explains two methods to run Jenkins with Docker and enable Python 3 support. You can either install Python 3 while building a custom Jenkins Docker image using a Dockerfile or access an existing Jenkins container using SSH as the root user and install Python 3 manually.

### Method 1: Building a Custom Jenkins Docker Image

1. Clone this repository:
   ```bash
   git clone <repository_url>
   cd <repository_directory>
   ```

2. Create a Dockerfile:
   Create a new file named `Dockerfile` in the repository directory and paste the following content:

   ```Dockerfile
   FROM jenkins/jenkins:lts

   # Switch to the root user to install packages
   USER root

   # Update package repositories and install Python 3
   RUN apt-get update && apt-get install -y python3 python3-pip

   # Switch back to the Jenkins user
   USER jenkins
   ```

3. Build the custom Jenkins image:
   Execute the following command to build the custom Jenkins image with Python 3:

   ```bash
   docker build -t custom_jenkins .
   ```

4. Run the custom Jenkins container:
   Run the custom Jenkins container with the following command:

   ```bash
   docker run -d -p 8080:8080 -p 50000:50000 --name jenkins_with_python custom_jenkins
   ```

5. Access Jenkins:
   Access Jenkins by opening http://localhost:8080 in your web browser. Use the initial admin password to complete the setup.

6. Verify Python 3 installation:
   Create a new Jenkins pipeline job with a script that uses Python 3. Confirm that Python 3 is now available in your Jenkins pipelines.

### Method 2: Installing Python 3 on an Existing Jenkins Container

1. Find the container ID or name:
   Run the following command to list all running containers and find the Jenkins container's ID or name:

   ```bash
   docker ps
   ```

   Look for the container running the Jenkins image (e.g., `jenkins/jenkins:lts`).

2. Access the Jenkins container as the root user:
   Use the following command to access the Jenkins container as the root user:

   ```bash
   docker exec -u 0 -it <container_name_or_id> sh
   ```

   Replace `<container_name_or_id>` with the actual name or ID of the Jenkins container.

3. Install Python 3 and pip:
   Once inside the container as the root user, run the following commands to install Python 3 and pip:

   ```bash
   apt-get update
   apt-get install -y python3 python3-pip
   ```

4. Verify Python 3 installation:
   Exit the container and verify that Python 3 is now available in your Jenkins pipelines.

5. (Optional) Save changes to the container:
   If you want to save the changes made to the container, create a new Docker image using the following command:

   ```bash
   docker commit <container_name_or_id> jenkins_with_python
   ```

   You can then use this image in the future to run Jenkins with Python 3 support.

Please note that running Jenkins with the ability to execute tasks as the root user poses security risks. Use the second method with caution and ensure that only necessary administrative tasks are performed with root access. Method 1 is recommended for a more controlled and repeatable setup.
