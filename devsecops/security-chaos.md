---
title: Security Focused Chaos Engieering
author: Zheng Jie
---

# Security Focused Chaos Engieering

## Principle of Chaos Engieering


## Practising Chaos



## Steps to Perform

As mentioned in the [overview](chaos-overview.md#practising-chaos), the theory will now be implemented for practical application.

1. Identify areas within the system to test
   * Rather than testing the whole system at once, test in parts for easier management to better understand clusters of services that eventually adds up to the entire system
   * Only implment on the whole system once there is enough confidence on the in-depth understanding of the system
   * Looking at areas could develop more niche edge cases that creates faults 
2. Ask questions about potential technical gaps to form baseline of chaos engineering experiments
   * Important to define the objectives of experiment to minimise impact radius of other services outside the scope of experiment
   * Need to identify services within testing area that have large sample size to collect essential data
   * Can rely on threat modeling methods and other frameworks for identifying attack vectors 
3. Detect vulnerable issues within the area
   * MITRE ATT&CK lists of tactics and procedures could be replicated
   * Random fault injection to servers can test for unexpected faults and recovery automation (if any)
4. Observe, measure and log the statistical data for further improvements
   * Data obtained from monitoring the chaos testing creates a baseline of how resilient the system currently is
   * Faulty areas could be improved to prevent scenarios playing out in prodcution
   * Other areas can benefit as similar issues can reuse the same solution


## Practical Usage

Netflix spearheaded Chaos Engineering as a way to fully test their complex system. However, most organisations cannot compete with Netflix's budget and methodology of testing in production environment (business revenue issues) therefore, a separate environment before production should be performed upon.

Even testing in non-production environments can still expose areas of faults and poor resilience within the current infrastructure, which can be improved and promote greater resilience when deploying into production environment. Once these gaps are filled, it is also important to validate the system again and perform real-time monitoring on the system to watch for unexpected changes within the system.


## Best Practices

### Integration into DevOps pipelines

Allows developement teams to automate resilience and fault tolerance testing at various stages. Weak areas could be identified earlier to be rectify, reducing time at future development stages. Continuous integration can be observed due to how code changes impacts on stability of system.

### Integration with Recovery Plans

Working with incident response team to utilise recovery plans and automate the process after system failure will show the effectiveness of such plans. Weaknesses in recovery plans can be improved upon if the recovery timings are not optimal/efficient enough.

### Pre-launch Checklist

Minimise risks of faulty experimentations which induces unnecessary development time. A checklist ensures that the required applications and its configurations are in place and optimal for chaos engineering testing to ensure accurate experimental results, and not due to careless development mistakes.

## Interview Questions

* What are the constraints faced when perform chaos testing on production environment?
* What are the best practices done to ensure effective chaos testing?
* How do you perform chaos engineering safely without crashing the entire system?

## Author

- [Zheng Jie](https://github.com/Bread7) 🍞

## References

1. [CSA - Security Chaos Engineering](https://cloudsecurityalliance.org/blog/2024/02/01/security-chaos-engineering-fewer-blind-spots-and-improved-stress-testing-move-cisos-closer-to-cyber-resilience)
2. [Maddevs - Chaos Engineering](https://maddevs.io/blog/chaos-engineering/#haos-engineering-tools)
3. [Datadog - Security Chaos Engineering for the cloud](https://www.datadoghq.com/blog/chaos-engineering-for-security/#next-steps-for-security-focused-chaos-engineering)