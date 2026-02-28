# Configure AWS PrivateLink for Redshift [Enterprise +](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")

<!-- -->

Available to certain Enterprise tiers

The private connection feature is available on the following dbt Enterprise tiers:

* Business Critical
* Virtual Private

To learn more about these tiers, contact us at <sales@getdbt.com>.

AWS provides two different ways to create a PrivateLink VPC endpoint for a Redshift cluster that is running in another VPC:

* [Redshift-managed PrivateLink Endpoints](https://docs.aws.amazon.com/redshift/latest/mgmt/managing-cluster-cross-vpc.html)
* [Redshift Interface-type PrivateLink Endpoints](https://docs.aws.amazon.com/redshift/latest/mgmt/security-private-link.html)

dbt supports both types of endpoints, but there are several [considerations](https://docs.aws.amazon.com/redshift/latest/mgmt/managing-cluster-cross-vpc.html#managing-cluster-cross-vpc-considerations) to take into account when deciding which endpoint type to use. Redshift-managed provides a simpler setup with no additional cost, which might make it the preferred option for many, but may not be an option in all environments. Based on these criteria, determine which type is right for your system. Follow the instructions from the section below that corresponds to your chosen endpoint type.

Private connection endpoints can't connect across cloud providers (AWS, Azure, and GCP). For a private connection to work, both dbt and the server (like <!-- -->Redshift<!-- -->) must be hosted on the same cloud provider. For example, dbt hosted on AWS cannot connect to services hosted on Azure, and dbt hosted on Azure can’t connect to services hosted on GCP.

## Configuring Redshift-managed PrivateLink[​](#configuring-redshift-managed-privatelink "Direct link to Configuring Redshift-managed PrivateLink")

1. Locate the **Granted accounts** section of the Redshift configuration

   * **Standard Redshift**

     * On the running Redshift cluster, select the **Properties** tab.

     [![Redshift Properties tab](/img/docs/dbt-cloud/redshiftprivatelink1.png?v=2 "Redshift Properties tab")](#)Redshift Properties tab

   * **Redshift Serverless**

     * On the Redshift Serverless **Workgroup configuration** page.

2. In the **Granted accounts** section, click **Grant access**.

[![Redshift granted accounts](/img/docs/dbt-cloud/redshiftprivatelink2.png?v=2 "Redshift granted accounts")](#)Redshift granted accounts

3. Enter the AWS account ID: `346425330055` - *NOTE: This account ID only applies to dbt Multi-Tenant environments. For Virtual Private/Single-Tenant account IDs please contact [Support](https://docs.getdbt.com/community/resources/getting-help.md#dbt-cloud-support).*

4. Choose **Grant access to all VPCs** —or— (optional) contact [Support](https://docs.getdbt.com/community/resources/getting-help.md#dbt-cloud-support) for the appropriate regional VPC ID to designate in the **Grant access to specific VPCs** field.

[![Redshift grant access](/img/docs/dbt-cloud/redshiftprivatelink3.png?v=2 "Redshift grant access")](#)Redshift grant access

5. Add the required information to the following template, and submit your request to [dbt Support](https://docs.getdbt.com/community/resources/getting-help.md#dbt-cloud-support):

   * **Standard Redshift**

     ```text
     Subject: New Multi-Tenant PrivateLink Request
     - Type: Redshift-managed
     - Redshift cluster name:
     - Redshift cluster AWS account ID:
     - Redshift cluster AWS Region (for example, us-east-1, eu-west-2):
     - <Constant name="cloud" /> multi-tenant environment (US, EMEA, AU):
     ```

   * **Redshift Serverless**

     ```text
     Subject: New Multi-Tenant PrivateLink Request
     - Type: Redshift-managed - Serverless
     - Redshift workgroup name:
     - Redshift workgroup AWS account ID:
     - Redshift workgroup AWS Region (for example, us-east-1, eu-west-2):
     - <Constant name="cloud" /> multi-tenant environment (US, EMEA, AU):
     ```

<!-- -->

dbt Labs will work on your behalf to complete the private connection setup. Please allow 3-5 business days for this process to complete. Support will contact you when the endpoint is available.

## Configuring Redshift interface-type PrivateLink[​](#configuring-redshift-interface-type-privatelink "Direct link to Configuring Redshift interface-type PrivateLink")

### 1. Provision AWS resources[​](#1-provision-aws-resources "Direct link to 1. Provision AWS resources")

Creating an Interface VPC PrivateLink connection requires creating multiple AWS resources in the account containing the Redshift cluster:

* **Security Group** — If you are connecting to an existing Redshift cluster, this likely already exists, however, you may need to add or modify Security Group rules to accept traffic from the Network Load Balancer (NLB) created for this Endpoint Service.

* **Target Group** — The Target Group will be attached to the NLB to tell it where to route requests. There are various target types available for NLB Target Groups, but you will use the IP address type.

  * Target Type: **IP**

    * **Standard Redshift**

      * Use IP addresses from the Redshift cluster’s **Network Interfaces** whenever possible. While IPs listed in the **Node IP addresses** section will work, they are also more likely to change.

      [![Target type: IP address](/img/docs/dbt-cloud/redshiftprivatelink4.png?v=2 "Target type: IP address")](#)Target type: IP address

      * There will likely be only one Network Interface (NI) to start, but if the cluster fails over to another availability zone (AZ), a new NI will also be created for that AZ. The NI IP from the original AZ will still work, but the new NI IP can also be added to the Target Group. If adding additional IPs, note that the NLB will also need to add the corresponding AZ. Once created, the NI(s) should stay the same (This is our observation from testing, but AWS does not officially document it).

    * **Redshift Serverless**

      * To find the IP addresses for Redshift Serverless instance locate and copy the endpoint (only the URL listed before the port) in the Workgroup configuration section of the AWS console for the instance.

      [![Redshift Serverless endpoint](/img/docs/dbt-cloud/redshiftserverless.png?v=2 "Redshift Serverless endpoint")](#)Redshift Serverless endpoint

      * From a command line run the command `nslookup <endpoint>` using the endpoint found in the previous step and use the associated IP(s) for the Target Group.

  * Target Group protocol: **TCP**

* **Network Load Balancer (NLB)** — Requires creating a Listener that attaches to the newly created Target Group (port `5439`is the default)

  * **Scheme:** Internal
  * **IP address type:** IPv4
  * **Network mapping:** Choose the VPC that the VPC Endpoint Service and NLB are being deployed in, and choose subnets from at least two Availability Zones.
  * **Security Groups:** The Network Load Balancer (NLB) associated with the VPC endpoint service must either not have an associated security group, or the security group must have a rule that allows requests from the appropriate dbt **private CIDR(s)**. Note that *this is different* than the static public IPs listed on the dbt [Access, Regions, & IP addresses](https://docs.getdbt.com/docs/cloud/about-cloud/access-regions-ip-addresses.md) page. dbt Support can provide the correct private CIDR(s) upon request. If necessary, until you can refine the rule to the smaller CIDR provided by dbt, allow connectivity by temporarily adding an allow rule of `10.0.0.0/8`.
  * **Listeners:** Create one listener per target group that maps the appropriate incoming port to the corresponding target group ([details](https://docs.aws.amazon.com/elasticloadbalancing/latest/network/load-balancer-listeners.html)).

* **VPC Endpoint Service** — Attach to the newly created NLB.

  * Acceptance required (optional) — Requires you to [accept our connection request](https://docs.aws.amazon.com/vpc/latest/privatelink/configure-endpoint-service.html#accept-reject-connection-requests) after dbt creates the endpoint.

Cross-Zone Load Balancing

We highly recommend cross-zone load balancing for your NLB or Target Group; some connections may require it. Cross-zone load balancing may also [improve routing distribution and connection resiliency](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/how-elastic-load-balancing-works.html#cross-zone-load-balancing). Note that cross-zone connectivity may incur additional data transfer charges, though this should be minimal for requests from dbt.

* [Enabling cross-zone load balancing for a load balancer or target group](https://docs.aws.amazon.com/elasticloadbalancing/latest/network/edit-target-group-attributes.html#target-group-cross-zone)

### 2. Grant dbt AWS account access to the VPC endpoint service[​](#2-grant-dbt-aws-account-access-to-the-vpc-endpoint-service "Direct link to 2. Grant dbt AWS account access to the VPC endpoint service")

On the provisioned VPC endpoint service, click the **Allow principals** tab. Click **Allow principals** to grant access. Enter the ARN of the root user in the appropriate production AWS account and save your changes.

* Principal: `arn:aws:iam::346425330055:role/MTPL_Admin`

[![Enter ARN](/img/docs/dbt-cloud/privatelink-allow-principals.png?v=2 "Enter ARN")](#)Enter ARN

### 3. Obtain VPC endpoint service name[​](#3-obtain-vpc-endpoint-service-name "Direct link to 3. Obtain VPC endpoint service name")

Once the VPC Endpoint Service is provisioned, you can find the service name in the AWS console by navigating to **VPC** → **Endpoint Services** and selecting the appropriate endpoint service. You can copy the service name field value and include it in your communication to dbt support.

[![Get service name field value](/img/docs/dbt-cloud/privatelink-endpoint-service-name.png?v=2 "Get service name field value")](#)Get service name field value

### 4. Submit your request to dbt Support[​](#4-submit-your-request-to-dbt-support "Direct link to 4. Submit your request to dbt Support")

Add the required information to the template below and submit your request to [dbt Support](https://docs.getdbt.com/community/resources/getting-help.md#dbt-cloud-support):

```text
Subject: New Multi-Tenant PrivateLink Request
- Type: Redshift Interface-type
- VPC Endpoint Service Name:
- Redshift cluster AWS Region (for example, us-east-1, eu-west-2):
- dbt AWS multi-tenant environment (US, EMEA, AU):
```

dbt Labs will work on your behalf to complete the private connection setup. Please allow 3-5 business days for this process to complete. Support will contact you when the endpoint is available.

## Create connection in dbt[​](#create-connection-in-dbt "Direct link to Create connection in dbt")

Once dbt Support completes the configuration, you can start creating new connections using PrivateLink.

1. Navigate to **Settings** → **Create new project** → select **Redshift**.
2. You will see two radio buttons: **Public** and **Private**. Select **Private**.
3. Select the private endpoint from the dropdown (this automatically populates the hostname/account field).
4. Configure the remaining data platform details.
5. Test your connection and save it.

## Troubleshooting[​](#troubleshooting "Direct link to Troubleshooting")

If the PrivateLink endpoint has been provisioned and configured in dbt but connectivity is still failing, check the following in your networking setup to ensure requests and responses can be successfully routed between dbt and the backing service.

### Configuration[​](#configuration "Direct link to Configuration")

Start with the configuration:

 1. NLB Security Group

The Network Load Balancer (NLB) associated with the VPC Endpoint Service must either not have an associated Security Group or the Security Group must have a rule that allows requests from the appropriate dbt *private CIDR(s)*. Note that this differs from the static public IPs listed on the dbt Connection page. dbt Support can provide the correct private CIDR(s) upon request.

* **Note**\*: To test if this is the issue, temporarily adding an allow rule of `10.0.0.0/8` should allow connectivity until the rule can be refined to a smaller CIDR

 2. NLB Listener and Target Group

Check that there is a Listener connected to the NLB that matches the port that dbt is trying to connect to. This Listener must have a configured action to forward to a Target Group with targets that point to your backing service. At least one (but preferably all) of these targets must be **Healthy**. Unhealthy targets could suggest that the backing service is, in fact, unhealthy or that the service is protected by a Security Group that doesn't allow requests from the NLB.

 3. Cross-zone Load Balancing

Check that *Cross-zone load balancing* is enabled for your NLB (check the **Attributes** tab of the NLB in the AWS console). If this is disabled, and the zones that dbt is connected to are misaligned with the zones where the service is running, requests may not be able to be routed correctly. Enabling cross-zone load balancing will also make the connection more resilient in the case of a failover in a zone outage scenario. Cross-zone connectivity may incur additional data transfer charges, though this should be minimal for requests from dbt.

 4. Routing tables and ACLs

If all the above check out, it may be possible that requests are not routing correctly within the private network. This could be due to a misconfiguration in the VPCs routing tables or access control lists. Review these settings with your network administrator to ensure that requests can be routed from the VPC Endpoint Service to the backing service and that the response can be returned to the VPC Endpoint Service. One way to test this is to create a VPC endpoint in another VPC in your network to test that connectivity is working independent of dbt's connection.

### Monitoring[​](#monitoring "Direct link to Monitoring")

To help isolate connection issues over a PrivateLink connection from dbt, there are a few monitoring sources that can be used to verify request activity. Requests must first be sent to the endpoint to see anything in the monitoring. Contact dbt Support to understand when connection testing occurred or request new connection attempts. Use these times to correlate with activity in the following monitoring sources.

 VPC Endpoint Service Monitoring

In the AWS Console, navigate to VPC -> Endpoint Services. Select the Endpoint Service being tested and click the **Monitoring** tab. Update the time selection to include when test connection attempts were sent. If there is activity in the *New connections* and *Bytes processed* graphs, then requests have been received by the Endpoint Service, suggesting that the dbt endpoint is routing properly.

 NLB Monitoring

In the AWS Console, navigate to EC2 -> Load Balancers. Select the Network Load Balancer (NLB) being tested and click the **Monitoring** tab. Update the time selection to include when test connection attempts were sent. If there is activity in the *New flow count* and *Processed bytes* graphs, then requests have been received by the NLB from the Endpoint Service, suggesting the NLB Listener, Target Group, and Security Group are correctly configured.

 VPC Flow Logs

VPC Flow Logs can provide various helpful information for requests being routed through your VPCs, though they can sometimes be challenging to locate and interpret. Flow logs can be written to either S3 or CloudWatch Logs, so determine the availability of these logs for your VPC and query them accordingly. Flow logs record the Elastic Network Interface (ENI) ID, source and destination IP and port, and whether the request was accepted or rejected by the security group and/or network ACL. This can be useful in understanding if a request arrived at a certain network interface and whether that request was accepted, potentially illuminating overly restrictive rules. For more information on accessing and interpreting VPC Flow Logs, see the related [AWS documentation](https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs.html).

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
