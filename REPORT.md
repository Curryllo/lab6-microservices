# Lab 6 Microservices - Project Report

## 1. Configuration Setup

**Configuration Repository**: https://github.com/Curryllo/lab6-microservices.git

Each file in config/config/ has these:
- Service id
- Server port
- Enables env files
- Enables info and health endpoints
- Publish all endpoints available
- Static info from the service

Describe the changes you made to the configuration:

- What did you modify in `accounts-service.yml`?
- server.port from 3333 to 2222
- Why is externalized configuration useful in microservices?
  1. All instances are updated or none are
  2. Environment-specific configs (dev, staging, prod)
  3. Version control of configuration changes
  4. Change behaviour without inactivity
---

## 2. Service Registration (Task 1)

### Accounts Service Registration

![Accounts Registration Log](docs/screenshots/accounts-registration.png)

Explain what happens during service registration.
The service registers with Eureka using ACCOUNTS-SERVICE, then fetches configuration
from Config Server via Eureka. Finally, the service starts in the given port and
accepts requests.

### Web Service Registration

![Web Registration Log](docs/screenshots/web-registration.png)

Explain how the web service discovers the accounts service.
Uses Eureka to find ACCOUNTS-SERVICE.
---

## 3. Eureka Dashboard (Task 2)

![Eureka Dashboard](docs/screenshots/eureka-dashboard.png)

Describe what the Eureka dashboard shows:

- Which services are registered?
- Server configuration, accounts service and web service. Eureka is not
- self registered thanks to `register-with-eureka: false` in `application.yml`
- What information does Eureka track for each instance?
- Registers IP, name and port of each instance

---

## 4. Multiple Instances (Task 4)

![Multiple Instances](docs/screenshots/multiple-instances.png)

Answer the following questions:

- What happens when you start a second instance of the accounts service?
- When the second instance is launched, it uses the new port, 2222.
- How does Eureka handle multiple instances?
- It just shows the list of the available services under the same name
- How does client-side load balancing work with multiple instances?
- Since Eureka answered with the list of the available services, the client has to choose which one. Usually uses
- a Round Robin algorithm.

---

## 5. Service Failure Analysis (Task 5)

### Initial Failure

![Error Screenshot](docs/screenshots/failure-error.png)

Describe what happens immediately after stopping the accounts service on port 3333.
The dashboard shows the service in down status, but we must be quick, in just a second it will disappear.

### Eureka Instance Removal

![Instance Removal](docs/screenshots/instance-removal.png)

Explain how Eureka detects and removes the failed instance:

- How long did it take for Eureka to remove the dead instance?
- In the second request made, it works. In this lab, Eureka is configured so every
- second checks for the services available. When 1 second passes, Eureka removes the
- fallen instance from the list.
- What mechanism does Eureka use to detect failures?
- It uses a heartbeat mechanism. If Eureka stops receiving heartbeats, Eureka marks it as expired.

---

## 6. Service Recovery Analysis (Task 6)

![Recovery State](docs/screenshots/recovery.png)
![img.png](img.png)
Answer the following questions:

- Why does the web service eventually recover?
- When the fallen service is acknowledged by Eureka, it deletes it from the available
- ones and it only offers the ones that are up and the service is recovered.
- How long did recovery take?
- 1 second plus the time until the next client cache refresh
- What role does client-side caching play in the recovery process?
- It acts as a temporary buffer that may have outdated information, the client might try to connect to a service which
- is down.

---

## 7. Conclusions

Summarize what you learned about:

- Microservices architecture
- These kind of architectures provide:
  - Scalability
  - Resilience
  - Technology diversity
  - Independent deployment
- Service discovery with Eureka
- Acts as a dynamic registry which can handle multiple instances at the same time
- and allows other services to talk between them
- System resilience and self-healing
- It handles fallen services by removing them and updating itself
- Challenges you encountered and how you solved them
- None, everything worked fine at the first try

---

## 8. AI Disclosure

**Did you use AI tools?** (ChatGPT, Copilot, Claude, etc.)

- If YES: Which tools? What did they help with? What did you do yourself?
- Yes, I used Gemini. Just to understand better every component in this lab.
- If NO: Write "No AI tools were used."

**Important**: Explain your own understanding of microservices patterns and Eureka behavior, even if AI helped you write parts of this report.
Microservices are a bunch of independent services that discover themselves by Eureka Server. The Config Server provides a centralized
configuration in order to make configuration tasks easier by doing one change for all the services of that kind. Then you
can launch any other service that will register dynamically with Eureka. Eureka also handles availability by removing fallen
services.
---

## Additional Notes

Any other observations or comments about the assignment.

