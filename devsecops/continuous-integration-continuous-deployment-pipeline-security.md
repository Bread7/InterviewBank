---
description: >-
  CI/CD, a cornerstone in modern software development, focuses on continuous
  integration of code and continuous delivery of updates. We'll discuss more on
  pipeline security
---

# Continuous Integration/Continuous Deployment Pipeline Security

**CI/CD and Pipeline Security**

A continuous integration and continuous delivery/deployment (CI/CD) pipeline is a series of steps that software delivery undergoes from code creation to deployment. Foundational to [DevOps](https://www.paloaltonetworks.com/cyberpedia/what-is-devops), CI/CD streamlines application development through automation of repetitive tasks, which enables early bug detection, reduces manual errors, and accelerates software delivery.

<figure><img src="../.gitbook/assets/image (7) (1).png" alt=""><figcaption></figcaption></figure>

[https://www.paloaltonetworks.com/cyberpedia/what-is-the-ci-cd-pipeline-and-ci-cd-security](https://www.paloaltonetworks.com/cyberpedia/what-is-the-ci-cd-pipeline-and-ci-cd-security)\
\
There are 4 phases to a Pipeline:\\

<figure><img src="../.gitbook/assets/image (8).png" alt=""><figcaption><p><a href="https://allcloud.io/wp-content/uploads/2017/08/sourcebuild.jpg">https://allcloud.io/wp-content/uploads/2017/08/sourcebuild.jpg</a></p></figcaption></figure>

1.  **Source Phase**:\
    The source stage involves the version control system where developers commit their code changes. The CI/CD pipeline monitors the repository and triggers the next stage when a new commit is detected. Git, Mercurial, and Subversion are popular version control systems. **(Standard Chartered was migrating from Bitbucket to Azure when i was workin there)**\
    \-------------------------------------------------------------------------------------------------

    * Threats: Source code repositories are prime targets. Malicious modifications can lead to compromised software.
    * Mitigations: Use **signed commits** for traceability. Perform **static code analysis** to detect malicious code.

    Code Example: Signing Commits

`Generate a GPG key`

`gpg --full-generate-key`

`Set GPG signing key in Git`

`git config --global user.signingkey YOUR_GPG_KEY_ID`

`Sign commits by default`

`git config --global commit.gpgsign true`

One of the main functions of the CI/CD pipeline is to test code before deployment to production. This includes security testing, designed to identify vulnerabilities in the code before they are exposed to potential exploitation.\\

2. **Build Phase**:\
   During the build stage, the CI/CD pipeline compiles the source code and creates executable artifacts. The build stage may also involve packaging the code into a Docker container or another format suitable for deployment. The build process should be repeatable and consistent to provide reliability.\
   \--------------------------------------------------------------------------------------------------

* Threats: The build server, if compromised, can be used to inject malicious code into the software.
* Mitigations: Isolate build environments. Regularly scan for vulnerabilities in the build tools and environments.

**Code Example: Secure Build Environment**

`#Dockerfile for building in an isolated environment`

`FROM node:14 AS build WORKDIR /usr/src/app COPY package*.json ./ RUN npm install COPY . . RUN npm run build`\\

Code within a CI/CD pipeline must have access to certain data and resources to build a functioning image for testing. Pipeline access controls limit pipelines’ access to only what is needed for their roles, minimizing the potential impacts if malicious code is executed within the pipeline.

3. **Automated Testing Phase**:\
   The test phase of the CI/CD pipeline involves running a series of automated tests on the built artifacts. Tests can include unit tests, integration tests, and end-to-end tests. Test automation is crucial at this stage to quickly identify and fix issues.\
   \--------------------------------------------------------------------------------------------------

* Threats: Insufficient testing can miss security flaws, while overly detailed test results can leak sensitive information if exposed.
* Mitigations: Integrate security testing, such as Dynamic Application Security Testing (DAST), and guard test data.

**Code Example: Dynamic Security Testing in Jenkins**

`pipeline { agent any stages { stage('DAST') { steps { script { sh 'docker run -t owasp/zap2docker-stable zap-baseline.py -t http://your-example-application/' } } } } }`

Applications may require access to various types of confidential information, such as passwords and API keys. As a result, these secrets must be accessible within CI/CD pipelines for testing. If these secrets are exposed in the CI/CD pipeline or DevOps environments, then they may allow an attacker to steal data, access corporate systems, or add malicious functionality to applications.

**PS. What is Jenkins?**\
**It is an open-source automation server, widely used in the realm of software development for its robust capability to facilitate Continuous Integration and Continuous Delivery (CI/CD). In simpler terms, Jenkins automates the various stages of the software development process, particularly those involving building, testing, and deploying software.**

4. **Deployment Phase**:\
   The deployment stage is the final stage of the CI/CD pipeline. With a continuous delivery setup, the deploy stage prepares the release for manual deployment. In a continuous deployment setup, the pipeline automatically deploys the release to the production environment.\
   \--------------------------------------------------------------------------------------------------

* Threats: Automated deployment tools, if compromised, can deploy malicious updates to production systems.
* Mitigations: Use canary releases and blue/green deployment to detect issues before full deployment.

Since nearly all applications rely upon third-party code to implement various functions. If these third-party libraries contain vulnerabilities or backdoors, this could open applications using the libraries up to exploitation by an attacker.

**PS. What is a Canary Release and what is blue/green deployment?**

A canary release is a strategy where a new version of an application is rolled out to a small subset of users before it is made available to the entire user base. This approach is named after the "canary in a coal mine" concept, where miners used to take a canary with them as an early warning sign for toxic gases.\
\--------------------------------------------------------------------------------------------------------

**Blue/green deployment** is a method of releasing applications by shifting traffic between two identical environments that only differ in the version of the application they are running.

**How It Works:**

Blue and Green Environments: There are two production environments (Blue - current version, Green - new version).\
\
Deployment: The new version of the application is deployed to the Green environment.\
\
Testing: The Green environment is tested thoroughly. This environment is identical to Blue, ensuring realistic testing conditions.\
\
Switch Over: Once the new version is verified, the traffic is switched from the Blue environment to the Green environment.\
\
Rollback Plan: If anything goes wrong, traffic can be immediately switched back to the Blue environment.

### Best Practices and Securing the CI/CD Pipeline

CI/CD pipelines and the applications that they work with face a variety of potential security risks. Some solutions that can be integrated into CI/CD pipelines to improve [application security (AppSec)](https://www.checkpoint.com/cyber-hub/cloud-security/what-is-application-security-appsec/) include the following:

* Source Composition Analysis (SCA): SCA solutions identify the third-party dependencies that an application uses and the potential vulnerabilities that they contain. This can protect against vulnerable third-party code and supply chain attacks.
* Source Code Scanning: Static application security testing (SAST) examines the source code of an application for potential vulnerabilities. [Code scanning](https://www.checkpoint.com/cyber-hub/cloud-security/what-is-code-scanning/) solutions enable DevOps teams to identify and correct vulnerabilities early in the software development lifecycle (SDLC) when they are less expensive to remediate.
* Security Testing: During the testing phase of the SDLC, [dynamic application security testing ](https://www.checkpoint.com/cyber-hub/cloud-security/what-is-dynamic-application-security-testing-dast/)(DAST) solutions can identify vulnerabilities in functional applications. These tests occur later in the SDLC but can identify issues that are undetectable by SAST solutions.
* Runtime Security: Vulnerabilities may be overlooked during testing or discovered after an application is in production. Runtime security solutions such as [runtime application self-protection](https://www.checkpoint.com/cyber-hub/cloud-security/what-is-runtime-application-self-protection-rasp/) (RASP) can provide ongoing monitoring and protection for an application after it has been deployed to production.

{% embed url="https://www.checkpoint.com/cyber-hub/cloud-security/what-is-ci-cd-security/" %}

**Now basically what is CI/CD????**

<figure><img src="../.gitbook/assets/image (6) (1).png" alt=""><figcaption><p><a href="https://media.licdn.com/dms/image/D4D12AQEYIeuQxtU24w/article-cover_image-shrink_600_2000/0/1673462246819?e=2147483647&#x26;v=beta&#x26;t=5xow7rhHwxge_UoNgb_fY0XlIh8lyvZqf920xwIkH70">https://media.licdn.com/dms/image/D4D12AQEYIeuQxtU24w/article-cover_image-shrink_600_2000/0/1673462246819?e=2147483647&#x26;v=beta&#x26;t=5xow7rhHwxge_UoNgb_fY0XlIh8lyvZqf920xwIkH70</a></p></figcaption></figure>

Now that you have a general understanding how Pipeline Systems and Security works. Take a look and answer these questions:

1. "How would you design a CI/CD pipeline to ensure maximum security at each stage?"
2. "Can you describe a situation where a CI/CD pipeline might be vulnerable to a supply chain attack, and how to mitigate it?"
3. "What are some common mistakes teams make in terms of security when setting up their CI/CD pipelines?"
4. "How would you integrate automated security testing into a CI/CD pipeline, and what tools would you use?"
5. "What are the best practices for managing secrets and credentials in CI/CD workflows?"

If yall want more information regarding this, since many companies make use of the CI/CD pipeline system....\
[https://vulcan.io/blog/ci-cd-security-5-best-practices/](https://vulcan.io/blog/ci-cd-security-5-best-practices/)\
[https://owasp.org/www-project-top-10-ci-cd-security-risks/](https://owasp.org/www-project-top-10-ci-cd-security-risks/)\
[https://sysdig.com/learn-cloud-native/container-security/cicd-pipeline/](https://sysdig.com/learn-cloud-native/container-security/cicd-pipeline/)\
(Very good to know if in the future you get the opportunity for a DEVOPS or DEVSECOPS job)

## Author

* [Nikhil](https://github.com/KR0N0S99)
