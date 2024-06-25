### Prerequisites for Creating CI/CD Pipelines

**Go to Jenkins Dashboard:**

1. **Click on Manage Jenkins.**
2. **Click on Plugins.**
3. **Click on Available Plugins.**

**Plugins to Install:**

1. **jdk:**
   - **Eclipse Temurin Installer:** Automatically installs and manages the Eclipse Temurin JDK, ensuring the correct Java environment for builds.

2. **Maven:**
   - **Config File Provider:** Manages and provides configuration files to jobs, maintaining consistent configurations across multiple jobs.
   - **Pipeline Maven Integration:** Integrates Maven build steps into Jenkins pipelines, allowing for building, testing, and deploying Maven projects.
   - **Maven Integration:** Provides Maven functionalities within Jenkins, such as project setup, build automation, and dependency management.

3. **SonarQube:**
   - **SonarQube Scanner:** Integrates SonarQube code analysis into Jenkins pipelines, enabling automatic static code analysis to ensure code quality.

4. **Docker:**
   - **Docker:** Allows Jenkins to interact with Docker, enabling building, running, and managing Docker containers from Jenkins jobs.
   - **Docker Pipeline:** Provides Docker functionalities within Jenkins pipelines, allowing Docker commands and steps within pipeline scripts.

5. **Kubernetes:**
   - **Kubernetes:** Integrates Jenkins with Kubernetes, dynamically provisioning and managing Kubernetes pods as Jenkins agents.
   - **Kubernetes Client API:** Provides necessary API functionalities to interact with Kubernetes clusters.
   - **Kubernetes CLI:** Allows execution of Kubernetes CLI commands within Jenkins jobs.
   - **Kubernetes Credentials:** Manages Kubernetes credentials securely, allowing Jenkins to authenticate and interact with Kubernetes clusters.

**Click on Install:**
- After selecting the plugins, click on **Install** to download and install them into your Jenkins instance.

Once directed to the Download Progress page, monitor the progress of each plugin download. Do not restart Jenkins unless there are errors in the downloading process.

### Configure Installed Plugins

1. **Go to Manage Jenkins.**
2. **Under System Configuration, click on Tools.**
3. **Scroll down to each installed plugin and configure them.**

**JDK Installation:**

1. Click on **Add JDK**.
2. Name it **jdk17**.
3. Tick the box **Install automatically**.
4. Click on **Add Installer**.
5. Click on **Install from adoptium.net**.
6. Choose the **jdk version** (`jdk-17.0.9+9`).

**SonarQube Scanner Installation:**

1. Click on **Add SonarQube Scanner**.
2. Name it **sonar-scanner**.
3. In **Install from Maven Central**, choose the version (`SonarQube Scanner 5.0.1.3006`).

**Maven Installation:**

1. Click on **Add Maven**.
2. Name it **maven3**.
3. In **Install from Apache**, choose the version (`3.6.1`).

**Docker Installation:**

1. Click on **Add Docker**.
2. Name it **docker**.
3. In **Installation root**, tick the box **Install automatically**.
4. Click on **Add Installer**.
5. Click on **Download from docker.com**.
6. In **Docker version**, choose the version (`latest`).

**Apply and Save:**
- Click on **Apply and Save** to save all the configurations.

Click on **Manage Jenkins** to return to the main page.

### Create the CI/CD Pipeline

1. **Click on New Item.**
2. **Enter an item name:** Name your pipeline "BoardGame".
3. **Select Pipeline:** Choose the Pipeline project type.
4. **Click OK.**

### Configure the Pipeline

1. **General Settings:**
   - Click on **General**.
   - Choose **Discard old builds**.
   - In Strategy, choose **Log Rotation**.
   - In **Max # of builds to keep**, set the value to **2**. This is a best practice to limit the number of old builds and save space.

2. **Pipeline Script:**
   - Scroll down to **Pipeline**.
   - In the **Script** section, click on the dropdown in the right-hand corner and choose **Hello World**. This will create a sample (skeleton) Jenkins pipeline for you.
   - The initial script will look like this:

```groovy
pipeline {
    agent any

    stages {
        stage('Hello') {
            steps {
                echo 'Hello World'
            }
        }
    }
}
```

3. **Create Multiple Stages:**
   - Modify the initial script to create multiple stages. This is a basic example of how to structure your pipeline with multiple stages:

```groovy
pipeline {
    agent any

    stages {
        stage('Hello') {
            steps {
                echo 'Hello World'
            }
        }

        stage('Stage 2') {
            steps {
                echo 'Stage 2'
            }
        }

        stage('Stage 3') {
            steps {
                echo 'Stage 3'
            }
        }

        stage('Stage 4') {
            steps {
                echo 'Stage 4'
            }
        }

        stage('Stage 5') {
            steps {
                echo 'Stage 5'
            }
        }
    }
}
```

### Modify the Pipeline According to Your Requirements

**Add Tools Section:**

1. Add the tools section to define the JDK and Maven configurations:

```groovy
pipeline {
    agent any

    tools {
        jdk 'jdk17'    // This is the name we provided when configuring the JDK plugin
        maven 'maven3' // This is the name we provided when configuring the Maven plugin
    }
```

**Stage 1: Clone Repository**

1. Use Git Checkout to pull the project from the repository:

```groovy
    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', credentialsId: 'git-cred', url: 'https://github.com/FahadMKhan/Boardgame.git'
            }
        }
```

**Resolve Git Error Code 128**

If you encounter a Git error code 128, it generally indicates an issue with accessing the repository. This could be due to incorrect credentials, repository URL, or network issues. Note that support for password authentication was removed on August 13, 2021, which means we cannot use passwords for authentication purposes of git repositories.

**Steps to Resolve:**

1. **Open Jenkins Dashboard:**
   - Go to **Manage Jenkins**.
   - Scroll down and click on **Credentials**.
   - Click on **Global**.
   - Click on **Add Credentials**.

2. **Add GitHub Credentials:**
   - In the **Username**, enter your GitHub username.
   - In **Password**, paste the personal access token you created on GitHub.
   - In **ID**, enter an ID name like `git-cred`.
   - In **Description**, type a description (optional).
   - Click on **Create**.

3. **Update Pipeline Script:**
   - Go back to your pipeline configuration.
   - Click on **Pipeline Syntax**.
   - In the **Steps** section, click on the dropdown bar under **Sample Step** and select **git: Git**.
   - Enter the **https URL of your Git repository**.
   - In **Branch**, type/change to `main`.
   - In the **Credentials** dropdown, choose the `git-cred` that you created earlier.
   - Ensure both **Include in polling** and **Include in changelog** are checked.
   - Click on **Generate Pipeline Script**.

4. **Copy the Generated Script:**
   - Copy the script created in the box and paste it into the `steps` section of your pipeline:

```groovy
        stage('Git Checkout') {
            steps {
                git branch: 'main', credentialsId: 'git-cred', url: 'https://github.com/FahadMKhan/Boardgame.git'
            }
        }
```

**Stage 2: Compile**

1. Compile the source code to find any syntax-based errors:

```groovy
        stage('Compile') {
            steps {
                sh "mvn compile"
            }
        }
```

**Stage 3: Test**

1. Execute Maven command to run test cases:

```groovy
        stage('Test') {
            steps {
                sh "mvn test"
            }
        }
```

**Stage 4: File System Scan**

1. Install Trivy on your Jenkins VM and configure it to scan the file system for vulnerabilities:

**Install Trivy on Jenkins VM (Ubuntu):**

1. Open your Jenkins VM terminal.
2. Create a script file for Trivy installation:

```sh
vi trivy.sh
```

3. Paste the following commands into the `trivy.sh` file:

```sh
#!/bin/bash
sudo apt-get install wget apt-transport-https gnupg lsb-release -y
wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | sudo apt-key add -
echo deb https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main | sudo tee -a /etc/apt/sources.list.d/trivy.list
sudo apt-get update
sudo apt-get install trivy -y
```

4. Save the file and make it executable:

```sh
sudo chmod +x trivy.sh
```

5. Run the script to install Trivy:

```sh
./trivy.sh
```

6. Verify the installation:

```sh
trivy --version
```

**Configure the Pipeline for File System Scan:**

1. Use Trivy to

 scan the file system and generate a report:

```groovy
        stage('File System Scan') {
            steps {
                sh "trivy fs --format table -o trivy-fs-report.html ."
            }
        }
```

**Stage 5: SonarQube Analysis**

**Configure SonarQube Server:**

1. **Create a Server Authentication Token:**
   - Go to your SonarQube Server (VM).
   - Click on **Administration** in the top menu.
   - Click on **Security**.
   - Select **Users**.
   - Click on **Tokens** (on the right-hand side).
   - In **Generate Tokens**, enter a name (e.g., "sonar-token").
   - Click on **Generate**.
   - Copy the generated token.

2. **Add the Token to Jenkins:**
   - Go to your Jenkins server.
   - Go to **Dashboard**.
   - Click on **Manage Jenkins**.
   - Under **Security**, click on **Credentials**.
   - In **Credentials** under **Domain**, click on **(global)**.
   - Under **Global credentials (unrestricted)** click on **Add Credentials**.
   - In **Kind**, select **Secret text**.
   - In the **Secret** box, paste the copied token.
   - In **ID**, enter "sonar-token".
   - In **Description**, enter "sonar-token".
   - Click on **Create**.

3. **Configure SonarQube Server in Jenkins:**
   - Go to **Dashboard**.
   - Click on **Manage Jenkins**.
   - Under **System Configuration**, click on **System**.
   - Scroll down to **SonarQube Servers**.
   - Under **SonarQube Installations**, click on **Add SonarQube**.
   - In the **Name** box, enter "sonar".
   - In the **Server URL** box, enter the URL of your SonarQube Server (e.g., `http://10.101.10.101:9000`).
   - In the **Server authentication token** dropdown, select "sonar-token".
   - Click on **Apply** and **Save**.

**Add SonarQube Analysis to Pipeline:**

1. **Add SonarQube Environment:**

```groovy
    environment {
        SCANNER_HOME = tool 'sonar-scanner'
    }
```

2. **Add SonarQube Analysis Stage:**

```groovy
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonar') {
                    sh '''$SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=BoardGame \
                          -Dsonar.projectKey=BoardGame -Dsonar.java.binaries=.'''
                }
            }
        }
```

**Stage 6: Quality Gates**

1. **Configure SonarQube Webhook:**
   - Go to SonarQube server.
   - Click on **Administration**.
   - Click on **Configuration** and choose **Webhooks**.
   - Click on **Create**.
   - In **Create Webhook**, enter "Jenkins" for the name.
   - In the **URL** box, enter the URL in this format: `http://jenkins-public-ip:8080/sonarqube-webhook/` (e.g., `http://10.101.10.101:8080/sonarqube-webhook/`).
   - Click on **Create**.

2. **Add Quality Gates Stage:**

```groovy
        stage('Quality Gates') {
            steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar-token'
                }
            }
        }
```

**Stage 7: Build**

1. **Build the Application:**

```groovy
        stage('Build') {
            steps {
                sh "mvn package"
            }
        }
```

**Stage 8: Publish to Nexus**

1. **Add Repository URLs to `pom.xml`:**
   - Go to your Nexus server.
   - Click on **Browse**.
   - Copy the URLs for `maven-releases` and `maven-snapshots`.
   - Go to the repository and open `pom.xml`.
   - Add the following section if it doesn't exist:

```xml
<distributionManagement>
    <repository>
        <id>maven-releases</id>
        <url>http://13.201.68.82:8081/repository/maven-releases/</url>
    </repository>
    <snapshotRepository>
        <id>maven-snapshots</id>
        <url>http://13.201.68.82:8081/repository/maven-snapshots/</url>
    </snapshotRepository>
</distributionManagement>
```

2. **Add Nexus Credentials to Jenkins:**
   - Go to your Jenkins server.
   - Go to **Dashboard**.
   - Click on **Manage Jenkins**.
   - Click on **Managed files** under **System Configuration**.
   - Click on **Add a new config**.
   - Select **Global Maven settings.xml**.
   - Provide an ID name (e.g., "global-settings").
   - Scroll down to **Content**
   - Copy the `-->` under the `</servers>` tag as shown in the picture below:
![Nexus-setting-1](https://github.com/FahadMKhan/BoardgameListingWebApp/assets/97802721/711c4dcb-bff2-46b7-8deb-26403da9b820)
- **Paste it under the NOTE** as shown in the picture below:
![Nexus-setting-2](https://github.com/FahadMKhan/BoardgameListingWebApp/assets/97802721/fcea7679-bf72-4b79-8bb7-8a4eea43906c)
   - Add the credentials for accessing Nexus:
```xml
<servers>
    <server>
        <id>maven-releases</id>
        <username>your-username</username>
        <password>your-password</password>
    </server>
    <server>
        <id>maven-snapshots</id>
        <username>your-username</username>
        <password>your-password</password>
    </server>
</servers>
```

