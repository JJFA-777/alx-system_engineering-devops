Postmortem: The Great Power Fluctuation Fiasco of '25 

Issue Summary

Duration: 3 hours and 10 minutes (2025-04-07, 14:45 GMT - 2025-04-07, 17:55 GMT)
Impact: Intermittent disruptions to our mobile money transaction processing service, "SwiftCash." Approximately 70% of users attempting to send or receive funds experienced failed transactions or significant delays. Other app features, like balance checks, were less affected. The digital cedi flow experienced a rather unwelcome hiccup.
Root Cause: Unexpected and severe power fluctuations across our primary data center's grid, triggering multiple, rapid failovers and causing instability in the transaction processing database connections. Our UPS systems struggled to cope with the sustained volatility.

Timeline

14:45 GMT - Automated alerts fire: Significant increase in transaction failures and latency on the SwiftCash processing endpoints. Initial thought: network connectivity issues with telco providers.
14:50 GMT - Customer support lines overwhelmed with users reporting failed mobile money transfers. Social media starts buzzing with #SwiftCashDown.
15:00 GMT - On-call infrastructure engineer checks network connectivity and reports no major issues. Attention shifts to the data center's power status.
15:15 GMT - Data center team confirms multiple reports of unstable power supply and voltage fluctuations from the national grid. UPS systems are cycling frequently.
15:30 GMT - Database team investigates intermittent connection losses and transaction rollbacks. Correlation with power fluctuation events becomes apparent. Assumption: database corruption due to abrupt shutdowns.
16:00 GMT - Attempts to stabilize database connections prove challenging due to the continued erratic power supply. Decision made to temporarily throttle transaction processing to prevent further data inconsistencies.
16:45 GMT - Power supply stabilizes. Data center team reports grid conditions returning to normal.
17:15 GMT - Transaction processing service gradually brought back online with increased monitoring.
17:55 GMT - All SwiftCash functionalities fully restored. Transaction backlog cleared. Postmortem initiated (while keeping an ear out for the familiar hum of the generator).

Root Cause and Resolution

What Happened:

As is sometimes the unpredictable nature of things here, our primary data center experienced a series of significant and rapid power fluctuations from the national grid. These weren't your garden-variety dips; they were more like electrical roller coaster rides. While our Uninterruptible Power Supply (UPS) systems are designed to handle brief outages, the sustained and erratic nature of these fluctuations overwhelmed their capacity to provide consistent power. This instability directly impacted our critical transaction processing database for SwiftCash, causing intermittent connection losses, transaction failures, and delays as the system struggled to maintain a stable state through repeated failovers. It was like trying to conduct a delicate surgery during an earthquake.

How We Fixed It:

Once the data center confirmed the grid power issues, our immediate focus shifted to mitigating the impact on the database. We initially considered a full database failover to our secondary site, but the continuous power instability made this risky. Instead, we opted to temporarily throttle the transaction processing rate to prevent further data corruption and give the systems a chance to recover during the power fluctuations. Once the power supply stabilized, we gradually brought the SwiftCash service back online, closely monitoring transaction success rates and database health. We also manually reviewed logs to ensure no transactions were permanently lost or corrupted.

Corrective and Preventative Measures

Improvements Needed:

Our reliance on the stability of the national grid for primary power is a known vulnerability. We need to explore more robust power redundancy solutions and improve our systems' resilience to power instability. Better monitoring of UPS performance under sustained load is also crucial.

TODOs:

[TODO] Conduct a thorough assessment of our current UPS infrastructure and identify potential upgrades or redundancies needed for prolonged and fluctuating power conditions.
[TODO] Investigate and implement a more seamless and automated failover mechanism to our secondary data center in the event of prolonged primary power issues.
[TODO] Enhance monitoring of power input and UPS status at the data center level, with proactive alerts for sustained instability.
[TODO] Explore options for on-site power generation (e.g., more robust generators with larger fuel reserves) as a supplementary power source during grid outages.
[TODO] Review and optimize our database configuration for better resilience to abrupt connection losses and rapid failovers.
[TODO] Implement application-level retry mechanisms with exponential backoff for transaction processing to gracefully handle temporary database unavailability.

Bonus Visual: Our Data Center's Power Situation

National Grid  -->  Data Center  -->  SwiftCash Database 
                       |
                       +-->  UPS (Feeling Overworked) 

TL;DR: Our mobile money service took an unscheduled break thanks to some unpredictable dumsor affecting our data center. We managed to bring things back online, but this experience has highlighted the need for us to be even more prepared for the realities of our local infrastructure. Time to double down on power resilience so our users can keep sending and receiving their cedis without interruption!