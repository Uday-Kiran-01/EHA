# Design Decisions

> Why I built this agent the way I did — written for a hiring manager reviewing my portfolio.

## 1. Why Copilot Studio (not LangChain / custom code)?

ING's JD specifically calls for Copilot Studio and Power Platform experience. The role is about configuring agents within the Microsoft ecosystem, not building AI infrastructure from scratch. Copilot Studio is the right tool because:

- **Low-code** means faster iteration. A topic change takes minutes, not pull requests.
- **M365-native**: Agents live inside Teams, SharePoint, and Copilot — where 80,000 ING employees already work.
- **Governance built-in**: Agent 365, DLP, and Entra ID handle the security that a bank legally requires.
- **1,400+ connectors**: ServiceNow, Workday, SAP — no custom API integration code needed.

## 2. Why Adaptive Cards (not custom UI)?

The JD specifically calls out adaptive cards for ticket creation. I chose them because:

- **WCAG-compliant**: Native screen reader support, keyboard navigation, proper ARIA labels.
- **Cross-channel**: Same card works in Teams, Outlook, SharePoint, and mobile.
- **Structured data**: A form collects clean data (title, severity, description, email) with validation — much better than free-text parsing.
- **User feedback loop**: Cards include confirmation before submission and clear error states.

## 3. Why Intent Routing (not just generative AI)?

I use a hybrid approach:
- **Recognized intents** (create ticket, check status, escalate) have structured topic flows with slot filling for reliable data extraction.
- **Unknown intents** fall back to **Conversational Boosting** (generative AI + knowledge search) — this handles the "long tail" of questions without breaking the structured workflows.

This follows the JD's requirement for "intent routing, fallback handling, and escalation."

## 4. Why Mock ServiceNow (not real)?

This is a **portfolio demo** — I don't own a ServiceNow instance. But I designed the integration with:
- Real ServiceNow API schema (incident table fields: short_description, impact, urgency, caller_id)
- The Power Automate flow is fully production-ready — just swap the endpoint URL
- The mock returns realistic `INC0009XXX` ticket numbers

## 5. Accessibility (WCAG) Decisions

- All adaptive cards use proper `label` attributes for screen readers
- Error messages are descriptive (not just "invalid input")
- Emoji is used sparingly and always paired with text labels
- Response messages avoid relying on visual formatting alone
- The escalation flow preserves full context so the human agent doesn't need the user to repeat themselves

## 6. Evaluation Strategy (Beyond Star Ratings)

The JD says: *"Collaborate on measurement strategies that go beyond star ratings."* My eval set covers:

- **Category coverage**: Troubleshooting, Knowledge Lookup, Ticket Creation, Status Lookup, Escalation, Edge Cases
- **Safety boundaries**: Tests for competitor discussions, out-of-scope questions (HR, compensation)
- **Accessibility**: Explicit test for screen reader compatibility
- **Edge cases**: Empty input, ambiguous queries ("help"), repeated attempts

## 7. What I'd Do Differently in Production

- Add **Dutch language fallback** (ING is Netherlands-based)
- Implement **speech-to-text** readiness for voice channels
- Add a **Power BI dashboard** for agent analytics
- Set up **automated eval runs** in a CI/CD pipeline (Copilot Studio supports this)
- Configure **DLP policies** to prevent data leakage
