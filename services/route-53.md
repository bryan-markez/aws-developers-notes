# Route 53
A highly available, scalable, fully managed and authoritative DNS

## Records
Each record will contain:
- Domain/subdomain name (example.com)
- RecordType (A, AAAA, CNAME, NS)
- Value (ip addresses)
- Routing Policy (how Route 53 will respond to queries)
- TTL

A -> maps a hostname to IPv4  
AAAA -> maps a hostname to IPv6  
CNAME -> maps a hostname to another hostname  
NS -> Name Servers for the Hosted Zone and controls how traffic is routed for a domain

## Hosted Zones
A container for records that define how to route traffic to a domain and its subdomains

Public Hosted Zones  
contains records that specify how to route traffic on the Internet

Private Hosted Zones  
contains records that specify how you route traffic within one or more VPCs

Pay $0.50 per month per hosted zone

## TTL
High TTL, less traffic but possibly outdated records
Low TTL, more traffic which will cost more $ on Route 53

## CNAME vs Alias
CNAME will point a hostname to any other hostname and only for non-root domains.

Alias is a Route 53 specific feature that allows you to point a hostname to an AWS Resource (works for both root domain and non-root domain) (free of charge and native health checks)

Alias Record Targets
- ELB
- CloudFront
- API Gateway
- Elastic Beanstalk
- S3 Websites
- VPC Interface Endpoints
- Global Accelerator
- Route 53 record in the same hosted zone

NOTE: YOU CANNOT SET AN ALIAS RECORD FOR AN EC2 DNS NAME

## Health Checks
Note: mainly for public resources

Health checks can be created so that you can do automated DNS failover.  
1. can monitor health endpoint
2. monitor other health checks
3. monitor CloudWatch Alarms

Monitoring an Endpoint  
ex) /health
HC will hit the endpoint with ~15 Global HC to check the endpoint health.  
You can:
- set Healthy/Unhealthy threshold
- interval for how often the HC checks are
- generally <18% HC reports healthy, then the record is considered unhealthy
- HC will only pass 2XX/3XX response
- HC can check text based response in the first 5120 bytes
- Security Groups/Routing Tables have to be adjusted so that it allows requests from [Route 53 Health Checkers](https://ip-ranges.amazonaws.com/ip-ranges.json)

Calculated Health Checks  
You can combine the results of multiple Health Checks into a single Health Check  
Up to 256 Child HC

Health Checks in Private Hosted Zones  
Route 53 Health Checkers are outside VPC so they cannot access private endpoints  
Use CloudWatch Metrics instead


## Routing Policies
Helps Route 53 respond to DNS queries. 
### Simple
Routes traffic to a single resource

If multiple values are returned, then a random one is chosen by the client. (Not multiple AWS resource)

### Weighted
Control % of the requests that go to each specific resource. You can assign each record a relative weight

Possible use case: testing a new application version

### Failover
Mandatory health check, if health check fails then Route 53 will automatically failover to secondary records

### Latency based
Redirect to the resource that has the least latency close to us.

Latency is based on traffic between users and AWS Regions

### Geolocation
Specify location by a continent, country, or US State to route to.  
There's a "Default" record for this policy in case there are no matches.

This can be used to restrict content distribution, website localization, or in some case, load balancing.

### Geoproximity
Similar to Geolocation, route traffic to your resources based on the geographic location. The idea is that the traffic will get routed depending on the `bias` of the geographic region.

If the bias of the region is high, then it will have a larger area of influence when determining which users it should route to.

![Geoproximity Diagram Representation](/assets/geoproximitypolicy.png)

### Multi-Value Answer
Route 53 can return multiple values/resources, and up to 8 healthy records can be returned. MVA is not a substitute for having an LB, but it does allow the client to do their own load balancing somewhat.