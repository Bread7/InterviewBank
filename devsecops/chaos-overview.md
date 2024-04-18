---
title: Chaos Engieering Overview
author: Zheng Jie
---

# Chaos Engieering Overview

## Principle of Chaos Engieering

Discipline of experimenting on system to build confidence in system's capability to withstand turbulent conditions in production.

Modern large-scale software systems are complex with many components and services functioning in a distributed system. Interactiosn between services can cause unpredictable outcomes that affect production environments.

Weaknesses in system need to be tested for improper fallback settings, unavailable services, outages from traffic overload, cascading failures from single point of failure and many more. Rigorous testing will measure stability of complex system in production deployment and areas to improve and deal with potential chaos.

## Practising Chaos

1. Define 'steady state' of measurable output of system indicating normal behaviour
2. Hypothesise steady state in control and experiment group
3. Introduce vairables of real world events like service failure, network overloading etc.
4. Disprove hypothesis by viewing difference in steady state between control and experiment group

The more difficult it is to disrupt the steady state, there are more confidence in the system's resilience.

## Benefits

* Improved system resilience and reliability
* Reduce revenue loss
* Develop in-depth understanding of system
* Improve failure recovery

## Challenges

* Risk of outages
* Resource limitation
* Requirement of robust monitoring systems

## Tools

{% embed url="https://github.com/Netflix/chaosmonkey" %}

{% embed url="https://www.gremlin.com/" %}

{% embed url="https://litmuschaos.io/" %}

{% embed url="https://chaos-mesh.org/" %}

## Interview Questions

* How would you test for resilience in a system?
* What is the difference between fault tolerance and resiliency?
* What are the differences between load testing and chaos engineering?

## Author

- [Zheng Jie](https://github.com/Bread7) 🍞

## References

1. [Principle of Chaos Engieering](http://principlesofchaos.org/)
2. [Splunk - Chaos Engineering](https://www.splunk.com/en_us/blog/learn/chaos-engineering.html)