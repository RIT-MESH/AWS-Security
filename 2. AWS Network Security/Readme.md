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


#### **2.1 Virtual Private Cloud (VPC) Security**
**Theory:** Amazon VPC allows users to create isolated cloud environments with security features like subnetting, security groups, and network ACLs.

**UI Steps:**
1. Open AWS Management Console → VPC Dashboard.
2. Click "Create VPC" and configure CIDR blocks and subnets.
3. Set up route tables and internet/NAT gateways.
4. Configure Security Groups and Network ACLs.

**CLI Steps:**
```sh
aws ec2 create-vpc --cidr-block 10.0.0.0/16
aws ec2 create-subnet --vpc-id vpc-xxxx --cidr-block 10.0.1.0/24
aws ec2 create-internet-gateway
aws ec2 attach-internet-gateway --vpc-id vpc-xxxx --internet-gateway-id igw-xxxx
```

---

#### **2.2 Security Groups and Network ACLs**
**Theory:** Security Groups act as firewalls at the instance level, while Network ACLs operate at the subnet level, allowing or denying traffic.

**UI Steps:**
1. Open AWS Console → VPC → Security Groups.
2. Click "Create Security Group" and define rules.
3. Navigate to Network ACLs and configure inbound/outbound rules.

**CLI Steps:**
```sh
aws ec2 create-security-group --group-name MySecurityGroup --description "SG for web server"
aws ec2 authorize-security-group-ingress --group-id sg-xxxx --protocol tcp --port 22 --cidr 0.0.0.0/0
```

---

#### **2.3 AWS PrivateLink and Interface VPC Endpoints**
**Theory:** PrivateLink enables private connectivity between VPCs and AWS services without exposing traffic to the internet.

**UI Steps:**
1. Navigate to VPC → Endpoints.
2. Click "Create Endpoint" and select the service.
3. Attach the endpoint to a VPC and security group.

**CLI Steps:**
```sh
aws ec2 create-vpc-endpoint --vpc-id vpc-xxxx --service-name com.amazonaws.us-east-1.s3 --vpc-endpoint-type Interface
```

---

#### **2.4 VPC Flow Logs**
**Theory:** VPC Flow Logs capture network traffic for security analysis and compliance monitoring.

**UI Steps:**
1. Navigate to VPC → Flow Logs.
2. Click "Create Flow Log" and select a log destination.
3. Enable logs for a specific VPC, subnet, or ENI.

**CLI Steps:**
```sh
aws ec2 create-flow-logs --resource-ids vpc-xxxx --resource-type VPC --traffic-type ALL --log-group-name my-flow-logs
```

---

#### **2.5 AWS Shield (Standard & Advanced)**
**Theory:** AWS Shield protects against DDoS attacks, with Shield Advanced offering proactive threat mitigation.

**UI Steps:**
1. Navigate to AWS Shield in the console.
2. Click "Enable AWS Shield Advanced" for protection.
3. Configure protections for specific resources.

**CLI Steps:**
```sh
aws shield create-protection --name MyWebAppProtection --resource-arn arn:aws:elasticloadbalancing:us-east-1:xxxx:loadbalancer/app/MyALB
```

---

#### **2.6 AWS WAF (Web Application Firewall)**
**Theory:** AWS WAF protects web applications from common security threats such as SQL injection and cross-site scripting (XSS).

**UI Steps:**
1. Navigate to AWS WAF in the console.
2. Create a new Web ACL.
3. Add rules and associate it with CloudFront or an ALB.

**CLI Steps:**
```sh
aws wafv2 create-web-acl --name MyWebACL --scope CLOUDFRONT --rules file://waf-rules.json
```

---

#### **2.7 AWS Network Firewall**
**Theory:** AWS Network Firewall provides managed network protection for VPC traffic.

**UI Steps:**
1. Navigate to AWS Network Firewall in the console.
2. Click "Create Firewall" and define the VPC and subnets.
3. Add firewall rules and configure logging.

**CLI Steps:**
```sh
aws network-firewall create-firewall --firewall-name MyFirewall --vpc-id vpc-xxxx --subnet-mappings SubnetId=subnet-xxxx
```

---

#### **2.8 AWS Direct Connect Security**
**Theory:** AWS Direct Connect provides a dedicated network connection between on-premises infrastructure and AWS, improving security and reducing latency.

**UI Steps:**
1. Navigate to AWS Direct Connect in the console.
2. Click "Create Connection" and select a location.
3. Configure VLAN tagging and BGP routing.

**CLI Steps:**
```sh
aws directconnect create-connection --location EqDC2 --bandwidth 1Gbps --connection-name MyDirectConnect
```

---

#### **2.9 AWS VPN Security Best Practices**
**Theory:** AWS VPN provides secure communication between on-premises networks and AWS using IPSec encryption.

**UI Steps:**
1. Navigate to AWS VPN in the console.
2. Create a new Site-to-Site VPN.
3. Configure the VPN connection and customer gateway.

**CLI Steps:**
```sh
aws ec2 create-vpn-connection --customer-gateway-id cgw-xxxx --type ipsec.1 --vpn-gateway-id vgw-xxxx
```

---

#### **2.10 AWS Global Accelerator Security**
**Theory:** AWS Global Accelerator improves application availability and security by directing traffic through AWS's global network.

**UI Steps:**
1. Navigate to AWS Global Accelerator in the console.
2. Click "Create Accelerator" and define endpoints.
3. Configure traffic routing policies.

**CLI Steps:**
```sh
aws globalaccelerator create-accelerator --name MyAccelerator
```


---

#### **2.11 VPC Peering and Security Considerations**
**Theory:** VPC Peering allows secure communication between VPCs while maintaining isolation. It does not support transitive peering and requires appropriate route table configurations.

**UI Steps:**
1. Navigate to AWS VPC → Peering Connections.
2. Click "Create Peering Connection" and define the requester and accepter VPCs.
3. Accept the peering request in the accepter VPC.
4. Update the route tables to allow communication.

**CLI Steps:**
```sh
aws ec2 create-vpc-peering-connection --vpc-id vpc-xxxx --peer-vpc-id vpc-yyyy
aws ec2 accept-vpc-peering-connection --vpc-peering-connection-id pcx-xxxx
aws ec2 create-route --route-table-id rtb-xxxx --destination-cidr-block 10.0.2.0/24 --vpc-peering-connection-id pcx-xxxx
```

---

#### **2.12 DNS Security with Route 53 Resolver**
**Theory:** AWS Route 53 Resolver provides DNS resolution and forwarding capabilities for hybrid cloud architectures. It ensures secure DNS traffic between AWS and on-premises environments.

**UI Steps:**
1. Navigate to AWS Route 53 → Resolver.
2. Click "Create Resolver Rule" and select "Forward" type.
3. Define the domain name and forwarding IP addresses.
4. Attach the rule to a VPC for DNS resolution.

**CLI Steps:**
```sh
aws route53resolver create-resolver-rule --rule-name MyResolverRule --domain-name example.com --rule-type FORWARD --resolver-endpoint-id rslvr-endpoint-xxxx --target-ip Ip=192.168.1.1
```

---

#### **2.13 AWS Traffic Mirroring for Security Analysis**
**Theory:** AWS Traffic Mirroring captures network traffic from EC2 instances for analysis, helping detect anomalies and threats.

**UI Steps:**
1. Navigate to AWS VPC → Traffic Mirroring.
2. Click "Create Traffic Mirror Session".
3. Select the source ENI and the target monitoring instance.
4. Define traffic filters and logging settings.

**CLI Steps:**
```sh
aws ec2 create-traffic-mirror-session --network-interface-id eni-xxxx --traffic-mirror-target-id tmt-xxxx --traffic-mirror-filter-id tmf-xxxx --session-number 1
```

---

#### **2.14 AWS Security Hub Integration with VPC**
**Theory:** AWS Security Hub aggregates security alerts and findings from AWS services such as GuardDuty, Inspector, and IAM Access Analyzer, helping enhance network security visibility.

**UI Steps:**
1. Navigate to AWS Security Hub.
2. Enable Security Hub and select security standards.
3. Integrate Security Hub with AWS services and partner tools.
4. Review and act on security findings.

**CLI Steps:**
```sh
aws securityhub enable-security-hub
aws securityhub get-findings
```

---

#### **2.15 Network Access Analyzer**
**Theory:** AWS Network Access Analyzer helps identify unintended network access paths that could pose security risks.

**UI Steps:**
1. Navigate to AWS EC2 → Network Access Analyzer.
2. Define an analysis scope and set access paths.
3. Run the analysis and review findings.

**CLI Steps:**
```sh
aws ec2 start-network-insights-analysis --network-insights-path-id nip-xxxx
```


