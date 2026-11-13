# Personal Product Case Study: EHA (The Authentic PM Journey)

This document breaks down the product thinking, strategic trade-offs, and authentic motivations behind **EHA (Employee Help Assistant)**. When I saw the AI Engineer role at ING Bank requiring Microsoft 365 and Copilot Studio familiarity, I refused to just read documentation and list buzzwords on my CV. I approached this as an aspiring Product Manager: I wanted to design a real, working product architecture that solves a deeply human bottleneck in the modern digital workplace.

---

### 1. User Discovery (The Workplace Friction Gap)
**The Raw Problem:** In an enterprise with 80,000+ employees, internal IT support is where employee momentum dies. When an employee is blocked by a software crash or VPN dropout, they are thrust into a labyrinth of complex ITSM portals with 50 dropdown categories, or worse, forced to talk to an unhelpful chatbot that loops endlessly without an exit. On the other side, support engineers aren't slow by choice - they are buried under thousands of incomplete tickets like *"my laptop is broken"* with no error codes, device info, or urgency level. Both sides are exhausted and frustrated.
**The PM Discovery:** I realized that most enterprise workplace chatbots fail because they treat conversational AI as a novelty rather than a structured workflow engine. For 80,000 employees, you cannot just throw an unconstrained LLM at them and pray it doesn't hallucinate or leak sensitive bank data. You need a reliable, accessible bridge between human conversational needs and rigid enterprise systems of record (like ServiceNow).

---

### 2. Roadmapping & Prioritization (The Low-Code & WCAG Scope Cut)
**The Core Goal:** The goal wasn't to write 5,000 lines of custom Python or React code just to prove technical vanity. In a regulated banking environment like ING, deploying unvetted custom microservices into core infrastructure creates massive security review and maintenance friction. The goal was to build a governed, production-viable architecture within the Microsoft ecosystem where employees already live every day.
**My Pivot:** My initial instinct was to build a full custom web chat widget using React and WebSockets. But I had to check my ego. An external widget requires custom SSO integration, separate security audits, mobile wrappers, and WCAG accessibility certifications from scratch. I ruthlessly cut the custom frontend. I chose **Microsoft Copilot Studio** and **Adaptive Cards**. Why? Because Adaptive Cards are natively WCAG-compliant (built-in screen reader support, keyboard navigation, high contrast), and Copilot Studio natively inherits Entra ID governance, DLP policies, and pre-built ServiceNow connectors. I traded custom code vanity for enterprise viability and accessibility.

---

### 3. Sprint Execution & The Tenant Reality (Where I Paused)
**The Hustle:** I mapped out the entire product architecture: the agent instructions, the 5 core dialog topic state machines, the accessible Adaptive Card v1.5 form, the Power Automate flow mapping plain-English severity into ITIL `impact`/`urgency` values, and a 16-case golden evaluation test suite.
**The Tenant Bottleneck:** Once the architecture was written, I wanted to test the entire flow end-to-end inside Microsoft Teams. However, testing Copilot Studio agents connected to live Power Automate and Teams channels requires an active **work or school organization account (an enterprise M365 tenant)** and a live corporate ServiceNow instance. As an independent individual without an enterprise corporate account, I hit a hard stop.
**The PM Decision:** As a PM, when you hit an infrastructure blocker, you don't panic or pretend the deployment happened. You document the specifications rigorously. I finalized the YAML topics, adaptive card JSON, flow templates, and evaluation datasets so that the architecture, guardrails, and accessibility designs were 100% complete and ready for deployment the moment enterprise tenant access is granted.

---

### 4. Self-Management (Preventing the "AI Trap")
**The Crisis:** The most infuriating anti-pattern in conversational AI is the "AI Trap" - when a chatbot insists on giving useless suggestions and actively blocks the user from reaching a human. In high-stress workplace emergencies (e.g., system outages, critical trading floor issues), trapping a user inside a bot is unacceptable product design.
**My Action:** I established clear, hard rules for human escape:
1. If the user explicitly asks for a human ("agent please", "talk to someone"), the bot immediately stands down.
2. If the user encounters two consecutive failed resolution attempts, the system automatically triggers escalation.
3. Crucially, I engineered the escalation handoff so the bot compiles a structured handoff memo (user name, department, attempted steps, and timestamp). The human engineer receives the complete context instantly, meaning the user never has to re-explain their problem.

---

### 5. Value Proposition & Measurement (Beyond Star Ratings)
**The Original Flaw:** Enterprise IT dashboards measure chatbot success using lazy vanity metrics: *"4.2 out of 5 stars"* or raw message counts. A star rating tells you nothing about whether the user actually got back to work or simply gave up in disgust.
**The True ROI:** The true value of EHA is **Time-to-First-Resolution (TTFR)** and **Ticket Intake Quality**.
*   **For the Employee:** Instant self-service deflection for tier-1 tasks, or a sub-60-second ticket creation experience without leaving Microsoft Teams.
*   **For the Support Desk:** 100% of created tickets arrive with complete, structured fields - eliminating the "clarification back-and-forth" that slows down resolution times.
*   **Continuous Diagnostic Learning:** In `collect-feedback.mcs.yml`, I built a diagnostic feedback loop that captures qualitative root causes (what worked vs. what failed) and streams them directly into Dataverse for continuous prompt and knowledge base optimization.

---

### 6. Product Intuition (The Superpower & Empathy Lens)
**The Skill:** Just as I apply lifelong pattern recognition and emotional intuition to understand why certain stories connect with audiences while others fall completely flat, I apply that same gut intuition to software products. When someone has an IT problem at work, they aren't looking to "interact with an AI"; they are in a state of professional anxiety because their productivity is blocked.
**The Realization:** Because of that empathy-driven gut feeling, I recognized that **accessibility and clear boundaries are not optional "nice-to-haves"** - they are core usability requirements. If an employee with a screen reader cannot navigate the ticket form, the product has fundamentally failed them. If the bot wanders into inappropriate topics or gives vague, non-committal answers, trust evaporates immediately. I built EHA with clean guardrails, single-question clarifications, and guaranteed human escalation because respecting employee time is the only way an enterprise product earns real adoption.
