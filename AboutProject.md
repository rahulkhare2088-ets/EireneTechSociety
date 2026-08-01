# ETS Order Fulfillment Processor Engine v1.0.0-Beta 

A high-performance, modular supply chain infrastructure engine built on **Spring Boot (Java open-source ecosystem)**. This ecosystem provides a secure **OAuth2 Authorization Server**, an isolated **API Gateway Proxy**, and a resilient **Fulfillment Processing Core** designed to handle asynchronous e-commerce (B2B) transaction state machines at scale.  

ℹ️ **Project Status:** Currently operating in **Phase 1 (Bootstrapping)**. The hosted API endpoints are **100% Free** for public use, sustained entirely via developer sponsorships and crowdfunding. 
--- 

## 🚀 Live API Reference & Links 

*   **Interactive Documentation:** [Postman API Portal](https://www.postman.com/eirene-tech-society-2088/ets-public/documentation/bu73jqs/order-fulfillment-processor-engine?sideView=agentMode) 

*   **BaaS (Backend as a Service) project Links:** Click below links and takes 1-2 min for all 3 respective instances to spin up and run, currenty after 15 min of idle time (no request) instance spin down automatically.
  
    [API Gateway Proxy](https://portal.eirenetechsociety.in/swagger-ui/index.html)
    [Authorization Server](https://auth.eirenetechsociety.in/auth) 

--- 

 

## 🏗️ Ecosystem Architecture 

 

The platform splits complex fulfillment system into three decoupled, secure layers to prevent processing bottlenecks: 

 

┌──────────────────────────────────────┐ 

     Public API Client Integration (AI Agent/UI/Mobile App)  

└───────────────────┬──────────────────┘ 

                              HTTPS 
                                ▼ 

┌──────────────────────────────────────┐ 

                       API Gateway Proxy  

└────────┬────────────────────┬────────┘ 

     OAuth2 Token Verification            Routed Requests     
                ▼                                ▼

┌───────────────────┐ ┌─────────────────┐ 

        Authorization Server            Fulfillment Engine  

└───────────────────┘ └─────────────────┘ 

 

### 1. Authorization Server 

*   Acts as the secure perimeter for identity and multi-tenant access control. 

*   Issues cryptographically signed tokens to validate downstream requests securely. 

*   Enables safe third-party integrations and protects client operational data boundaries. 

 

### 2. API Gateway Proxy 

*   Serves as the single entry point for all external system calls, shielding internal services. 

*   Manages operational boundaries, intelligent request routing, and payload inspection. 

*   Ready with implementation of rate-limiting, CORS handling, and telemetry logging. 

 

### 3. Fulfillment Processing Core 

*   Executes structured transaction requests and governs complex order life cycles. 

*   Maintains strict data persistence synchronization across distributed microservices. 

*   Engineered to seamlessly balance platform network loads during peak request spikes. 

 

--- 

 

## 🛠️ Technology Stack & Deployment 

 

*   **Backend Framework:** Spring Boot (Java open-source ecosystem) 

*   **Security Protocols:** OAuth2 / JWT (JSON Web Tokens) 

*   **Database Management:** Neon Serverless PostgreSQL 

*   **Cloud Hosting Infrastructure:** Render (Compute Engines) & AWS (SQS) 

 

--- 

 

## 🗺️ Community Growth & Open Source Roadmap 

 

We believe in building public utilities transparently. The engine follows a phased path from an individually maintained deployment to a completely community-governed foundation: 

 

*   **Phase 1 (Current): Free Public Tier.** The codebase remains closed-source while we stress-test infrastructure stability. Hosted API layers are entirely free for developers, funded by community micro-donations. 

*   **Phase 2: Open Core & Commercial Scale.** Introduction of enterprise high-volume tiers. All incoming revenue directly finances advanced cloud scaling, with a 10% operational allocation to the Eirene Tech Society for ongoing maintenance. 

*   **Phase 3: The Apache Foundation Transition.** The entire source code will be released publicly under the **Apache 2.0 License**. Governance will officially transfer to an independent, Indian Section 8 non-profit foundation. 

 

--- 

 

## 💖 Supporting the Infrastructure 

 

Maintaining real-time server clusters on Render, AWS, and Neon requires continuous compute power. If this order processing engine accelerates your development workflows, please consider sponsoring our live runtime costs: 

 

*   🌐 **International Support:** [Sponsor via GitHub Sponsors](https://github.com) (Zero processing fees) 

*   🇮🇳 **Domestic India (UPI/Cards):** [Support via Razorpay Page](https://rzp.io) 

 

--- 

*Developed and maintained by Eirene Tech Society. Driven by open-access technology.* 
