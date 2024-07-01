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

At this stage, all 3 applications Prometheus, Grafana and Blackbox are running but we need to apply certain changes to Prometheus.yaml file

To see the required configuration, click on: https://github.com/FahadMKhan/blackbox_exporter/blob/master/README.md#prometheus-configuration

### Explanation and Summary of the Stage

At this stage, you are configuring Prometheus to use the Blackbox Exporter for monitoring various endpoints. Here's a step-by-step explanation and summary of why this stage is necessary and what the outcome will be.

### Explanation

1. **Applications Running**:
   - You have three applications running: Prometheus (for metrics collection), Grafana (for visualisation), and Blackbox Exporter (for probing and monitoring specific endpoints).

2. **Prometheus Configuration File**:
   - The `prometheus.yaml` file is where Prometheus' configuration is defined. This includes specifying which endpoints to scrape metrics from.

3. **Blackbox Exporter Integration**:
   - The Blackbox Exporter allows Prometheus to monitor various endpoints (e.g., websites) by probing them.
   - The configuration involves setting up a `scrape_configs` section for the Blackbox Exporter.

4. **Configuration Details**:
   - **Job Name**: Each set of configuration rules is identified by a `job_name`. Here, it is set to `'blackbox'`.
   - **Metrics Path**: The `metrics_path` is set to `/probe`, which is the endpoint that Blackbox Exporter exposes for probing targets.
   - **Params**: The `params` section specifies that the module `http_2xx` should be used, meaning it will look for HTTP 200 responses (successful HTTP requests).
   - **Static Configurations**: The `static_configs` section lists the targets to probe. These targets can be various URLs that you want to monitor.
   - **Relabelling**: The `relabel_configs` section is used to modify the labels of the scraped metrics. This is necessary to pass the target URL to the Blackbox Exporter correctly.
   - **Replacement**: The `replacement` line sets the address of the Blackbox Exporter (`127.0.0.1:9115`), which is running on the local machine on port 9115.

5. **Operational Metrics**:
   - Another job named `blackbox_exporter` is set up to collect the operational metrics of the Blackbox Exporter itself.

### Summary

**Purpose**:
- The purpose of this stage is to configure Prometheus to use the Blackbox Exporter for probing and monitoring various endpoints. This configuration allows Prometheus to collect metrics on the availability and performance of these endpoints.

**Outcome**:
- After applying these changes to the `prometheus.yaml` file, Prometheus will be able to monitor the specified targets using the Blackbox Exporter. This will enable you to visualise the monitoring data in Grafana, ensuring that the endpoints you care about are up and running as expected.

**Why Necessary**:
- This configuration is necessary because it integrates the Blackbox Exporter with Prometheus, allowing you to monitor HTTP endpoints effectively. Without this configuration, Prometheus would not be able to use the Blackbox Exporter to probe the specified targets.

### Configuration Example

Here's the configuration you would add to your `prometheus.yaml` file:

```yaml
scrape_configs:
  - job_name: 'blackbox'
    metrics_path: /probe
    params:
      module: [http_2xx]  # Look for a HTTP 200 response.
    static_configs:
      - targets:
        - http://prometheus.io    # Target to probe with http.
        - https://prometheus.io   # Target to probe with https.
        - http://example.com:8080 # Target to probe with http on port 8080.
    relabel_configs:
      - source_labels: [__address__]
        target_label: __param_target
      - source_labels: [__param_target]
        target_label: instance
      - target_label: __address__
        replacement: 127.0.0.1:9115  # The blackbox exporter's real hostname:port.
  - job_name: 'blackbox_exporter'  # Collect blackbox exporter's operational metrics.
    static_configs:
      - targets: ['127.0.0.1:9115']
```

In summary, this stage configures Prometheus to use the Blackbox Exporter to monitor specified endpoints, ensuring you can collect and visualise data on their availability and performance.

### Steps to Locate and Edit the `prometheus.yaml` File

1. **Go to the Monitor VM Terminal:**
   - Open your terminal connected to the Monitor VM.

2. **List the Contents:**
   - Verify the directory contents to ensure everything is in place by running:
     ```sh
     ls
     ```
   - You should see the `prometheus-2.53.0.linux-amd64` folder.

3. **Change to the Directory:**
   - Navigate to the Prometheus directory:
     ```sh
     cd prometheus-2.53.0.linux-amd64
     ```

4. **Verify the Contents:**
   - List the contents of the directory to confirm the presence of the `prometheus.yaml` file:
     ```sh
     ls
     ```
   - You should see the `prometheus.yaml` file listed.

5. **Edit the `prometheus.yaml` File:**
   - Open the `prometheus.yaml` file in the `vi` editor:
     ```sh
     vi prometheus.yaml
     ```

6. **Scroll to the Bottom:**
   - Use the arrow keys to scroll down to the bottom of the file.

7. **Paste the Configuration:**
   - Copy and paste the following configuration at the bottom of the file. Make sure to copy the configuration from `- job_name: 'blackbox'` up to the `replacement: 127.0.0.1:9115` line:
     ```yaml
     - job_name: 'blackbox'
       metrics_path: /probe
       params:
         module: [http_2xx]  # Look for a HTTP 200 response.
       static_configs:
         - targets:
           - http://prometheus.io    # Target to probe with http.
           - https://prometheus.io   # Target to probe with https.
           - http://example.com:8080 # Target to probe with http on port 8080.
       relabel_configs:
         - source_labels: [__address__]
           target_label: __param_target
         - source_labels: [__param_target]
           target_label: instance
         - target_label: __address__
           replacement: 127.0.0.1:9115  # The blackbox exporter's real hostname:port.
     ```

8. **Update the IP Address:**
   - Change the `replacement: 127.0.0.1:9115` to the IP address where the Blackbox Exporter is running. For example, if the Blackbox Exporter IP address is `10.1.1.1`, update the line as shown below:
     ```yaml
     - job_name: 'blackbox'
       metrics_path: /probe
       params:
         module: [http_2xx]  # Look for a HTTP 200 response.
       static_configs:
         - targets:
           - http://prometheus.io    # Target to probe with http.
           - https://prometheus.io   # Target to probe with https.
           - http://example.com:8080 # Target to probe with http on port 8080.
       relabel_configs:
         - source_labels: [__address__]
           target_label: __param_target
         - source_labels: [__param_target]
           target_label: instance
         - target_label: __address__
           replacement: 10.1.1.1:9115  # The blackbox exporter's real hostname:port.
     ```

     ### Configuring Prometheus to Monitor Websites

**Objective:**
To configure Prometheus to monitor the Prometheus website and an application running on Slave-1 (WorkerNode-1).

**Steps:**

1. **Edit Prometheus Configuration File:**
   - Open the `prometheus.yaml` configuration file in a text editor.
   - Add the targets to the `static_configs` section.

   ```yaml
   static_configs:
     - targets:
       - http://prometheus.io  # Target to probe with http.
       - http://10.1.1.1:30119 # Target to probe with http on port 30119.
   ```

2. **Save the Configuration File:**
   - Save the file and exit the editor.
     ```bash
     Esc -> wq -> Enter
     ```

3. **Verify the Contents:**
   - List the contents of the directory to ensure the file is in place.
     ```bash
     ls
     ```

4. **Restart Prometheus:**

   a. **Get the Process ID:**
      - Find the process ID of Prometheus.
        ```bash
        pgrep Prometheus
        ```
      - For example, if the output is `2621`.

   b. **Kill the Process:**
      - Kill the Prometheus process using its process ID.
        ```bash
        kill 2621
        ```

   c. **Restart Prometheus:**
      - Restart Prometheus and run it in the background.
        ```bash
        ./Prometheus &
        ```

5. **Verify Prometheus Configuration:**

   a. **Refresh the Prometheus Page:**
      - Go to the browser where Prometheus is open and refresh the page.

   b. **Check Targets:**
      - Click on "Status" in the menu.
      - Choose "Targets" from the dropdown menu.

   c. **Verify Endpoint Status:**
      - On the Target page, ensure the state of both endpoints is "UP".
      - The labels should show the URLs of both endpoints set up in the `prometheus.yaml` file with job= 'blackbox'.

By following these steps, you will have successfully configured Prometheus to monitor the specified websites and verified that both endpoints are being monitored properly.

### Adding Prometheus as a Data Source in Grafana

**Step-by-Step Guide:**

1. **Add Prometheus as a Data Source:**
   - Open Grafana in your browser.
   - Click on the ">" next to "Connections" and from the list, select "Data Sources."
   - Click on "Add Data Source."
   - Select "Prometheus."
   - On the Prometheus configuration page, under the "Connection" section, provide the IP address and port number of Prometheus (e.g., `http://10.0.0.0:9090`).
   - Scroll down to the end of the page and click on "Save & Test."
   - Ensure the outcome message is: "Successfully queried the Prometheus API."

2. **Import the Blackbox Exporter Dashboard:**
   - Open a new tab in your browser.
   - Search for "blackbox grafana dashboard" and navigate to: https://grafana.com/grafana/dashboards/7587-prometheus-blackbox-exporter/
   - Scroll down to the "Get this dashboard" section.
   - Under "Import the dashboard template," click on "Copy ID to clipboard."
   - Return to the Grafana browser tab.
   - Click on the "+" icon in the top right corner and select "Import dashboard."
   - Paste the copied ID (7587) into the "Import via grafana.com" field and click "Load."
   - Scroll down to "signcl-prometheus" and select the data source, e.g., "prometheus default."
   - Click on "Import."
   - From the menu bar at the top, click on the time range selector (default "Last 5 minutes") and choose your preferred time range.

3. **Verify the Blackbox Exporter Dashboard:**
   - The dashboard should display monitoring information for your configured HTTP targets.
   - You can monitor and get results of your application status, HTTP duration, and HTTP probe duration.
   - You can adjust the scan/refresh interval from the top menu (e.g., set to 5s for more frequent updates).
   - In the "Enter variable value" box in front of "target," you can select specific targets/websites to monitor. In this case, `http://prometheus.io` and `http://10.0.0.1:30119`.

### System Level Monitoring with Node Exporter

1. **Download and Install Node Exporter:**
   - Go to https://prometheus.io/download/.
   - Scroll down to "node_exporter."
   - Copy the link address for the Linux tar file (e.g., `https://github.com/prometheus/node_exporter/releases/download/v1.8.1/node_exporter-1.8.1.linux-amd64.tar.gz`).

2. **Download and Extract the Package:**
   - Open your Jenkins VM terminal.
   - Download the node_exporter package:
     ```bash
     wget https://github.com/prometheus/node_exporter/releases/download/v1.8.1/node_exporter-1.8.1.linux-amd64.tar.gz
     ```
   - Extract the downloaded tar file:
     ```bash
     tar -xvf node_exporter-1.8.1.linux-amd64.tar.gz
     ```
   - Remove the tar file to clean up the directory:
     ```bash
     rm -rf node_exporter-1.8.1.linux-amd64.tar.gz
     ```
   - Change to the extracted directory:
     ```bash
     cd node_exporter-1.8.1.linux-amd64
     ```
   - List the contents to ensure everything is in place:
     ```bash
     ls
     ```

3. **Run the Node Exporter:**
   - Start the Node Exporter in the background:
     ```bash
     ./node_exporter &
     ```
   - Node Exporter runs on port `9100`.

4. **Verify Node Exporter:**
   - Copy the Public IPv4 address of your VM.
   - Open your browser and navigate to `http://<VM Public IPv4>:9100` (e.g., `http://10.1.1.2:9100`).
   - Verify that the Node Exporter is running successfully by checking the metrics page.

### Integrating Jenkins with Prometheus

1. **Install Prometheus Metrics Plugin for Jenkins:**
   - Open Jenkins in your browser.
   - Navigate to the Dashboard.
   - Click on "Manage Jenkins."
   - Select "Manage Plugins."
   - Go to the "Available Plugins" tab.
   - Search for "Prometheus" and select "Prometheus metrics."
   - Click "Install" (You may need to restart Jenkins for the plugin to take effect).

2. **Restart Jenkins:**
   - To restart Jenkins, append `/restart` to the Jenkins URL (e.g., `http://10.101.10.101:8080/restart`).
   - Confirm the restart when prompted.

3. **Configure Prometheus Plugin in Jenkins:**
   - Once Jenkins restarts, log in using your credentials.
   - Navigate to "Manage Jenkins."
   - Click on "Configure System."
   - Scroll down to find the "Prometheus" section.
   - Configure the plugin as needed. (see the screenshots below):

  <img width="590" alt="Pometheus_ 1" src="https://github.com/FahadMKhan/BoardgameListingWebApp/assets/97802721/99f18496-37a4-428e-88df-9c753822eaa6">

<img width="614" alt="Promotheus_2" src="https://github.com/FahadMKhan/BoardgameListingWebApp/assets/97802721/2360bc51-acb7-4997-abae-6e3d9fd8a2cb">
     

By following these steps, you will successfully integrate Prometheus with Grafana, set up monitoring for your websites and system using Node Exporter, and configure Jenkins to expose metrics to Prometheus for comprehensive system-level monitoring.

**==========>>>>>>>>>>>>>>>>>>>>>>> Start here**
**==========>>>>>>>>>>>>>>>>>>>>>>> Start here**













9. **Save and Exit `vi` Editor:**
   - Press `Esc` to exit insert mode.
   - Type `:wq` and press `Enter` to save the changes and exit the editor.

### Summary

**Purpose**:
- The purpose of this stage is to configure Prometheus to use the Blackbox Exporter for probing and monitoring various endpoints. This ensures Prometheus can collect metrics on the availability and performance of these endpoints.

**Outcome**:
- After completing these steps, Prometheus will be configured to use the Blackbox Exporter to monitor specified targets. This integration allows for effective monitoring and ensures the targets are operational as expected.
