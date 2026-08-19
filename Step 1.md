# Jenkins Installation and Overview Guide

This guide provides an overview of Jenkins, its features, and step-by-step instructions to install Jenkins on a Debian-based system. 
The content is designed to help users understand Jenkins and set it up effectively.

## What is Jenkins?
Jenkins is an open-source automation server written in Java. It facilitates continuous integration and continuous deployment (CI/CD) by automating the building, testing, and deployment of software projects. 
Jenkins is highly extensible, with a vast ecosystem of plugins that allow it to integrate with various tools and services, making it a popular choice for DevOps teams.

### Key Features
- **Continuous Integration/Continuous Deployment (CI/CD)**: Automates the process of integrating code changes, running tests, and deploying applications.
- **Extensive Plugin Ecosystem**: Over 1,800 plugins to integrate with tools like Git, Docker, Kubernetes, AWS, and more.
- **Pipeline Support**: Define build processes as code using Jenkins Pipeline, enabling complex workflows.
- **Distributed Builds**: Supports master-agent architecture to distribute workloads across multiple machines.
- **Customizable**: Highly configurable to suit various project needs, from simple builds to complex deployment pipelines.
- **Community Support**: Backed by a large community, providing extensive documentation and resources.

Prerequisites
Minimum hardware requirements:
256 MB of RAM 1 GB of drive space (although 10 GB is a recommended minimum if running Jenkins as a Docker container)
Recommended hardware configuration for a small team:
4 GB+ of RAM 50 GB+ of drive space

Software requirements:
Java 21 or later: see the Java Requirements page
Web browser: see the Web Browser Compatibility page
For Windows operating system: Windows Support Policy
For Linux operating system: Linux Support Policy
For servlet containers: Servlet Container Support Policy

Installation of Java:
Jenkins requires Java to run, yet not all Linux distributions include Java by default. Additionally, not all Java versions are compatible with Jenkins.
There are multiple Java implementations that you can use. OpenJDK is the most popular one at the moment, we will use it in this guide.
Update the Debian apt repositories, install OpenJDK 21, and check the installation using the following commands:

```bash
sudo apt update
sudo apt install fontconfig openjdk-21-jre
java -version
```

Verify the Java installation to ensure it is correctly installed:

```bash
java -version
```

The output should display the installed Java version, such as:
```
openjdk 21.0.8 2025-07-15
OpenJDK Runtime Environment (build 21.0.8+9-Debian-1)
OpenJDK 64-Bit Server VM (build 21.0.8+9-Debian-1, mixed mode, sharing)
```

### 2. Install Jenkins (Long-Term Support Release)

Jenkins provides a Long-Term Support (LTS) release every 12 weeks, chosen from the stream of regular releases for enhanced stability. Follow these steps to install Jenkins from the Debian-stable apt repository:

```bash
sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2026.key
echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
```

# Update the package index and install Jenkins

```bash
sudo apt-get update
sudo apt-get install jenkins
```

### 3. Start and Enable Jenkins

Once installed, Jenkins runs as a systemd service. Start and enable the Jenkins service to ensure it runs automatically on system boot:

```bash
sudo systemctl start jenkins
sudo systemctl enable jenkins
```

Verify that Jenkins is running:

```bash
sudo systemctl status jenkins
```

The output should indicate that the Jenkins service is `active (running)`.

## Post-Installation Steps

1. **Access Jenkins**:
   - Open a web browser and navigate to `http://localhost:8080` (or `http://<server-ip>:8080` if accessing remotely).
   - Jenkins runs on port 8080 by default. Ensure this port is open in your firewall settings if necessary:
     ```bash
     sudo ufw allow 8080
     ```

2. **Unlock Jenkins**:
   - On first access, Jenkins will prompt you to unlock it using an initial admin password.
   - Retrieve the password from the server:
     ```bash
     sudo cat /var/lib/jenkins/secrets/initialAdminPassword
     ```
   - Copy the password and paste it into the Jenkins web interface to proceed.

3. **Complete Setup**:
   - Follow the on-screen instructions to complete the setup.
   - Choose **Install suggested plugins** for a standard setup, or select specific plugins based on your needs.
   - Create an admin user account and configure the Jenkins URL.

4. **Configure Jenkins**:
   - After setup, access the Jenkins dashboard to create jobs, configure pipelines, or install additional plugins via **Manage Jenkins > Manage Plugins**.

condition: if we stop the EC2 instance and trying to login jenkins then public Ip got change then it will create problem to load the jenkins page. 
so we need to update the below file with latest IP. 
then restart the jenkins application as well.
  
  ```bash
/var/lib/jenkins
jenkins.model.JenkinsLocationConfiguration.xml
  ```
## Basic Usage

-**Create a Job**:
  - From the Jenkins dashboard, click **New Item**, choose a job type (e.g., Freestyle project or Pipeline), and configure the job settings.
  - Link to a version control system (e.g., Git) and define build steps (e.g., shell commands, tests, or deployments).

- **Set Up Pipelines**:
  - Use Jenkins Pipeline to define build processes as code. Create a `Jenkinsfile` in your repository to describe the pipeline stages.

- **Monitor Builds**:
  - View build history, logs, and statuses from the Jenkins dashboard to monitor job execution.

## Troubleshooting

- **Jenkins Not Starting**:
  - Check the Java version (`java -version`) to ensure compatibility.
  - Review Jenkins logs: `sudo journalctl -u jenkins`.

- **Port Conflicts**:
  - If port 8080 is in use, modify the Jenkins configuration file (`/etc/default/jenkins`) to change the `HTTP_PORT`.

- **Plugin Installation Issues**:
  - Ensure internet connectivity and check the plugin manager for errors.

## Additional Notes

- **Upgrading Jenkins**:
  - Regularly update Jenkins using:
    ```bash
    sudo apt-get update
    sudo apt-get install jenkins
    ```

- **Backup**:
  - Back up the `~/.jenkins` directory (or `/var/lib/jenkins` for system-wide installations) to preserve configuration and job data.

- **Security**:
  - Configure authentication and authorization in **Manage Jenkins > Configure Global Security**.
  - Use SSL for secure access by configuring a reverse proxy (e.g., Nginx or Apache).

For more information, refer to the official [Jenkins Documentation](https://www.jenkins.io/doc/).
