- A central-purpose aggregating gateway sits between external user and multiple downstream microservices and performs call filtering and aggregation for all user interfaces.
- Without aggregation, an user interface may have to make multiple calls to fetch required information, often throwing away data was retrieved but not needed.
- With an aggregating gateway, we can issue a single call from the user interface to the gateway. The aggregating gateway then carries out all the required calls, combines the results into a single response, and throws away any data that the user interface doesn't need. => *Aggregation/Orchestration problem*^07a768

# Problem
- As more user interfaces make use of the central gateway, it becomes a central bottleneck.
- Also, as the logic grows, who will owns the gateway?
	- If multiple team share the responsibility own owning the gateway, making changes will require coordination and slow down everyone.
- Another problem, for the same kind of user interfaces (for example, a User Account Dashboard), we may also want to split the logic for Desktop and Mobile devices => Bloated & duplicate logic & increase risk of bottleneck.