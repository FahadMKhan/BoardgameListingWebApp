### Creating CI/CD Pipelines

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

        stage('Hello') {
            steps {
                echo 'Hello World'
            }
        }

        stage('Hello') {
            steps {
                echo 'Hello World'
            }
        }

        stage('Hello') {
            steps {
                echo 'Hello World'
            }
        }

        stage('Hello') {
            steps {
                echo 'Hello World'
            }
        }
    }
}
```

Now let's modify the pipeline according to our requirements:

**Stage 1:**

Whenever you install a plugin for Jenkins and you want to use it in your pipeline, you would need to define it inside your pipeline in one way or another.

To do that, we will write the following:

```groovy
pipeline {
    agent any

    tools {
        jdk 'jdk17'    // This is the name we provided when configuring the JDK plugin
        maven 'maven3' // This is the name we provided when configuring the Maven plugin
    }

    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', credentialsId: 'git-cred', url: 'Your git-repo url'
            }
        }
    }
}
```

In the first stage, we want to work with the project that should be available on the local workspace. For that, we will use "Git Checkout" to pull it.

If you don't know how to write the command for any stage, scroll down to "Pipeline Syntax" to take help from the pipeline.

In our case on how to write the Git Checkout command:

1. Click on **Pipeline Syntax**.
2. In the **Steps** section, click on the dropdown bar under **Sample Step** and select **git: Git**.
3. In the **Repository URL**, enter the "https URL of your git repository".
4. In **Branch**, type/change to "main".

If you encounter a Git error code 128, it generally indicates an issue with accessing the repository. This could be due to incorrect credentials, repository URL, or network issues. Note that support for password authentication was removed on August 13, 2021, meaning we cannot use passwords for authentication purposes of git repository. To resolve this:

1. Open the Dashboard in a separate tab.
2. Go to **Dashboard**.
3. Click on **Manage Jenkins**.
4. Scroll down and click on **Credentials**.
5. Click on **Global**.
6. Click on **Add Credentials**.
7. In the **Username**, enter a username (your GitHub username).
8. In **Password**, copy the token that we created and paste it in here. Here is an example token for you: `ghp_NTi10iImmnJKS52fqk5hsnfHaMAwjeyF88hb29`.
9. In **ID**, enter an ID name "git-cred" (make sure you enter an ID name; otherwise, it will create a long ID name which is very hard to remember).
10. In **Description**, type anything. (I normally type the same ID name in the description, i.e., "git-cred").
11. Click on **Create**.

Once the above steps are completed, go back to your pipeline configuration.
1. Click on **Pipeline Syntax**.
2. Click on the dropdown bar under **Sample Step** and select **git: Git**.
3. Enter the "https URL of your git repository".
4. In **Branch**, type/change to "main".
5. In the **Credentials** dropdown, choose the "git-cred" that we created earlier.
6. Make sure both boxes are ticked for the below:
    - Include in polling
    - Include in changelog

7. Click on **Generate Pipeline Script**.

Copy the script created in the box and paste it in the steps:

```groovy
stages {
    stage('Git Checkout') {
        steps {
            git branch: 'main', credentialsId: 'git-cred', url: 'Your git-repo url'
        }
    }
}
```

**Stage 2: Compile**

In the next step, we will compile our source code to find any syntax-based errors in our source code. For that, we will execute Maven commands, which are usually Shell commands. In order to run a command in Shell, we will enclose it in `sh`. i.e., `sh "mvn compile"`.

```groovy
stage('Compile') {
    steps {
        sh "mvn compile"
    }
}
```

**Stage 3: Test**

In the next step, we will execute Maven commands to execute test cases. For that, we can write the command in Shell, i.e., `sh "mvn test"`.

```groovy
stage('Test') {
    steps {
        sh "mvn test"
    }
}
```

**Stage 4: File System Scan**

In the next step, we will scan the entire file system, meaning scan the entire source

 code to find out the vulnerabilities or any sensitive data that may exist/stored in the dependencies that we are using for this project. You can find all the dependencies in my repo Boardgame and in the `pom.xml` file.

To find the vulnerabilities or any sensitive data that may exist/stored in the dependencies, we will be using a third-party tool called "Trivy". As there is no plugin available for Trivy, we will be installing it on our Jenkins VM using commands.

Steps to install Trivy on Jenkins VM (Ubuntu): [Trivy Installation](https://aquasecurity.github.io/trivy/v0.18.3/installation/)

1. Open your Jenkins VM.
2. Type `vi trivy.sh`.
3. Paste the entire command line in `vi`.

```bash
sudo apt-get install wget apt-transport-https gnupg lsb-release
wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | sudo apt-key add -
echo deb https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main | sudo tee -a /etc/apt/sources.list.d/trivy.list
sudo apt-get update
sudo apt-get install trivy -y
```

4. Save the file and exit by pressing `Esc`, then type `:wq` and press `Enter`.
5. Make the script executable by running `sudo chmod +x trivy.sh`.
6. Execute the script by running `./trivy.sh`.
7. Verify the installation by running `trivy --version`.

After installing Trivy, if we write `sh "trivy fs ."` in the steps, it will generate the report inside the console log, which is hard to analyze. So for that reason, we want Trivy to generate the report in a specific format and export it inside a separate file. The command `sh "trivy fs --format table -o trivy-fs-report.html ."` will do this.

```groovy
stage('File System Scan') {
    steps {
        sh "trivy fs --format table -o trivy-fs-report.html ."
    }
}
```

**SonarQube Analysis**

We want to perform SonarQube Analysis, so for that, we would need to configure the SonarQube Server first.

First, we will create a "Server authentication token" for the SonarQube server.

1. Go to your SonarQube Server (VM).
2. Click on **Administration** in the top menu.
3. Click on **Security**.
4. Select **Users**.
5. To the right-hand side, you will see **Tokens**, click on the three lines.
6. In **Generate Tokens**, in the **Name** box, write a name "sonar-token".
7. Click on **Generate**.
8. Copy the token.

Now go to your Jenkins server.

1. Go to the Dashboard.
2. Click on **Manage Jenkins**.
3. Under **Security**, click on **Credentials**.
4. In **Credentials** under **Domain**, click on **(global)**.
5. Under **Global credentials (unrestricted)**, click on **Add Credentials**.
6. In **New credentials**, in the dropdown of **Kind**, select **secret text**.
7. In the **Secret** box, paste the copied token.
8. In the **ID** box, type "sonar-token".
9. In the **Description** box, type "sonar-token".
10. Click on **Create**.

Go to:

1. **Dashboard**
2. **Manage Jenkins**
3. **System Configuration**
4. **System**

Scroll down to the point where you will find **SonarQube Servers**.

1. Under **SonarQube Installations**, click on **Add SonarQube**.
2. In the **Name** box, type "sonar".
3. Now copy the URL of your SonarQube-Server along with the port number and paste it in the **Server URL** box, e.g., `10.101.10.101:9000` (make sure you remove the last `/` from the server URL).
4. In the **Server authentication token**, select "sonar-token" we created earlier.
5. Click on **Apply and Save**.

As the SonarQube configuration has completed, we will write the steps that will perform SonarQube analysis. In order to write the steps, we need to write them inside a block that will have the credentials and the server URL of the SonarQube on which we are supposed to publish the report.

To write the block:

1. Scroll down to **Pipeline Syntax** to take help from the pipeline.
2. Click on **Pipeline Syntax**.
3. Click on the dropdown bar under **Sample Step** and select **withSonarQubeEnv: Prepare SonarQube Scanner environment**.

Note: If you are unable to find this option, simply click on the Jenkins URL and type `restart` at the end, i.e., `10.101.10.101:8080/restart`. It will restart Jenkins and the issue will be resolved.

4. In the **Server authentication token** dropdown, choose "sonar-token" that we did earlier.
5. Click on **Generate Pipeline Script**.

The script will look like this:

```groovy
withSonarQubeEnv(credentialsID:'sonar-token'){
    // some block
}
```

Now we will modify the script as we do not want to add `(credentialsID:'sonar-token')`, so we remove this and simply type `('sonar')`.

The reason we are doing this is: In order to access a server, you need the following things: URL, username, and password. So instead of username and password, we are using a token and we have the URL of the Jenkins server.

To verify:

1. Go to **Dashboard**.
2. Click on **Manage Jenkins**.
3. Click on **System**.
4. Scroll down to **SonarQube Servers**.
5. Under **Name**, you will see "sonar". Under **Server URL**, you will see the URL of our Jenkins server, and under **Server authentication token**, you will see "sonar-token".

So we will modify our pipeline script to `withSonarQubeEnv('sonar')` meaning we are calling the Jenkins server by mentioning "sonar" in our stage.

Copy the script created in the box and paste it in the steps:

```groovy
stage('SonarQube Analysis') {
    steps {
        withSonarQubeEnv('sonar'){
            // some block
        }
    }
}
```

As discussed earlier, when you install a third-party tool via plugin for Jenkins and you want to use it in your pipeline, you would need to define it inside your pipeline in one way or another.

To do that, we will write the following:

```groovy
pipeline {
    agent any

    environment {
        SCANNER_HOME = tool 'sonar-scanner'
    }
}
```

Now we will go back to our pipeline and mention this over there. As we have multiple lines of code like project name, project key, etc., we will be using triple quotes `'''` instead of double quotes `""` and call that variable, and will close the block with `'''` quotes. Any tool that you install, the executable file of that tool will be in the `/bin` folder and inside the `/bin` folder we will be able to see `sonar-scanner`. By doing this, we are calling the sonar-scanner tool by providing the location of it. Next, we will pass on some arguments like:

- `-Dsonar.projectName=BoardGame`
- `-Dsonar.projectKey=BoardGame`
- `-Dsonar.java.binaries=.`

To write the argument in the next line, use `\` as you will see in the steps. As we are using `'''` quotes, we will stay in the same block.

See the steps below:

```groovy
stage('SonarQube Analysis') {
    steps {
        withSonarQubeEnv('sonar'){
            sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=BoardGame \
                  -Dsonar.projectKey=BoardGame -Dsonar.java.binaries=. '''
        }
    }
}
```

**Stage 5: Quality Gates**

In the next step, we will perform SonarQube Conditions called "Quality Gates". So if the conditions are passed, we can then say that our source code is of good quality, etc.

To write the Quality Gates:

1. Go to SonarQube server.
2. Click on **Administration**.
3. Click on **Configuration** and choose **Webhooks**.
4. Click on **Create**.
5. In **Create Webhook**:
    - Name: Jenkins
    - URL: You need to write the URL in this format: `http://jenkins-public-ip:8080/sonarqube-webhook/`, e.g., `http://10.101.10.101:8080/sonarqube-webhook/`.
6. Click on **Create**.

Now we will add a stage for SonarQube to perform its analysis. 

```groovy
stage('Quality Gates') {
    steps {
        script {
            waitForQualityGate abortPipeline: false, credentialsId: 'sonar-token'
        }
    }
}
```

**Stage 6: Build**

In the next stage, we will build the application:

```groovy
stage('Build') {
    steps {
        sh "mvn package"
    }
}
``

`

**Stage 7: Publish to Nexus**

In this stage, we will publish the artifacts to Nexus. In order to publish to Nexus, we need to add our repository URLs to our `pom.xml` file.

1. Go to your Nexus server.
2. Click on **Browse**.
3. Copy the below files URL one by one:
    - maven-releases
    - maven-snapshots
4. Go to the repository and open `pom.xml` file.
5. Scroll down to the end and you will find the section below:

If you do not find it, you can manually add the Maven releases repo name `maven-releases` and Maven releases URL. Also add `snapshotRepository` repo name `maven-snapshots` and `snapshotRepository` URL.

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

Click on **Commit changes**.

Now we have to add the credentials to access the repository. In order to do that, go to your Jenkins server.

1. Go to **Dashboard**.
2. Click on **Manage Jenkins**.
3. As we have installed a plugin called **Config File Provider**, we will see **Managed files** under **System Configuration**.
4. Click on **Managed files**.
5. Click on **Add a new config**.
6. Under **New Configuration**, select **Global Maven settings.xml**.

This section gives you an option to create credentials according to your needs. For us, we are using Java, so we will go with **Global Maven settings.xml**.

7. Scroll to the bottom and provide the ID name in the **ID of the config file** box:
    - ID name: "global-settings"
8. Click on **Next**.
9. Scroll down until you find **Content**.
10. Under **Content**, you will find `settings.xml` file.
11. Here we will provide the credentials to access Nexus.
12. Scroll down `settings.xml` file until you find an option by the name `<servers>`.
13. Copy the configuration under the `</server>`.

Paste it under the `NOTE` as shown in the picture:

![Nexus-setting-1](https://github.com/FahadMKhan/BoardgameListingWebApp/assets/97802721/711c4dcb-bff2-46b7-8deb-26403da9b820)
![Nexus-setting-2](https://github.com/FahadMKhan/BoardgameListingWebApp/assets/97802721/fcea7679-bf72-4b79-8bb7-8a4eea43906c)

14. Save and Apply the changes.

### Configure Jenkins Pipeline for Maven and Docker Integration

**Configure Maven**

1. Go to Dashboard.
2. Click on **Boardgame**.
3. Click on **Configure**.
4. Scroll all the way down to **Pipeline Syntax**.
5. Click on **Pipeline Syntax**.
6. Click on the dropdown bar under **Sample Step** and select **withMaven: Provide Maven environment**.
7. Enter the following details:
    - Maven: maven3
    - JDK: jdk17
    - Maven Settings Config: Leave empty or the default setting as we have not configured it.
    - Global Maven Settings Config: MyGlobalSettings
    - Tick **Maven Traceability**.
8. Click on **Generate Pipeline Script**.

Example Script: `withMaven(globalMavenSettingsConfig: 'xyz'){ //some block }`

Copy the script and paste it in the step and instead of `//some block`, write the following in order to deploy the artifacts of this application to Nexus:

```groovy
stage('Publish To Nexus') {
    steps {
        withMaven(globalMavenSettingsConfig: 'xyz') {
            sh "mvn deploy"
        }
    }
}
```

**Configure Docker**

1. To write Docker commands, scroll all the way down to **Pipeline Syntax**.
2. Click on **Pipeline Syntax**.
3. Click on the dropdown bar under **Sample Step** and select **withDockerRegistry: Sets up Docker registry endpoint**.
4. Enter the following details:
    - Docker registry URL: Since we are using a public docker registry, we can leave it blank but if you are using a private one, you need to provide the URL.
    - Registry credentials: Instead of going to the Credential section to manage Jenkins, we can simply click on **+Add** and select **Jenkins**.
    - In Jenkins Credentials Provider: Jenkins
    - Scroll down to Username.
    - Username: Your Docker Hub username.
    - Password: Your Docker Hub password.
    - ID: docker-cred
    - Description: docker-cred.
5. Click on **Add**.

You will now see your Pipeline Syntax steps. If not:

1. Go to **Dashboard**.
2. Select **BoardGame**.
3. Click on **Pipeline Syntax**.
4. Scroll down to **Registry credentials**.
5. Select the **docker-cred** from your dropdown that we created earlier.
6. In the Docker installation, select the **docker** from the dropdown that we created earlier.
7. Click on **Generate Pipeline Script**.

Example Script: `withDockerRegistry(credentialId:'docker-cred', xyz'){ //some block }`

Note that the Docker file already exists inside our root directory `Boardgame` so we need to build it. For Docker command, we will add a Script to the stage steps and will write the commands inside it. We will remove `//some block` with `sh "Docker build command"`.

```groovy
stage('Build & Tag Docker Image') {
    steps {
        script {
            withDockerRegistry(credentialId:'docker-cred', xyz') {
                sh "docker build -t username/nameofyourchoice:latest ."
            }
        }
    }
}
```

**Stage 8: Docker Image Scan**

In this step, we will be scanning the Docker image before we push it to Docker Hub repository. For that, we will use "Trivy". We will let Trivy know what to scan, i.e., image and the image name which we have set up in the stage step above: `username/nameofyourchoice:latest`.

```groovy
stage('Docker Image Scan') {
    steps {
        sh "trivy image --format table -o trivy-fs-report.html username/nameofyourchoice:latest"
    }
}
```

**Stage 9: Push Docker Image**

In this stage, we will push our Docker image to our Docker Hub repository.

```groovy
stage('Push Docker Image') {
    steps {
        script {
            withDockerRegistry(credentialId:'docker-cred', xyz') {
                sh "docker push username/nameofyourchoice:latest"
            }
        }
    }
}
```

After this step, now we can deploy the application to Kubernetes Cluster.

**Deploy to Kubernetes Cluster**

In order to deploy the application to Kubernetes Cluster in a professional way, you need to create a "Service Account" so we can use RBAC. RBAC means "Role-Based Access Control". This is use to secure our deployment.

**Example of RBAC:**

RBAC in Kubernetes is a method of regulating access to resources based on the roles of individual users within your cluster. This is done by defining roles and then binding those roles to users or groups.

**Steps to Create RBAC:**

### Creating CI/CD Pipelines

**First of all we need to create a service account**

1. **Create a Service Account:**

- **We will create a user.**
- **Go to your Master terminal.**
- **Change to the root user using the command below:**
  ```sh
  sudo su
  ```
- **Once you are the Root-user, create a vi svc.yaml:**
  ```sh
  vi svc.yaml
  ```
- **Paste the below in vi svc.yaml:**
  ```yaml
  apiVersion: v1
  kind: ServiceAccount
  metadata:
    name: jenkins
    namespace: webapps
  ```
  *(What the above is doing is creating a service account which will be used to perform the deployment. The name of the user is Jenkins and we are going to do the deployment in a separate project or a namespace called webapps.)*

- **Save the file.**

- **Now we will create the namespace using the command below:**
  ```sh
  kubectl create ns webapps
  ```
  *You will see the below outcome:*
  ```
  namespace/webapps created
  ```

- **Now we will execute the vi svc.yaml:**
  ```sh
  kubectl apply -f svc.yaml
  ```
  *You will see the below outcome:*
  ```
  serviceaccount/jenkins created
  ```

- **Now we have successfully created the service account inside the webapps namespace.**

2. **Create a Role:**
   *(This role that we are creating will have full permission to get details, list details, watch it, create, update, patch, delete etc. So in short, the role will have complete access that is required to perform any deployment.)*

- **We will create a role.**
- **Create a vi role.yaml:**
  ```sh
  vi role.yaml
  ```
- **Paste the below contents in vi role.yaml:**
  ```yaml
  apiVersion: rbac.authorization.k8s.io/v1
  kind: Role
  metadata:
    name: app-role
    namespace: webapps
  rules:
    - apiGroups:
        - ""
        - apps
        - autoscaling
        - batch
        - extensions
        - policy
        - rbac.authorization.k8s.io
      resources:
        - pods
        - secrets
        - componentstatuses
        - configmaps
        - daemonsets
        - deployments
        - events
        - endpoints
        - horizontalpodautoscalers
        - ingress
        - jobs
        - limitranges
        - namespaces
        - nodes
        - pods
        - persistentvolumes
        - persistentvolumeclaims
        - resourcequotas
        - replicasets
        - replicationcontrollers
        - serviceaccounts
        - services
      verbs: ["get", "list", "watch", "create", "update", "patch", "delete"]
  ```
  *You can see that the role we are creating is for webapps and the name of the role will be app-role.*

- **Save the file.**

- **Now we will execute the vi role.yaml:**
  ```sh
  kubectl apply -f role.yaml
  ```
  *You will see the below outcome:*
  ```
  role.rbac.authorization.k8s.io/app-role created
  ```

- **Now we have successfully created the role.**

3. **Create a RoleBinding:**
   *(Now we will bind the role to the service account.)*

- **Create a vi bind.yaml:**
  ```sh
  vi bind.yaml
  ```
- **Paste the below contents in vi bind.yaml:**
  ```yaml
  apiVersion: rbac.authorization.k8s.io/v1
  kind: RoleBinding
  metadata:
    name: app-rolebinding
    namespace: webapps 
  roleRef:
    apiGroup: rbac.authorization.k8s.io
    kind: Role
    name: app-role 
  subjects:
  - namespace: webapps 
    kind: ServiceAccount
    name: jenkins 
  ```
  *(This RoleBinding associates the app-role Role with the jenkins ServiceAccount in the webapps namespace, allowing Jenkins to perform actions defined in the Role.)*

- **Save the file.**

- **Now we will execute the vi bind.yaml:**
  ```sh
  kubectl apply -f bind.yaml
  ```
  *You will see the below outcome:*
  ```
  rolebinding.rbac.authorization.k8s.io/app-rolebinding created
  ```

- **Now we have successfully bound the role to the service account. The user we created with Jenkins user has full permission to perform the deployment and everything required.**

4. **Create a Token for Jenkins to Connect to Kubernetes:**
   *(Now we want our Jenkins to connect to our Kubernetes cluster. For that, we will create a Token that will be used for authentication.)*

- **Browse to the Kubernetes.io website:**
  [Kubernetes Documentation](https://kubernetes.io/docs/reference/access-authn-authz/service-accounts-admin/#create-token)

- **Copy the contents from: secret/serviceaccount/mysecretname.yaml**

- **Create a vi secret.yaml:**
  ```sh
  vi secret.yaml
  ```

- **Paste the below contents in vi secret.yaml:**
  ```yaml
  apiVersion: v1
  kind: Secret
  type: kubernetes.io/service-account-token
  metadata:
    name: mysecretname
    annotations:
      kubernetes.io/service-account.name: jenkins
  ```
  *(Note that we need to change the service-account name to our service-account name which is jenkins: 
  -> (kubernetes.io/service-account.name: myserviceaccount) change to -> (kubernetes.io/service-account.name: jenkins)*

- **Save the file.**

- **Now we will execute the vi secret.yaml:**
  ```sh
  kubectl apply -f secret.yaml -n webapps
  ```
  *(-n is used for namespace. So if you have not mentioned your namespace in your .yaml file, you can manually use it in your command so that the resource that we are creating should be created in that namespace.)*

  *You will see the below outcome:*
  ```
  secret/mysecretname created
  ```

5. **Display the Authentication Token:**

- **In order to print the authentication Token on your Master terminal, execute the following command:**
  ```sh
  kubectl describe secret mysecretname -n webapps
  ```
  *(The above command prints detailed information about the secret mysecretname in the webapps namespace, including the token value that can be used for authentication.)*

  *It should print something like:*
  ```
  eyJhbGciOiJSUzI1NiIsImtpZCI6IiJ9.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlcm5ldGVzLWRhc2hib2FyZCIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJhZG1pbi11c2VyLXRva2VuLXY1N253Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZXJ2aWNlLWFjY291bnQubmFtZSI6ImFkbWluLXVzZXIiLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC51aWQiOiIwMzAzMjQzYy00MDQwLTRhNTgtOGE0Ny04NDllZTliYTc5YzEiLCJzdWIiOiJzeXN0ZW06c2VydmljZWFjY291bnQ6a3ViZXJuZXRlcy1kYXNoYm9hcmQ6YWRtaW4tdXNlciJ9.Z2JrQlitASVwWbc-s6deLRFVk5DWD3P_vjUFXsqVSY10pbjFLG4njoZwh8p3tLxnX_VBsr7_6bwxhWSYChp9hwxznemD5x5HLtjb16kI9Z7yFWLtohzkTwuFbqmQaMoget_nYcQBUC5fDmBHRfFvNKePh_vSSb2h_aYXa8GV5AcfPQpY7r461itme1EXHQJqv-SN-zUnguDguCTjD80pFZ_CmnSE1z9QdMHPB8hoB4V68gtswR1VLa6mSYdgPwCHauuOobojALSaMc3RH7MmFUumAgguhqAkX3Omqd3rJbYOMRuMjhANqd08piDC3aIabINX6gP5-Tuuw2svnV6NYQ
  ```

6. **Add the Token to Jenkins Global Credentials

:**

- **Go to Jenkins.**
- **Click on Manage Jenkins.**
- **Click on Credentials.**
- **Click on System.**
- **Click on Global credentials (unrestricted).**
- **Click on + Add Credentials.**
- **In Kind, choose secret text.**
- **In Secret, paste the above created authentication Token.**
- **In ID, write k8-cred.**
- **In Description, write k8-cred.**

7. **Configure Jenkins Pipeline to Use Kubernetes:**

- **Now go back to your BoardGame pipeline.**
- **Click on Dashboard.**
- **Click on BoardGame.**
- **Click on Configuration.**
- **Scroll all the way down to Pipeline Syntax.**
- **Click on Pipeline Syntax.**
- **In Sample Step, choose: withKubeConfig: Configuration Kubernetes CLI (kubectl).**
- **In Credentials, choose the credential we created earlier: k8-cred.**

- **Now we need to enter our Kubernetes server endpoint and Cluster name. In order to get Kubernetes server endpoint and Cluster name, go to your Master terminal.**
- **Change directory to .kube, type:**
  ```sh
  cd ~/.kube 
  ```
  *(This is the folder where we will find the config file which holds the Kubernetes server endpoint.)*

- **Once inside ~/.kube, type:**
  ```sh
  ls
  ```
  *(You will see cache and config files as the outcome of the above command.)*

- **Now we will print the contents inside config, type:**
  ```sh
  cat config
  ```
  *(The above command will print all the details of our cluster.)*

- **Scroll down to the point when you see "server: https://ipaddress:6443" (This is our Kubernetes server endpoint that we need) and "cluster: yourclustername" (This is our Cluster name).**
  *For example:*
  ```sh
  server: https://10.101.10.101:6443
  cluster: kubernetes
  ```

- **Copy the server address and cluster name and paste it in the box of Kubernetes server endpoint and Cluster name in your Jenkins Pipeline syntax.**
  ```sh
  Kubernetes server endpoint: server: https://10.101.10.101:6443
  Cluster name: kubernetes
  ```

- **Now in the Namespace, type: (Our namespace that we created was "webapps")**
  ```sh
  Namespace: webapps          
  ```

- **Click on Generate Pipeline Script.**
  *e.g. Script:*
  ```groovy
  withKubeConfig(caCertificate: '', clusterName: 'kubernetes', contextName: ''xyz) {
  // some block
  }
  ```

- **Copy the Script generated.**

8. **Add Deployment Stage in Jenkins Pipeline:**

- **Now go to your Jenkins Pipeline.**
- **Click on Dashboard.**
- **Click on BoardGame.**
- **Click on Configuration.**

- **Inside Script, we will create a stage to execute the deployment. In order to do that we need to use a manifest file which we have in our "Boardgame" repository by the file name "deployment-service.yaml"** [deployment-service.yaml](https://github.com/FahadMKhan/Boardgame/blob/main/deployment-service.yaml).
  *Please see the contents of "deployment-service.yaml" file below which I have copied from [deployment-service.yaml](https://github.com/FahadMKhan/Boardgame/blob/main/deployment-service.yaml). You can modify it as per your own requirements so it becomes the manifest file for your own project.*

  *For example:*
  ```yaml
  apiVersion: apps/v1
  kind: Deployment # Kubernetes resource kind we are creating
  metadata:
    name: boardgame-deployment  #(Can change name: yourprojectname-deployment)
  spec:
    selector:
      matchLabels:
        app: boardgame  #(Can change app: yourprojectname)
    replicas: 2 # Number of replicas that will be created for this deployment
    template:
      metadata:
        labels:
          app: boardgame  #(Can change app: yourprojectname)
      spec:
        containers:
          - name: boardgame  #(Can change app: yourprojectname)
            image: adijaiswal/boardgame:latest #(Docker image can be changed image: yourprojectname. We can get this information from the Push Docker Image stage in our Jenkins Pipeline) Image that will be used to containers in the cluster
            imagePullPolicy: Always
            ports:
              - containerPort: 8080 # (We can get this info from our Dockerfile in our Boardgame repo "https://github.com/FahadMKhan/Boardgame/blob/main/Dockerfile"). The port that the container is running on in the cluster
  ```

  ```yaml
  ---
  apiVersion: v1 # Kubernetes API version
  kind: Service # Kubernetes resource kind we are creating
  metadata: # Metadata of the resource kind we are creating
    name: boardgame-ssvc  #(Can change app: yourprojectname-ssvc)
  spec:
    selector:
      app: boardgame  #(Can change app: yourprojectname)
    ports:
      - protocol: "TCP"
        port: 8080  #(We can get this info from our Dockerfile in our Boardgame repo "https://github.com/FahadMKhan/Boardgame/blob/main/Dockerfile". The port that the service is running on in the cluster
        targetPort: 8080  #(We can get this info from our Dockerfile in our Boardgame repo "https://github.com/FahadMKhan/Boardgame/blob/main/Dockerfile"). The port exposed by the service
    type: LoadBalancer  # type of the service.
  ```

  *"In Kubernetes, a Service is an abstract way to expose an application running on a set of Pods as a network service. Kubernetes Services support several types, each designed for different use cases. Here’s a breakdown of the three primary types of Services you mentioned: ClusterIP, NodePort, and LoadBalancer."*

  **ClusterIP** is the default Kubernetes Service that provides an internal IP address for in-cluster communication between Pods, ensuring services are accessible internally but not from the external network.

  **NodePort** is a Kubernetes Service type that exposes a service on a static port on each Node’s IP address, allowing external access to the service. This setup is useful for making applications accessible from outside the cluster by accessing any node at a specified port. As it opens a port on a worker Node, it is not a secure way, so we should avoid NodePort for external access unless required by your project or specifically requested by your organization.

  **LoadBalancer** is a Kubernetes Service type that integrates with external cloud-based load balancers, automatically routing external traffic to the service across cluster nodes, making it ideal for handling incoming internet traffic to applications. This feature is available by default on all cloud platforms, meaning this feature is available in the Kubernetes cluster by default for us to use it.

- **Once the changes are done according to our need in our deployment-service.yaml, now we will create the "Deploy To Kubernetes" stage and steps:**

- **Paste the example script in the steps below:**
  *e.g. Script:*
  ```groovy
  withKubeConfig(caCertificate: '', clusterName: 'kubernetes', contextName: ''xyz) {
  // some block
  }
  ```
  *For the Kubernetes apply command, we will add a Script to the stage steps and will write the commands inside it. We will remove // some block with sh "kubectl apply -f our-manifested-filename.yaml command".*

  ```groovy
  stage('Deploy To Kubernetes') {
      steps {
         withKubeConfig(caCertificate: '', clusterName: 'kubernetes', contextName: ''xyz) { 
              sh "kubectl apply -f deployment-service.yaml"
         } 
      }
  }
  ```
  
