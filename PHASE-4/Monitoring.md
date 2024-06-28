# Highly recomended to follow the steps shown in video

### Links to download Prometheus, Node_Exporter & black Box exporter https://prometheus.io/download/
### Links to download Grafana https://grafana.com/grafana/download
### Other link from video https://github.com/prometheus/blackbox_exporter

========================================================================================================

### Monitoring Setup with Prometheus and Grafana on AWS

We will launch a VM for monitoring. Follow these detailed steps to create an Ubuntu EC2 instance in AWS and set up Prometheus and Grafana.

#### 1. Launch an Ubuntu EC2 Instance in AWS

1. **Sign in to the AWS Management Console:**
   - Open your browser and go to [AWS Management Console](https://aws.amazon.com/console/).
   - Sign in using your AWS account credentials.

2. **Navigate to EC2:**
   - Once logged in, navigate to the EC2 dashboard by typing "EC2" in the search bar at the top or by selecting "Services" and then "EC2" under the "Compute" section.

3. **Launch Instance:**
   - Click on the "Instances" link in the EC2 dashboard sidebar.
   - Click the "Launch Instance" button.

4. **Configure Instance:**
   - **Number of Instances:** In the Summary section, choose the number of instances. For this guide, select `1`.
   - **Name and Tags:** In the Name and tags section, name the instance `Monitor`.

5. **Choose an Amazon Machine Image (AMI):**
   - In the "Application and OS Images (Amazon Machine Image)" section, select "Ubuntu" from the list of available AMIs.
   - Choose the Ubuntu version, e.g., "Ubuntu, 22.04 LTS, amd64 jammy image build on 2024-04-11".
   - Click "Select".

6. **Choose an Instance Type:**
   - In the "Choose an Instance Type" section, select the instance type that fits your requirements. The default option (usually `t2.micro`) is suitable for testing and small workloads. For this guide, choose `t2.medium` (2 vCPU, 4 GiB Memory).
   - Click "Next: Configure Instance Details".

7. **Configure Instance Details:**
   - **Key Pair (login):**
     - Ensure you have access to the selected key pair to securely connect to your instance. If you already have one configured, you can use the same key pair.
     - Choose the key pair name from the dropdown menu or create a new key pair.
   - **Network Settings:**
     - A security group is a set of firewall rules that control the traffic for your instance. Add rules to allow specific traffic to reach your instance.
     - You can create a new security group, but for this guide, select an existing security group.
     - Click on "Select existing Security Group".
     - In the "Common security groups" dropdown, choose your existing security group.
   - Click "Next: Add Storage".

8. **Add Storage:**
   - Specify the size of the root volume. For this guide, choose `1x 20 GiB`.
   - Click "Next: Add Tags".

9. **Add Tags:**
   - Optionally, add tags to your instance for easier management.
   - Click "Next: Configure Security Group".

10. **Review and Launch:**
    - Review the configuration of your instance.
    - Click "Launch".
    - Check the acknowledgment box and click "Launch Instances".

11. **Access Your Instance:**
    - Once the instance is launched, copy the Public IPv4 address of your VM.

#### 2. Connect to the Instance Using Mobaxterm

1. **Use Mobaxterm:**
   - Open Mobaxterm.
   - Create a duplicate of an existing VM session in Mobaxterm.
   - Rename it to `Monitor`.
   - Change the Remote host IP address to your VM's Public IPv4 address, e.g., `10.1.1.1`.
   - Click OK.
   - Connect to the session by double-clicking on `Monitor`.

2. **Update Package Lists:**
   - Once connected, update the package lists:
     ```bash
     sudo apt-get update
     ```

#### 3. Set Up Prometheus

1. **Download and Install Prometheus:**
   - Go to [Prometheus Download Page](https://prometheus.io/download/).
   - Download the Prometheus package:
     ```bash
     wget https://github.com/prometheus/prometheus/releases/download/v2.53.0/prometheus-2.53.0.linux-amd64.tar.gz
     ```
   - Verify the downloaded package:
     ```bash
     ls
     ```
   - Extract the package:
     ```bash
     tar -xvf prometheus-2.53.0.linux-amd64.tar.gz
     ```
   - Remove the tar file:
     ```bash
     rm -rf prometheus-2.53.0.linux-amd64.tar.gz
     ```
   - Change to the extracted directory:
     ```bash
     cd prometheus-2.53.0.linux-amd64
     ```
   - Verify the contents:
     ```bash
     ls
     ```
   - Run Prometheus in the background:
     ```bash
     ./prometheus &
     ```

2. **Access Prometheus:**
   - Copy the VM Public IPv4 address and paste it in the browser with port `9090`, e.g., `http://10.1.1.1:9090`.

#### 4. Set Up Grafana

1. **Download and Install Grafana:**
   - Go to [Grafana Download Page](https://grafana.com/grafana/download).
   
   **Ensure you are out of the directories and at the main VM prompt, e.g., `ubuntu@ip-10.1.1.1:`.**

   - Run the following commands to install Grafana:
     ```bash
     sudo apt-get install -y adduser libfontconfig1 musl
     ```
     - This command installs necessary dependencies for Grafana.

     ```bash
     wget https://dl.grafana.com/enterprise/release/grafana-enterprise_11.1.0_amd64.deb
     ```
     - This command downloads the Grafana package.

     ```bash
     sudo dpkg -i grafana-enterprise_11.1.0_amd64.deb
     ```
     - This command installs Grafana from the downloaded package.

2. **Start Grafana:**
     - Run the following command to start Grafana:
     ```bash
     sudo /bin/systemctl start grafana-server
     ```

3. **Access Grafana:**
   - Open your browser and navigate to `http://10.1.1.1:3000` (replace `10.1.1.1` with your VM's Public IPv4 address).
   - Log in using the default credentials:
     - Username: `admin`
     - Password: `admin`
   - You will be prompted to change the password:
     - New password: `Grafana`
     - Confirm new password: `Grafana`
   - Click on "Submit".

You have now successfully set up monitoring on your VM with Prometheus and Grafana.

### Installing Black Box Exporter

The Black Box Exporter is a tool that allows you to monitor the availability and performance of websites and other services. Follow these steps to download, install, and run the Black Box Exporter on your monitoring VM.

1. **Browse to Prometheus Downloads:**
   - Open your browser and go to [Prometheus Download Page](https://prometheus.io/download/).
   - In the search box, type `blackbox_exporter` or click directly on this [link](https://prometheus.io/download/#blackbox_exporter) to find the Black Box Exporter.

2. **Download the Black Box Exporter Package:**
   - Open your Monitor VM terminal.
   - Download the Black Box Exporter package by running the following command:
     ```bash
     wget https://github.com/prometheus/blackbox_exporter/releases/download/v0.25.0/blackbox_exporter-0.25.0.linux-amd64.tar.gz
     ```

3. **Extract the Package:**
   - Extract the downloaded tar file:
     ```bash
     tar -xvf blackbox_exporter-0.25.0.linux-amd64.tar.gz
     ```

4. **Verify the Downloaded Package:**
   - List the contents to verify the extraction:
     ```bash
     ls
     ```

5. **Remove the Tar File:**
   - Remove the extracted tar file to clean up the directory:
     ```bash
     rm -rf blackbox_exporter-0.25.0.linux-amd64.tar.gz
     ```

6. **Change to the Extracted Directory:**
   - Navigate to the extracted directory:
     ```bash
     cd blackbox_exporter-0.25.0.linux-amd64
     ```

7. **Verify the Contents:**
   - List the contents to ensure everything is in place:
     ```bash
     ls
     ```

8. **Run the Black Box Exporter:**
   - Run the Black Box Exporter in the background:
     ```bash
     ./blackbox_exporter &
     ```
   - By default, the Black Box Exporter runs on port `9115`.

9. **Access the Black Box Exporter:**
   - Copy the Public IPv4 address of your VM.
   - Open your browser and navigate to `http://<VM Public IPv4>:9115`. For example, if your VM's IP is `10.1.1.1`, go to:
     ```plaintext
     http://10.1.1.1:9115
     ```

10. **Verify Installation:**
    - You should see the Black Box Exporter display page, confirming that the Black Box Exporter is running successfully.

You have now successfully installed and started the Black Box Exporter on your monitoring VM.
