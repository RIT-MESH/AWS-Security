## **2. AWS Network Security**

### **Table of Contents**
1. **2.1 Virtual Private Cloud (VPC) Security**
2. **2.2 Security Groups and Network ACLs**
3. **2.3 AWS PrivateLink and Interface VPC Endpoints**
4. **2.4 VPC Flow Logs**
5. **2.5 AWS Shield (Standard & Advanced)**
6. **2.6 AWS WAF (Web Application Firewall)**
7. **2.7 AWS Network Firewall**
8. **2.8 AWS Direct Connect Security**
9. **2.9 AWS VPN Security Best Practices**
10. **2.10 AWS Global Accelerator Security**
11. **2.11 VPC Peering and Security Considerations**
12. **2.12 DNS Security with Route 53 Resolver**
13. **2.13 AWS Traffic Mirroring for Security Analysis**
14. **2.14 AWS Security Hub Integration with VPC**
15. **2.15 Network Access Analyzer**

---

AWS provides a robust set of network security tools to protect your cloud environment. Here's a breakdown of the key features and how to use them:

- **Virtual Private Cloud (VPC) Security:** Create isolated networks with subnets and route tables. Use the [AWS Management Console](https://console.aws.amazon.com/vpc/) to set up, ensuring secure traffic flow.
- **Security Groups and Network ACLs:** Control traffic at instance and subnet levels. Configure rules via the [VPC dashboard](https://console.aws.amazon.com/vpc/) to allow or deny access.
- **AWS PrivateLink and Interface VPC Endpoints:** Enable private connectivity to AWS services without internet exposure, managed through the [VPC console](https://console.aws.amazon.com/vpc/).
- **VPC Flow Logs:** Monitor network traffic for analysis, set up in the [VPC dashboard](https://console.aws.amazon.com/vpc/) under "Flow Logs."
- **AWS Shield (Standard & Advanced):** Protect against DDoS attacks, with Standard free and Advanced offering enhanced features, accessible via the [AWS Shield console](https://console.aws.amazon.com/shield/).
- **AWS WAF (Web Application Firewall):** Secure web apps from exploits like SQL injection, configured in the [AWS WAF console](https://console.aws.amazon.com/wafv2/).
- **AWS Network Firewall:** Deploy managed firewalls for VPC traffic, set up via the [Network Firewall console](https://console.aws.amazon.com/network-firewall/).
- **AWS Direct Connect Security:** Use dedicated connections for secure on-premises to AWS links, managed in the [Direct Connect console](https://console.aws.amazon.com/directconnect/).
- **AWS VPN Security Best Practices:** Set up secure VPNs for on-premises connections, configured in the [VPC dashboard](https://console.aws.amazon.com/vpc/).
- **AWS Global Accelerator Security:** Improve app availability with global routing, managed in the [Global Accelerator console](https://console.aws.amazon.com/global-accelerator/).
- **VPC Peering and Security Considerations:** Connect VPCs securely, ensuring no transitive routing, set up in the [VPC dashboard](https://console.aws.amazon.com/vpc/).
- **DNS Security with Route 53 Resolver:** Manage DNS resolution securely, configured in the [Route 53 Resolver console](https://console.aws.amazon.com/route53resolver/).
- **AWS Traffic Mirroring for Security Analysis:** Mirror traffic for analysis, set up in the [VPC dashboard](https://console.aws.amazon.com/vpc/) under "Traffic Mirroring."
- **AWS Security Hub Integration with VPC:** Aggregate security findings, enabled in the [Security Hub console](https://console.aws.amazon.com/sec-hub/).
- **Network Access Analyzer:** Identify unintended access paths, managed in the [EC2 console](https://console.aws.amazon.com/ec2/) under "Network Access Analyzer."

An unexpected detail is that AWS Devops Guru, primarily for operational issues, can also detect security anomalies, adding an extra layer of protection.

---

#### Introduction
AWS network security is critical for protecting cloud resources from unauthorized access, data breaches, and denial-of-service attacks. This guide covers a range of services and best practices, from configuring Virtual Private Clouds (VPCs) to leveraging advanced tools like AWS WAF and Network Firewall, ensuring users can secure their network infrastructure effectively.

---
#### 1. Virtual Private Cloud (VPC) Security

**Theory Explanation:**
Amazon Virtual Private Cloud (VPC) is a logically isolated section of the AWS cloud where you can launch resources in a virtual network that you define. It provides complete control over your virtual networking environment, including selection of your own IP address range, creation of subnets, and configuration of route tables and network gateways. Security in VPC involves configuring these components to ensure that data is protected and that only authorized access is allowed, using features like security groups, network ACLs, and private subnets.

**UI Steps:**
1. Open the [AWS Management Console](https://console.aws.amazon.com/) and navigate to the VPC dashboard.
2. Click "Create VPC" and configure the CIDR block, instance tenancy, and other options as needed.
3. Create subnets within the VPC, specifying the subnet's CIDR block and availability zone.
4. Set up route tables to manage traffic within the VPC and to the internet.
5. Configure internet gateways or NAT gateways for internet access.
6. Manage security groups and network ACLs to control access to resources within the VPC.

**CLI Steps:**
```sh
# Create a new VPC
aws ec2 create-vpc --cidr-block 10.0.0.0/16

# Create a subnet within the VPC
aws ec2 create-subnet --vpc-id vpc-xxxxxxxx --cidr-block 10.0.1.0/24 --availability-zone us-east-1a

# Create an internet gateway
aws ec2 create-internet-gateway

# Attach the internet gateway to the VPC
aws ec2 attach-internet-gateway --vpc-id vpc-xxxxxxxx --internet-gateway-id igw-xxxxxxxx

# Create a route table
aws ec2 create-route-table --vpc-id vpc-xxxxxxxx

# Create a route to the internet gateway
aws ec2 create-route --route-table-id rtb-xxxxxxxx --destination-cidr-block 0.0.0.0/0 --gateway-id igw-xxxxxxxx

# Associate the route table with the subnet
aws ec2 associate-route-table --route-table-id rtb-xxxxxxxx --subnet-id subnet-xxxxxxxx
```
---
#### 2. Security Groups and Network ACLs

**Theory Explanation:**
Security Groups and Network Access Control Lists (ACLs) are two types of firewalls in Amazon Web Services (AWS). Security groups are instance-level firewalls that control inbound and outbound traffic at the instance level, while network ACLs are subnet-level firewalls that control traffic at the subnet level. Security groups are stateful, meaning they track the state of connections, whereas network ACLs are stateless and do not track connections, requiring explicit rules for both inbound and outbound traffic.

**UI Steps:**
1. Navigate to the [VPC dashboard](https://console.aws.amazon.com/vpc/) in the AWS Management Console.
2. For Security Groups:
   - Click "Security Groups" under "Security" section.
   - Click "Create security group" to define a new security group.
   - Configure inbound and outbound rules to allow or deny specific traffic.
3. For Network ACLs:
   - Click "Network ACLs" under "Security" section.
   - Click "Create network ACL" to create a new network ACL.
   - Configure inbound and outbound rules to allow or deny specific traffic based on IP addresses and ports.

**CLI Steps:**
```sh
# Create a new security group
aws ec2 create-security-group --group-name my-security-group --description "My security group"

# Authorize inbound traffic for the security group
aws ec2 authorize-security-group-ingress --group-id sg-xxxxxxxx --protocol tcp --port 22 --cidr 0.0.0.0/0

# Create a new network ACL
aws ec2 create-network-acl --vpc-id vpc-xxxxxxxx

# Create an inbound rule for the network ACL
aws ec2 create-network-acl-entry --network-acl-id acl-xxxxxxxx --rule-number 100 --protocol 6 --port-range From=22,To=22 --cidr-block 0.0.0.0/0 --rule-action allow --entry-type ingress

# Associate the network ACL with a subnet
aws ec2 associate-network-acl --network-acl-id acl-xxxxxxxx --subnet-id subnet-xxxxxxxx
```
---
#### 3. AWS PrivateLink and Interface VPC Endpoints

**Theory Explanation:**
AWS PrivateLink provides private connectivity between your VPC and supported AWS services, or between your VPC and another VPC, without traversing the public internet. It uses Elastic Network Interfaces (ENIs) to connect to the service, ensuring that traffic remains within the AWS network. Interface VPC Endpoints are a type of endpoint that uses PrivateLink to connect to supported services like S3, DynamoDB, and others, enhancing security by keeping traffic private.

**UI Steps:**
1. Navigate to the [VPC dashboard](https://console.aws.amazon.com/vpc/) in the AWS Management Console.
2. Under "Endpoints", click "Create Endpoint".
3. Select "Interface" as the endpoint type.
4. Choose the service you want to connect to (e.g., S3, DynamoDB).
5. Select the VPC and subnets where the endpoint will be created.
6. Configure any additional settings as required and create the endpoint.

**CLI Steps:**
```sh
# Create an interface endpoint for S3
aws ec2 create-vpc-endpoint --vpc-id vpc-xxxxxxxx --service-name com.amazonaws.us-east-1.s3 --vpc-endpoint-type Interface

# Describe the created endpoint
aws ec2 describe-vpc-endpoints --vpc-endpoint-ids vpce-xxxxxxxx
```
---
#### 4. VPC Flow Logs

**Theory Explanation:**
VPC Flow Logs capture information about the IP traffic going to and from network interfaces in your VPC. They help you monitor and analyze traffic patterns, detect anomalies, and comply with regulatory requirements. Flow logs can be sent to Amazon CloudWatch Logs for real-time monitoring or to an S3 bucket for long-term storage, providing insights into network security and performance.

**UI Steps:**
1. Navigate to the [VPC dashboard](https://console.aws.amazon.com/vpc/) in the AWS Management Console.
2. Select "Flow Logs" under "Monitoring & Logs".
3. Click "Create Flow Log".
4. Choose the resource (VPC, subnet, or network interface) for which you want to create flow logs.
5. Select the log destination (CloudWatch Logs or S3 bucket).
6. Configure any additional settings and create the flow log.

**CLI Steps:**
```sh
# Create a flow log for a VPC
aws ec2 create-flow-log --resource-id vpc-xxxxxxxx --resource-type VPC --traffic-type ALL --log-destination-type cloud-watchlogs --log-group-name my-flow-log-group

# Verify the flow log creation
aws ec2 describe-flow-log --flow-log-id fl-xxxxxxxx
```
---
#### 5. AWS Shield (Standard & Advanced)

**Theory Explanation:**
AWS Shield is a managed DDoS protection service that safeguards applications running on AWS. It comes in two tiers: Shield Standard, which is free and provides basic DDoS protection for all AWS customers, and Shield Advanced, which offers enhanced protection and additional features like 24/7 support for a fee. Shield Advanced is particularly useful for protecting against large-scale attacks and ensuring compliance with availability requirements.

**UI Steps:**
1. Navigate to the [AWS Shield console](https://console.aws.amazon.com/shield/) in the AWS Management Console.
2. For Shield Advanced, click "Enable AWS Shield Advanced" and follow the subscription process.
3. Configure protections for specific resources such as Elastic Load Balancers or CloudFront distributions.
4. Monitor and manage DDoS attacks and protections from the console.

**CLI Steps:**
```sh
# Create a protection for an Elastic Load Balancer
aws shield create-protection --name my-protection --resource-arn arn:aws:elasticloadbalancing:us-east-1:xxxxxxxx:loadbalancer/app/my-alb

# List all protections
aws shield list-protections
```
---
#### 6. AWS WAF (Web Application Firewall)

**Theory Explanation:**
AWS WAF is a web application firewall that helps protect web applications from common web exploits such as SQL injection and cross-site scripting (XSS). It allows you to create rules that block or allow traffic based on conditions you define, such as IP addresses, HTTP headers, or query strings. WAF can be used with Amazon CloudFront, Application Load Balancers, and API Gateway, enhancing web application security.

**UI Steps:**
1. Navigate to the [AWS WAF console](https://console.aws.amazon.com/wafv2/) in the AWS Management Console.
2. Click "Create web ACL" to create a new web ACL.
3. Define rules to allow or block traffic based on conditions like IP addresses, HTTP headers, or query strings.
4. Associate the web ACL with your CloudFront distribution, ALB, or API Gateway.

**CLI Steps:**
```sh
# Create a web ACL
aws wafv2 create-web-acl --name my-web-acl --scope CLOUDFRONT --default-action Allow --visibility-config SampledRequestsEnabled=true,CloudWatchMetricsEnabled=true,MetricName=MyWebACL

# Create a rule to block traffic from a specific IP
aws wafv2 create-rule --name block-ip-rule --scope CLOUDFRONT --statement "{\"IpAddressSetReferenceStatement\":{\"IpAddressSetArn\": \"arn:aws:wafv2:us-east-1:xxxxxxxx:ipset/ipset-xxxxxxxx\"},\"NotStatement\":{}}"

# Associate the rule with the web ACL
aws wafv2 associate-rule-with-web-acl --web-acl-arn arn:aws:wafv2:us-east-1:xxxxxxxx:global/webacl/my-web-acl --rule-to-add "{\"Name\": \"block-ip-rule\", \"Priority\": 1, \"Statement\": {\"IpAddressSetReferenceStatement\": {\"IpAddressSetArn\": \"arn:aws:wafv2:us-east-1:xxxxxxxx:ipset/ipset-xxxxxxxx\"}}, \"Action\": {\"Block\": {}}, \"VisibilityConfig\": {\"SampledRequestsEnabled\": true, \"CloudWatchMetricsEnabled\": true, \"MetricName\": \"BlockIPRule\"}}"
```
---
#### 7. AWS Network Firewall

**Theory Explanation:**
AWS Network Firewall is a managed service that makes it easy to deploy and operate network firewalls for your VPCs. It allows you to define firewall policies that can inspect traffic at the network level and apply rules to block or allow traffic based on various criteria, such as source and destination IP addresses, ports, and protocols. It integrates with VPC routing to provide centralized control over network traffic.

**UI Steps:**
1. Navigate to the [Network Firewall console](https://console.aws.amazon.com/network-firewall/) in the AWS Management Console.
2. Click "Create firewall" to create a new firewall.
3. Choose the VPC where you want to deploy the firewall.
4. Select or create a firewall policy that defines the rules for traffic inspection.
5. Configure any additional settings like logging and subnet mappings.
6. Deploy the firewall and monitor its status.

**CLI Steps:**
```sh
# Create a firewall policy
aws network-firewall create-firewall-policy --firewall-policy-name my-policy --firewall-policy "{\"RuleGroupReferences\": [ {\"Priority\": 1, \"RuleGroupArn\": \"arn:aws:network-firewall:us-east-1:xxxxxxxx:rule-group/rule-group-xxxxxxxx\"} ]}"

# Create a firewall
aws network-firewall create-firewall --firewall-name my-firewall --vpc-id vpc-xxxxxxxx --subnet-mappings SubnetId=subnet-xxxxxxxx --firewall-policy-arn arn:aws:network-firewall:us-east-1:xxxxxxxx:firewall-policy/my-policy
```
---
#### 8. AWS Direct Connect Security

**Theory Explanation:**
AWS Direct Connect is a service that provides a dedicated network connection from your on-premises data center to AWS. It offers a more consistent network experience and better security compared to using the public internet, by reducing reliance on the internet and providing a private, high-bandwidth connection. Security aspects include encrypting data in transit, managing access controls, and ensuring that only authorized traffic is allowed through the connection.

**UI Steps:**
1. Navigate to the [Direct Connect console](https://console.aws.amazon.com/directconnect/) in the AWS Management Console.
2. Click "Create connection" to start the process of setting up a new connection.
3. Choose the connection type (dedicated or hosted) and specify the bandwidth.
4. Provide the necessary details for the connection, such as the location and the customer's router IP address.
5. Set up virtual interfaces for different types of traffic (private or public).
6. Configure BGP settings and route filters to control traffic flow.

**CLI Steps:**
```sh
# Create a new Direct Connect connection
aws directconnect create-connection --location EqDC2 --bandwidth 1Gbps --connection-name my-connection

# Create a private virtual interface
aws directconnect create-private-virtual-interface --connection-id dxcon-xxxxxxxx --newPrivateVirtualInterface "{\"virtualInterfaceName\": \"my-vif\", \"vlan\": 4094, \"asin\": \"AS65000\", \"authKey\": \"abc123\", \"routeFilterPrefixes\": [ {\"cidr\": \"10.0.0.0/8\"} ]}"
```
---
#### 9. AWS VPN Security Best Practices

**Theory Explanation:**
AWS VPN allows you to set up a secure connection between your on-premises network and your VPC using IPSec. Security best practices include using strong encryption algorithms, secure key management, and proper configuration of routing and access controls to ensure that the VPN connection is resilient against attacks and complies with security standards.

**UI Steps:**
1. Navigate to the [VPC dashboard](https://console.aws.amazon.com/vpc/) in the AWS Management Console.
2. Under "Virtual Private Networks", click "Customer Gateways" and create a new customer gateway with your on-premises device's IP address.
3. Create a VPN gateway and attach it to your VPC.
4. Create a VPN connection between the customer gateway and the VPN gateway.
5. Configure the VPN connection with the appropriate IPSec settings.
6. Set up routing to direct traffic through the VPN connection.

**CLI Steps:**
```sh
# Create a customer gateway
aws ec2 create-customer-gateway --type ipsec.1 --ip-address 192.168.1.1

# Create a VPN gateway
aws ec2 create-vpn-gateway --type ipsec.1

# Attach the VPN gateway to the VPC
aws ec2 attach-vpn-gateway --vpc-id vpc-xxxxxxxx --vpn-gateway-id vgw-xxxxxxxx

# Create a VPN connection
aws ec2 create-vpn-connection --type ipsec.1 --customer-gateway-id cgw-xxxxxxxx --vpn-gateway-id vgw-xxxxxxxx

# Describe the VPN connection to get the configuration needed for your on-premises device
aws ec2 describe-vpn-connections --vpn-connection-id vpn-xxxxxxxx
```
---
#### 10. AWS Global Accelerator Security

**Theory Explanation:**
AWS Global Accelerator is a service that improves the availability and performance of your applications by using the AWS global network. It provides a static IP address that acts as a single entry point for your application, routing traffic to your application's endpoints in multiple Availability Zones or Regions. Security considerations include ensuring that the endpoints are secure and that traffic is properly encrypted, leveraging AWS's global network for enhanced protection.

**UI Steps:**
1. Navigate to the [Global Accelerator console](https://console.aws.amazon.com/global-accelerator/) in the AWS Management Console.
2. Click "Create accelerator" to create a new accelerator.
3. Configure the accelerator with a name, IP address type (IPv4 or IPv6), and DNS name.
4. Create endpoint groups and add your application's endpoints to them.
5. Define traffic routing policies to control how traffic is distributed among the endpoints.

**CLI Steps:**
```sh
# Create a new accelerator
aws globalaccelerator create-accelerator --name my-accelerator

# Create an endpoint group
aws globalaccelerator create-endpoint-group --endpoint-group-name my-endpoint-group --endpoint-group-region us-east-1 --health-check-port 80 --health-check-path "/"

# Add endpoints to the endpoint group
aws globalaccelerator update-endpoint-group --endpoint-group-arn arn:aws:globalaccelerator::xxxxxxxx:accelerator/xxxxxxxx/endpoint-group/xxxxxxxx --endpoints "EndpointId=endpoint-xxxxxxxx,Weight=1"

# Associate the endpoint group with the accelerator
aws globalaccelerator create-listener --accelerator-arn arn:aws:globalaccelerator::xxxxxxxx:accelerator/xxxxxxxx --listener-port 80 --protocol TCP --endpoint-group-arn arn:aws:globalaccelerator::xxxxxxxx:accelerator/xxxxxxxx/endpoint-group/xxxxxxxx
```
---
#### 11. VPC Peering and Security Considerations

**Theory Explanation:**
VPC peering allows you to connect two VPCs via their network interfaces, enabling private communication between them. Security considerations include understanding that peering does not support transitive routing, meaning that if two VPCs are peers, and each is also peering with a third VPC, traffic won't automatically flow between the first and the third VPC through the second. Additionally, you need to manage route tables to control traffic flow between peer VPCs, ensuring proper segmentation and access control.

**UI Steps:**
1. Navigate to the [VPC dashboard](https://console.aws.amazon.com/vpc/) in the AWS Management Console.
2. Under "Peering Connections", click "Create Peering Connection".
3. Select the requester and accepter VPCs.
4. The accepter must accept the peering connection request.
5. Update the route tables in both VPCs to add routes for the peer VPC's CIDR blocks.

**CLI Steps:**
```sh
# Create a peering connection
aws ec2 create-vpc-peering-connection --vpc-id vpc-xxxxxxxx --peer-vpc-id vpc-yyyyyyyy

# Accept the peering connection (from the accepter's account)
aws ec2 accept-vpc-peering-connection --vpc-peering-connection-id pcx-xxxxxxxx

# Update route table to add a route to the peer VPC
aws ec2 create-route --route-table-id rtb-xxxxxxxx --destination-cidr-block 10.1.0.0/16 --vpc-peering-connection-id pcx-xxxxxxxx
```
---
#### 12. DNS Security with Route 53 Resolver

**Theory Explanation:**
Route 53 Resolver is a service that helps you manage DNS resolution between your on-premises network and your VPCs. It allows you to forward DNS queries from your VPC to your on-premises DNS servers or other DNS resolvers, ensuring that DNS traffic is handled securely within your network, reducing the risk of DNS-based attacks and ensuring compliance with security policies.

**UI Steps:**
1. Navigate to the [Route 53 Resolver console](https://console.aws.amazon.com/route53resolver/) in the AWS Management Console.
2. Click "Create resolver endpoint" to create a new resolver endpoint.
3. Choose the type of endpoint (inbound or outbound).
4. Configure the IP addresses for the endpoint.
5. Create resolver rules to define how DNS queries are handled.

**CLI Steps:**
```sh
# Create a resolver endpoint
aws route53resolver create-resolver-endpoint --name my-resolver-endpoint --direction OUTBOUND --ip-addresses "SubnetId=subnet-xxxxxxxx,Ip=10.0.1.100"

# Create a resolver rule
aws route53resolver create-resolver-rule --name my-resolver-rule --rule-type FORWARD --target-ips "Ip=192.168.1.1" --domain-name example.com
```
---
#### 13. AWS Traffic Mirroring for Security Analysis

**Theory Explanation:**
Traffic Mirroring allows you to copy network traffic from an elastic network interface (ENI) to a network analyzer for inspection. This is useful for security analysis, such as detecting anomalies or threats, by capturing and analyzing traffic patterns, enabling deep packet inspection and compliance monitoring.

**UI Steps:**
1. Navigate to the [VPC dashboard](https://console.aws.amazon.com/vpc/) in the AWS Management Console.
2. Under "Traffic Mirroring", click "Create traffic mirror session".
3. Select the source ENI from which to mirror traffic.
4. Choose the traffic mirror target, which can be another ENI or a network load balancer.
5. Define traffic filters to specify which traffic to mirror.
6. Configure the session number and other settings as needed.

**CLI Steps:**
```sh
# Create a traffic mirror filter
aws ec2 create-traffic-mirror-filter --description "My filter"

# Create a traffic mirror filter rule
aws ec2 create-traffic-mirror-filter-rule --traffic-mirror-filter-id tmf-xxxxxxxx --source-cidr-block 10.0.0.0/24 --destination-cid-block 10.0.1.0/24 --protocol 6 --destination-port-range From=80,To=80

# Create a traffic mirror session
aws ec2 create-traffic-mirror-session --name my-session --network-interface-id eni-xxxxxxxx --traffic-mirror-target-id tmt-xxxxxxxx --traffic-mirror-filter-id tmf-xxxxxxxx --session-number 1
```
---
#### 14. AWS Security Hub Integration with VPC

**Theory Explanation:**
AWS Security Hub is a service that provides a comprehensive view of your security state across your AWS environment. It aggregates security alerts and findings from various AWS services, including those related to networking like GuardDuty, Inspector, and IAM Access Analyzer. This integration helps in enhancing network security visibility and managing security at scale, providing a centralized dashboard for monitoring and remediation.

**UI Steps:**
1. Navigate to the [Security Hub console](https://console.aws.amazon.com/sec-hub/) in the AWS Management Console.
2. Enable Security Hub for your account.
3. Select the security standards you want to enable.
4. Integrate Security Hub with other services like GuardDuty, Inspector, etc., to collect findings.
5. Review and act on security findings from the Security Hub dashboard.

**CLI Steps:**
```sh
# Enable Security Hub
aws securityhub enable-security-hub

# Get findings from Security Hub
aws securityhub get-findings
```
---
#### 15. Network Access Analyzer

**Theory Explanation:**
Network Access Analyzer is a service that helps you understand and improve the security of your network by identifying unintended access paths that could pose security risks. It allows you to analyze your network configuration to find if there are any paths that allow access to or from resources in ways that might not be intended, enhancing network segmentation and access control.

**UI Steps:**
1. Navigate to the [EC2 console](https://console.aws.amazon.com/ec2/) in the AWS Management Console.
2. Under "Network Access Analyzer", click "Create analysis scope".
3. Define the scope of the analysis, including the VPCs and accounts to analyze.
4. Run an analysis to identify any security issues.
5. Review the findings and take corrective actions as needed.

**CLI Steps:**
```sh
# Create an analysis scope
aws ec2 create-network-insights-analysis-scope --description "My analysis scope" --tag-specifications "ResourceType=network-insights-analysis-scope,Tags=[{Key=Name,Value=MyScope}]"

# Start an analysis
aws ec2 start-network-insights-analysis --network-insights-path-id path-xxxxxxxx

# Describe the analysis results
aws ec2 describe-network-insights-analyses --network-insights-analysis-ids analysis-xxxxxxxx
```

#### Tables for Reference
| **Service**                     | **Primary Use Case**                     | **Key Feature**                          |
|----------------------------------|------------------------------------------|------------------------------------------|
| VPC                              | Isolated Network Environment             | Subnets, Route Tables, Security Groups   |
| Security Groups and Network ACLs | Traffic Control                          | Instance/Subnet Level Firewalls          |
| AWS PrivateLink                  | Private Service Connectivity             | Interface Endpoints, No Public Internet  |
| VPC Flow Logs                    | Traffic Monitoring                       | Logs to CloudWatch or S3                 |
| AWS Shield                       | DDoS Protection                          | Standard and Advanced Levels             |
| AWS WAF                          | Web App Security                         | SQL Injection, XSS Protection            |
| AWS Network Firewall             | Managed Network Protection               | VPC Traffic Inspection                   |
| AWS Direct Connect               | Dedicated Connectivity                   | Private, High Bandwidth                  |
| AWS VPN                          | Secure On-Premises Connection            | IPSec, Encryption                        |
| AWS Global Accelerator           | Global Traffic Routing                   | Static IP, Multi-Region Endpoints        |
| VPC Peering                      | VPC Interconnectivity                    | Private Communication, No Transitive     |
| Route 53 Resolver                | DNS Resolution                           | Hybrid Cloud DNS, Forwarding             |
| AWS Traffic Mirroring            | Security Analysis                        | Traffic Copy, Deep Packet Inspection     |
| AWS Security Hub                 | Security Aggregation                     | Findings from Multiple Services          |
|
