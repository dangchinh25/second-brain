# Infrastructure
- **Terminologies**:
	- **Region**: a cluster of data centers, contains many (2-6) AZ
	- **Availability zone**: one or more discrete data center, isolated from disasters, connected with high bandwidth + ultra low latency networking
	- **Local zone**: run a few specific AWS services closer to user populations where no AWS regions exist
	- **Data center**
	- **Edge Location**: reduce latency of content delivery
	- **Regional Edge Cache**:
		- Between the main servers and the edge location
		- App first seek content from edge location -> if not found seek for Regional Edge Cache -> if not found seek for main server
# Security and Compliance
- AWS: responsible for the security **OF** the cloud
- Customer: responsible for the security **IN** the cloud
**![](https://lh5.googleusercontent.com/y9fH4700nsrwASzrqZoEN2zADiN3mKnHNLOazjV6mo7pNHAOPVxVw5Bov4jabXb_8h59SfxRf361iFgVXXdYBM_sggsLeZ1hcXr2Cx-W8OnpXax8kSMGlxURpr3_TSjQ0scXI0bIrv_HG0bNXQ)**
![](https://i.imgur.com/VaVcJqe.png)
