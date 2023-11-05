**Internet Gateway**

An Internet Gateway is a horizontally scaled, redundant, and highly available VPC component that allows communication between instances in your VPC and the internet. It provides a target in your VPC route tables for internet-routable traffic and performs network address translation (NAT) for instances that have been assigned public IPv4 addresses.

Key Points:
- Serves as an access point for the internet to your VPC.
- Allows resources within your VPC (like EC2 instances) to access the internet.
- Supports IPv4 and IPv6 traffic.
- Is a necessary component for any AWS VPC that requires direct access to the internet.

Here is a simplified diagram showcasing an Internet Gateway:

```plaintext
[Internet]
     ||
[Internet Gateway]
     ||
   [VPC]
```

In the route table, it would look like this:

```plaintext
Destination     Target
0.0.0.0/0       igw-xxxxxx
```

The above signifies that any traffic destined for an IP address outside the VPC should be routed to the Internet Gateway.

**Interface Endpoint (AWS PrivateLink)**

An Interface Endpoint (also known as an AWS PrivateLink endpoint) is a construct for a private connection between your VPC and AWS services or your own services hosted in another VPC. It enables you to connect to services directly via the private IP space of your VPC without needing an Internet Gateway, NAT device, VPN connection, or AWS Direct Connect connection.

Key Points:
- Uses AWS PrivateLink technology.
- Provides private connectivity to AWS services, keeping traffic within the AWS network.
- Can be used to access AWS services (like S3, DynamoDB) or for services hosted by other AWS customers and partners.
- An endpoint service in one VPC can be privately accessed by endpoints in other VPCs.

Here is a simplified diagram of an Interface Endpoint:

```plaintext
      [Your VPC]
         ||
[Interface Endpoint]
         ||
  [AWS Service]
```

The routing for an Interface Endpoint is managed internally by AWS, and it is represented in the VPC route table as an entry pointing to the PrivateLink endpoint network interface.

**Comparison**

- **Scope of Connectivity**: An Internet Gateway is for broad internet access, while an Interface Endpoint is for private connectivity to specific AWS services.
- **Network Traffic**: Internet Gateways route traffic over the public internet, while Interface Endpoints keep traffic within the AWS network.
- **Cost**: There is no hourly charge for an Internet Gateway, but you do pay for the data transferred through it. Interface Endpoints have an hourly charge and a charge for data processed.
- **IP Addressing**: Internet Gateways work with public IP addresses, whereas Interface Endpoints enable access via private IP addresses within a VPC.

For specific and detailed references including diagrams, the AWS Documentation is the best resource. Here are some links where you can find more details:

- AWS Internet Gateways: [AWS Internet Gateway Documentation](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html)
- AWS PrivateLink and Interface Endpoints: [AWS PrivateLink Documentation](https://docs.aws.amazon.com/vpc/latest/userguide/endpoint-services-overview.html)

Please note, diagrams are not supported here, so you would need to refer to the links provided for visual representations.