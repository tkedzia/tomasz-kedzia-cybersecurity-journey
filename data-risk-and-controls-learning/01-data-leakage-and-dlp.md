# Study Notes — 8 November 2026

## Data, Data Leakage and Data Loss Prevention

These notes cover today’s session: understanding sensitive data and leakage channels, learning the structure of a DLP policy, reviewing a completed data-flow map and external-email policy, and preparing interview answers.

Study time: approximately 60 minutes. No lab, additional research or written assignment is required. The worked examples are fictional and are not descriptions of EY’s environment.

| Time | Read and review |
|---|---|
| 0–10 minutes | Sections 1–3: foundations and leakage channels |
| 10–30 minutes | Sections 4–6: policy structure, classification and encryption |
| 30–50 minutes | Sections 7–9: completed data-flow map, policy and rollout scenario |
| 50–60 minutes | Sections 10–12: interview answers, knowledge check and recap |

---

## 1. The central idea

**DLP**, or Data Loss Prevention, helps organisations identify sensitive information, monitor activities involving that information, and apply protective actions when those activities violate defined rules. Depending on the location and configuration, actions can include warnings, blocking, restrictions or allowing an override with justification.

The objective is not to stop all data movement.

The objective is to enable legitimate business activity while reducing inappropriate disclosure.

Consider these two situations:

- An employee sends an approved public brochure to a prospective customer.
- An employee sends a confidential client spreadsheet to a personal email account.

Both involve sending a file externally. They should not necessarily receive the same treatment.

A useful mental model is:

    DATA + ACTIVITY + CONTEXT
              ↓
    POLICY EVALUATION
              ↓
    ALLOW / WARN / RESTRICT / BLOCK
              ↓
    LOGGING, REVIEW AND FOLLOW-UP

Microsoft Purview is the reference example in these notes. It is not an assumption about EY’s technology or the meaning of “Dataguard.”

### Key sentence to remember

> DLP is about controlling what happens to sensitive information in a defined context, not simply detecting that sensitive information exists.

---

## 2. Data leakage, exfiltration and legitimate sharing

For this lesson, use the following working distinctions:

| Term | Meaning in our discussion | Example |
|---|---|---|
| Legitimate sharing | An authorised transfer for an approved business purpose | Sharing the minimum required information with an authorised client contact |
| Data leakage | Unintended or inappropriate exposure of information | Sending a confidential attachment to the wrong recipient |
| Data exfiltration | Unauthorised transfer of information out of the environment or control boundary | Copying client records to a personal account or unapproved storage |

These are practical distinctions for analysing scenarios, not legal classifications.

Do not assume that every external transfer is prohibited or that every DLP alert proves malicious activity. Purview policies evaluate conditions and activities; their results still require interpretation in the relevant business context.

### A better way to describe the risk

Weak description:

> “There is no DLP.”

This describes a possible control gap, not the complete business risk.

Better description:

> “An employee may send confidential client information to an unauthorised external recipient, resulting in disclosure of client data and potential contractual, reputational or regulatory consequences.”

Then explain the proposed response:

> “A scoped email DLP policy could identify relevant content and restrict inappropriate external sharing, while providing an approved alternative for legitimate collaboration.”

The risk statement and proposed control above are a worked example, not an assessment of a real organisation.

---

## 3. Four leakage channels you should be able to name

Microsoft documents DLP capabilities across email, collaboration services, devices and web/cloud activities. The available conditions and actions differ by location.

| Channel | Fictional example | Possible control approach | Important limitation |
|---|---|---|---|
| Email | A confidential spreadsheet is attached to an email addressed to a personal account | Identify sensitive content and external recipients; warn or restrict sharing | An email policy does not automatically cover other transfer channels |
| Web upload | A user uploads a client document to an unapproved website or GenAI service | Use supported endpoint, browser or network/cloud controls to inspect and restrict the activity | Coverage depends on the actual browser, device, integration and policy scope |
| Removable media | A user copies a sensitive file to a USB drive | Audit or restrict copying sensitive files to removable media | The relevant endpoint must meet the product’s prerequisites and be in scope |
| Cloud sharing | A sensitive document is made accessible through an inappropriate sharing link | Detect sensitive content and restrict inappropriate external access in supported services | Creating or storing the file and sharing access to it are different activities |

These are possible control patterns, not guarantees that a single product configuration covers every channel.

### Interview-ready summary

> “I would consider at least four leakage channels: email, web uploads, removable media and cloud sharing. I would then verify which users, devices, services and activities are actually covered. A policy protecting one channel should not be treated as complete protection against exfiltration.”

---

## 4. The anatomy of a DLP policy

Example mockup of Purview DLP Policy: https://tkedzia.github.io/tomasz-kedzia-cybersecurity-journey/purview-dlp-policy/

A policy expresses a protection requirement. Its rules translate that requirement into conditions and actions. In Purview, rules can also include user notifications, overrides, incident reporting and processing options.

Use this structure when explaining a policy:

    1. Objective — What inappropriate outcome are we trying to prevent?
    2. Data — What information needs protection?
    3. Scope — Which users, groups, services or devices are covered?
    4. Activity — What is happening to the information?
    5. Conditions — What must be true for the rule to match?
    6. Action — What should happen when it matches?
    7. Notification — What should the user and operational team see?
    8. Exception — What legitimate cases require a different route?
    9. Evidence — How will we verify the expected behaviour?
    10. Ownership — Who maintains, reviews and escalates the control?

The first seven items reflect the technical and behavioural components described in Microsoft’s documentation. The remaining items make the example operationally reviewable.

### 4.1 Scope is part of the protection

A well-written rule is not useful for a user or location outside its scope.

Ask:

- Which senders are included?
- Which services are included?
- Are there exclusions?
- For endpoint controls, are the relevant users and devices included?
- Have the necessary onboarding and technical prerequisites been met?

Purview supports location-specific scoping, and endpoint enforcement can depend on both the user and the device being in scope.

### 4.2 Conditions are not actions

Example condition:

> The item contains specified sensitive information and is being shared externally.

Example action:

> Restrict the sharing and notify the user.

A match tells you that conditions were satisfied. It does not, by itself, establish that a blocking action occurred. Microsoft explicitly distinguishes rule matching from the actions performed.

### 4.3 A warning is not a block

| Response | What it means |
|---|---|
| Audit | Record the relevant activity without necessarily preventing it |
| Warn | Inform the user about the concern |
| Block with override | Restrict the activity but permit a configured override |
| Block without override | Prevent the activity without providing that user override option |

The available responses depend on the location and supported configuration.

---

## 5. Classification: identifying what matters

Before deciding whether a transfer is appropriate, determine what information is involved.

The following is an illustrative classification scheme, not an EY policy:

| Class | Example | Illustrative handling expectation |
|---|---|---|
| Public | Published marketing brochure | External sharing generally permitted |
| Internal | Routine internal working instructions | Share within authorised organisational channels |
| Confidential | Client engagement spreadsheet | Share only for a defined purpose with authorised recipients |
| Restricted | Particularly sensitive records or secrets | Apply tighter access and handling requirements |

### Business classification versus technical detection

Business classification answers:

> “How sensitive is this information, and how should it be handled?”

Technical detection answers:

> “How does the system recognise information that should trigger a rule?”

In Purview, relevant mechanisms can include sensitive information types, sensitivity labels and trainable classifiers, depending on the location. Sensitive information detection can use more than simple keyword matching, including patterns, validation and supporting context.

Examples:

- A sensitivity label identifies a document as confidential.
- A sensitive information type detects a recognised pattern of personal or financial information.
- A classifier identifies a category of content.

### Important limitation

Do not assume that every confidential document has a correct label or contains easily detectable identifiers.

For our worked example, that means:

> A label-based rule needs a documented assumption about label accuracy. An unlabelled confidential file remains a scenario requiring separate consideration.

---

## 6. DLP versus encryption

### 6.1 What encryption contributes

Encryption protects the confidentiality of information through cryptographic protection and appropriate access to the required keys.

However, an authorised application or user may be able to decrypt and read that information.

Our practical example:

    Encrypted database
            ↓ authorised access
    Readable data exported by an application
            ↓
    Spreadsheet attached to an external email

The database can remain encrypted at rest while an inappropriate export still occurs.

### 6.2 What DLP contributes

DLP can evaluate the information and the activity involving it, then apply a policy response in supported locations. For example, a rule may restrict external sharing or copying a sensitive file to removable media.

| Question | Encryption | DLP |
|---|---|---|
| What is the main concern in this example? | Protecting confidentiality when data is stored or transmitted | Identifying sensitive data and controlling risky activities |
| Can an authorised user still read the information? | Often yes, subject to access and key arrangements | Reading may be allowed while particular transfer activities are restricted |
| Does it automatically stop inappropriate external sharing? | Not simply because storage is encrypted | It may restrict sharing within the configured and supported scope |

Some information-protection solutions combine labels, encryption and usage restrictions. Therefore, avoid saying that encryption never affects sharing. The narrower point is:

> Storage encryption alone does not prove that inappropriate data export is prevented.

### Connection to your experience

A useful interview bridge is:

> “My encryption experience is relevant because it involves implementing data-protection requirements and resolving delivery dependencies. However, I distinguish encryption coverage from control over subsequent data use and transfer. I would not treat encryption deployment as proof that exfiltration risk has been eliminated.”

This is a way to explain the relationship between your experience and DLP, not a claim that you have operated production DLP.

---

## 7. Completed data-flow map

### 7.1 Fictional business scenario

A regional delivery team needs to share an extract of client information with an authorised external contact.

The information is confidential.

The business requirement is legitimate collaboration. The security requirement is to prevent inappropriate external disclosure.

### 7.2 The flow

    [1. Approved client repository]
                  |
                  | Authorised access
                  v
    [2. Employee on a managed device]
                  |
                  | Creates a minimum-necessary extract
                  v
    [3. Confidential working file]
                  |
                  | Attempts to attach file to email
                  v
    [4. Corporate email service]
                  |
                  | DLP evaluation:
                  | - Is the sender in scope?
                  | - Is the content sensitive?
                  | - Is the recipient external?
                  | - What policy response applies?
                  |
           +------+-------------------+
           |                          |
           v                          v
    [Restricted transfer]     [Approved transfer route]
           |                          |
    User guidance and        Recipient access controls,
    operational review       approved sharing and logging
                                      |
                                      v
                          [5. Authorised external recipient]

The external recipient represents a different organisational control boundary. An approved recipient does not automatically make every file, channel or sharing method appropriate.

### 7.3 Asset and responsibility inventory

These are proposed roles for the example, not universal organisational assignments.

| Asset or flow | Classification/context | Proposed accountable role | Key question |
|---|---|---|---|
| Client source repository | Confidential | Business data owner | Who is entitled to access the original information? |
| Employee device | Handles confidential content | Endpoint control owner | Is the device managed and covered by the relevant controls? |
| Working extract | Confidential | Business data owner | Does it contain only what is necessary? |
| Outbound email flow | Carries confidential information | Email/DLP control owner | Is the transfer allowed, and what action should the policy take? |
| External sharing arrangement | Confidential information leaves the organisation’s direct environment | Business process owner | Is the recipient, purpose and transfer method approved? |

### What the map helps you see

The control question is not merely:

> “Is the original database encrypted?”

It is:

> “What happens to the information after an authorised user retrieves it?”

That is the connection between data protection at the source and controls over subsequent handling.

---

## 8. Completed external-email DLP policy

This is a design example, not executable product configuration.

### 8.1 Policy intent

> Prevent members of a regional pilot group from sending confidential client documents through ordinary external email, while directing legitimate collaboration to an approved transfer route.

### 8.2 Policy specification

| Component | Proposed design |
|---|---|
| Policy name | Regional confidential-client-data email pilot |
| Business objective | Reduce inappropriate disclosure through external email |
| Protected data | Documents identified as confidential client information |
| Initial scope | A defined regional pilot sender group |
| Location | Corporate email service |
| Triggering activity | Sending an email to an external recipient |
| Content condition | The attachment meets the agreed confidential-content detection criteria |
| Proposed enforcement response | Restrict the relevant external sharing without routine user override |
| User guidance | Explain the reason and direct the user to the approved sharing method |
| Operational reporting | Record the match and observed action; alert according to configured criteria |
| Exception route | A separately approved, narrowly scoped process with an owner and review date |
| Control ownership | Named DLP/email control owner, supported by technical and business teams |

The scope, conditions, actions, notifications and reporting model follow the policy components described by Microsoft. Exact configuration and behaviour must be validated for the selected location.

### 8.3 Logical representation

    IF
        sender is in the pilot scope
    AND
        at least one recipient is external
    AND
        attachment meets the confidential-content condition

    THEN
        apply the configured restriction
        show guidance about the approved transfer route
        record the match and resulting action
        generate an alert if the configured alert criteria are met

A business-approved exception must be translated into a controlled implementation. A verbal approval does not change system behaviour.

### 8.4 Worked test cases

These are expected results for the proposed design, not actual test outcomes.

| Test | Scenario | Expected result | What it checks |
|---|---|---|---|
| T1 | In-scope sender emails a confidential attachment externally | Configured restriction and notification occur | Main protection requirement |
| T2 | In-scope sender emails an approved public brochure externally | This confidential-content rule does not restrict it | Avoiding unnecessary business disruption |
| T3 | In-scope sender emails confidential content internally | This external-email rule does not trigger solely because the content is confidential | Recipient-context condition |
| T4 | Out-of-scope sender performs the same external transfer | This pilot policy does not provide coverage | Scope limitation |
| T5 | In-scope sender uploads the file to a website | This email policy provides no assurance over that channel | Channel limitation |
| T6 | In-scope sender sends an unlabelled confidential file | Depends on the detection methods configured | Classification/detection limitation |

The presence of other policies could change the overall result. Test both the intended rule and the effective combined behaviour.

### 8.5 Evidence to collect

For each test, the proposed evidence record would contain:

- Test identifier and execution date.
- Policy version and deployment mode.
- Sender and location scope.
- Synthetic test-file characteristics.
- Expected action.
- Observed action and user experience.
- Relevant event or alert identifiers.
- Pass/fail conclusion and follow-up.

Do not conclude “blocked” solely because a rule matched. Microsoft distinguishes the match from the resulting action.

---

## 9. Applying the example to a regional rollout

### Scenario

A global requirement restricts inappropriate external transfer of confidential files. The region still needs to collaborate with clients.

The correct starting point is neither “block everything” nor “exclude the whole region.”

A practical sequence is:

1. Understand the global control objective.
2. Identify regional data flows and legitimate sharing needs.
3. Confirm scope, ownership and technical prerequisites.
4. Design the policy and approved collaboration route.
5. Evaluate policy matches and business impact.
6. Run controlled enforcement tests and a limited pilot.
7. Provide user guidance and an exception process.
8. Expand deployment only when agreed criteria are met.
9. Continue monitoring, tuning and operational review.

Microsoft’s DLP lifecycle similarly emphasises business-process planning, stakeholder involvement, testing, simulation, tuning and user adoption.

### Simulation versus enforcement

Simulation helps evaluate matches and potential business impact before broader enforcement. It does not replace a controlled test of the actual protective action. Microsoft also documents location-specific behaviour and differences between simulation and enforcement processing.

Safe interview wording:

> “I would verify the exact behaviour of the selected workload and deployment mode. I would use simulation to assess likely impact, then validate the actual enforcement action in a controlled pilot.”

### Two distinctions to retain

    Policy deployed ≠ control proven effective
    Rule matched ≠ transfer blocked

---

## 10. Interview answers to review aloud

### Question 1: “What is DLP?”

> “Data Loss Prevention helps an organisation identify sensitive information, monitor activities involving that information and apply protective actions when defined conditions are met.
>
> For example, a policy may identify a confidential attachment being sent externally and warn the user or restrict the sharing.
>
> I would always clarify the data, users, channel, conditions and expected action, because DLP protection depends on the actual scope and supported capabilities.”

### Question 2: “How is DLP different from encryption?”

> “Encryption protects confidentiality, but authorised users or applications may still be able to read and export the information. DLP addresses what happens to sensitive data in a particular activity, such as external sharing or copying to removable media.
>
> For example, database encryption does not by itself prevent an authorised user from exporting readable data into a spreadsheet and emailing it externally. The controls are complementary.”

### Question 3: “How would you approach a regional DLP rollout?”

> “I would start by clarifying the global control objective and mapping the regional data flows and legitimate business needs.
>
> Next, I would agree scope and ownership, identify gaps and dependencies, and design the policy together with an approved collaboration route.
>
> I would evaluate matches and business impact, validate enforcement in a controlled pilot, communicate the expected user behaviour and establish an exception process.
>
> Before broader rollout, I would confirm the test results and operational ownership. I would report deployment coverage separately from validation results and remaining risk.”

This is a proposed approach for the fictional scenario, not a statement about EY’s internal process.

### Question 4: “Do you have production DLP experience?”

Use this wording only if it accurately reflects your experience:

> “My closest professional experience is in encryption, data-protection delivery and security remediation rather than production DLP policy operation.
>
> The transferable part is translating requirements into implementation steps, coordinating dependencies and following issues through to closure.
>
> I am developing my understanding of DLP scope, conditions, actions and testing. I distinguish that learning from production experience.”

---

## 11. Knowledge check with answers

Pause before reading each answer. No written work is necessary.

### 1. Name four leakage channels.

Email, web uploads, removable media and cloud sharing. Available protection differs by channel and configuration.

### 2. Does an encrypted database prove that exported information cannot leak?

No. An authorised user or application may retrieve readable information. Subsequent handling needs to be considered separately.

### 3. What is the difference between a policy condition and an action?

A condition determines whether a rule matches. An action determines the configured response, such as warning or restricting sharing.

### 4. Is every external recipient unauthorised?

No. In the worked scenario, external collaboration can be legitimate. The recipient, purpose, information and transfer method must fit the approved process.

### 5. Does a rule-match event prove that data was blocked?

No. Review the configured action and observed result.

### 6. Does an email DLP policy protect USB copying?

Not simply because the email policy exists. Removable-media activity requires relevant endpoint capabilities and scope.

### 7. What is a false positive in our example?

A detection incorrectly identifies content or an activity as meeting the relevant protection condition. A user complaint alone does not establish that the detection was wrong; the content, rule and business context need review.

### 8. What is a false negative in our example?

A sensitive transfer that should have been detected is not identified—for example, because the detection criteria do not recognise the content.

### 9. What is the purpose of the public-brochure test?

To check that the confidential-content rule does not unnecessarily restrict a legitimate, non-sensitive transfer.

### 10. What should an analyst say if asked to approve a broad regional exclusion?

> “I would clarify the business need and exposure, assess narrower alternatives, and route the decision to the authorised owner. I would not treat a broad exclusion as a routine technical fix.”

---

## 12. Final one-minute recap

> “DLP helps identify sensitive information and apply controls to activities involving that information.
>
> I need to understand the data, users, location, activity, conditions and expected action.
>
> Four important channels are email, web uploads, removable media and cloud sharing. Coverage of one channel does not imply coverage of the others.
>
> Encryption and DLP are complementary. Storage encryption alone does not prevent an authorised user from exporting readable information.
>
> For a regional rollout, I would map legitimate data flows, agree scope and ownership, test the policy, support users and monitor remaining gaps.
>
> Finally, I would distinguish a rule match from a successful block, and deployment from verified effectiveness.”

### What completing this session establishes

After reviewing these notes, you have studied:

- The main DLP concepts and four leakage channels.
- A completed data-flow example.
- A proposed external-email policy.
- Expected test results and coverage limitations.
- Interview explanations connecting DLP to your existing experience.

This is conceptual preparation and a worked policy-design review. It is not evidence of a configured DLP lab, successful enforcement tests or production DLP experience.
