TODO: Add images, add references to other related guides when completed

QoS parameters can be manipulated to achieve desired service qualities for different users or services. This document provides an overview of these parameters, their implications, and guides on how to adjust them within the Open5GS framework.

# **Quality of Service (QoS) Parameters in 5G Networks**

QoS in 5G networks is essential for ensuring that various applications and services receive the network resources they require to function optimally, according to their specific needs. Key QoS parameters that can be configured include:

1. **5G QoS Identifier (5QI)**
2. **Allocation and Retention Priority (ARP)**
3. **User Equipment Aggregate Maximum Bit Rate (UE AMBR)**

### **1. 5G QoS Identifier (5QI)**

- **Description**: 5QI is a scalar that maps to one QoS level, specifying the packet forwarding treatments, including priority, packet delay budgets (PDB), and packet error rates. This helps in differentiating between various traffic types (e.g., voice vs. data) and ensuring they're treated appropriately.
- **Configuration Options**: The values range from 1 to 255, with specific ranges designated for Guaranteed Bit Rate (GBR) services, non-GBR services, mission-critical services, etc.
- **Impact on Services**: Adjusting 5QI values can significantly impact the latency, reliability, and throughput of services. For example, VoIP services require low latency and high reliability, which can be achieved by assigning a 5QI value that prioritizes these characteristics.

### **2. Allocation and Retention Priority (ARP)**

- **Description**: ARP determines the priority of a bearer resource among others. It influences the decision in the allocation and retention of bearer resources during congestion or when resources are scarce.
- **Configuration Options**: ARP values range from 1 (highest priority) to 15 (lowest priority), including the pre-emption capability and vulnerability.
- **Impact on Services**: Services with higher ARP values have a higher chance of retention and allocation in congested networks, ensuring critical services remain uninterrupted.

### **3. User Equipment Aggregate Maximum Bit Rate (UE AMBR)**

- **Description**: UE AMBR defines the maximum allowed total bit rate across all Non-GBR bearers for a user, ensuring a fair distribution of network resources.
- **Configuration Options**: Specified in bits per second (bps), allowing administrators to set limits based on the user's subscription or service plan.
- **Impact on Services**: Influences the overall user experience by limiting the aggregate throughput available to a user's services, which is crucial for bandwidth management across multiple services.

## **Configuring QoS Parameters in Open5GS**

### **Prerequisites:**

- Ensure Open5GS and UERANSIM are correctly installed and configured.
- Familiarize yourself with the Open5GS WebUI for managing subscribers and QoS profiles.

### **Step-by-Step Guide:**

1. **Access the Open5GS WebUI:**
   - Navigate to the Open5GS WebUI through your web browser. The URL typically is `http://localhost:3000` (or http://localhost:9999 according to our guide).

2. **Modify Subscriber QoS Profiles:**
   - Go to the `Subscribers` section.
   - Select a subscriber to modify, or add a new subscriber.
   - Within the subscriber profile, scroll down to the QoS settings section.

3. **Adjusting 5QI:**
   - Locate the 5QI field.
   - Enter the desired 5QI value according to the service requirements of the subscriber. Refer to the 5G specifications for 5QI values corresponding to different service types.

4. **Setting ARP:**
   - Find the ARP settings.
   - Adjust the priority level, pre-emption capability, and vulnerability as required to ensure the service's resilience to network conditions and congestion.

5. **Configuring UE AMBR:**
   - In the UE AMBR section, specify the maximum bit rate for uplink and downlink. These values should align with the user's subscription plan and the network's capacity.

6. **Save Changes:**
   - After making the necessary adjustments, save the changes to the subscriber's profile.
   - Repeat the process for other subscribers as needed.

7. **Testing and Verification:**
   - Use UERANSIM to simulate UE behavior under different network conditions and verify that the QoS settings are correctly applied.
   - Monitor the performance and QoS treatment using Open5GS's logging and monitoring tools.


## **Example Scenario Setup**

1. **VoIP Service Subscriber (High Priority, Low Latency)**
   - **5QI**: For VoIP, a low latency and guaranteed bit rate are required, typically associated with a 5QI value of 1 (GBR, low latency).
   - **ARP**: High priority to ensure voice calls are not dropped during congestion. We'll use an ARP level of 1 with pre-emption capability and vulnerability set to not pre-emptable.
   - **UE AMBR**: Set to 128 Kbps to accommodate the VoIP bandwidth requirements.

2. **Web Browsing Subscriber (Standard Priority)**
   - **5QI**: Web browsing can tolerate slightly higher latencies and does not require a guaranteed bit rate, fitting a 5QI value of 9 (non-GBR, standard delay).
   - **ARP**: Standard priority. We'll use an ARP level of 9, with both pre-emption capability and vulnerability set to pre-emptable.
   - **UE AMBR**: Set to 5 Mbps to allow for a decent browsing experience without allocating excessive network resources.

### **VoIP Service Configuration**
1. Open Open5GS WebUI.
2. Navigate to the `Subscribers` section in the Open5GS WebUI.
3. Click on `Add Subscriber` to create a new subscriber or select an existing subscriber to modify.
4. For a new VoIP service subscriber:
   - Fill in the required subscriber details (IMSI, security context, etc.).
   - Scroll to the `Session-AMBR` and `QoS` sections.
   - **5QI**: Enter `1` for GBR with low latency.
   - **ARP Priority Level**: Enter `1`. Set `Pre-emption Capability` and `Pre-emption Vulnerability` as needed (not pre-emptable for high-priority services).
   - **UE AMBR (Uplink and Downlink)**: Set both to `128 Kbps`.
   - Click `Save`.

### **Web Browsing Configuration**

5. Follow the same steps to add or select another subscriber for web browsing:
   - **5QI**: Enter `9` for non-GBR with standard delay.
   - **ARP Priority Level**: Enter `9`, and set both `Pre-emption Capability` and `Pre-emption Vulnerability` to pre-emptable.
   - **UE AMBR (Uplink and Downlink)**: Set both to `5 Mbps`.
   - Click `Save`.

#### **Testing and Verification with UERANSIM**

6. To verify the configuration, simulate the UE using UERANSIM:
   - Modify the UERANSIM configuration files (`ue.yaml`) to match the IMSI of the subscribers you've configured.
   - Start the UERANSIM UE and gNB simulations.

7. Observe the behavior and performance:
   - For VoIP simulations, monitor latency and bitrate to ensure they meet the expected QoS.
   - For web browsing, ensure the data rate is capped at the configured AMBR and experiences appropriate treatment during network congestion.
   < TODO: Add more here about how to measure performace >


We have successfully configured two different QoS profiles for VoIP and web browsing services in Open5GS. These configurations demonstrate how to prioritize critical services like voice calls over standard data services, ensuring an optimal user experience even under varying network conditions.


