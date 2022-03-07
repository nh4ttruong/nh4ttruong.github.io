---
name: SDLC/DevSecOps simulation project on basic Java Web Application
tools: [DevSecOps]
image: https://raw.githubusercontent.com/nh4ttruong/milestone/images/cycle.png
description: SDLC/DevSecOps cycle with basic Java Web Application. This project use Jenkins to integrations all of stages.
---
# Milestone - SDLC/DevSecOps simulation project on basic Java Web Application
Learnathon is project which used to demo SDLC/DevSecOps cycle

[You can view demo here!](https://youtu.be/W9Mrn4KiaBE)

## About Milestone DevSecOps cycle
I use Jenkins for through out the process and all job is handled on it.

![DevSecOps cycle](https://raw.githubusercontent.com/nh4ttruong/milestone/images/cycle.png)

### Plan & Analysist Stages
- Prepare the checklist with [Security Knowledge Framework](https://www.securityknowledgeframework.org/)
- Value security & risk of blueprint using Miro App to design threat modeling

### Coding Stage
I use:
- VS Code: IDE for dev
- SonarLint: Application of Sonar ecosystem to improve code quality, code pattern and security code
- Github: Store & archive sources, prepare for next stage
- Maven: Build java project

### Test & Scan Stage
In this stage, I will use 2 type of scanning - SAST & DAST scan for this project:
- SAST (Static Application Security Testing): OWASP Dependency-Check and SonarQube scan
- DAST (Dynamic Application Security Testing): OWASP ZAP Proxy

### Release & Deploy Stage
- Jenkins Tool
- Heroku Cloud: for web app deploy and monitoring

## Threat Modeling

![DevSecOps Cycle](https://raw.githubusercontent.com/nh4ttruong/milestone/images/cycle.pngthreatmodeling.png)

![Infomation Table](https://raw.githubusercontent.com/nh4ttruong/milestone/images/info.png)

<p class="text-center">
{% include elements/button.html link="https://github.com/nh4ttruong/milestone" text="Github Repo" %}
</p>
