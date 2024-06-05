# Security Best Practices with Sherlock

 05.06.2024 | Shogoki (linktr.ee/shogoki) | Security Researcher Sherlock

---

# Agenda

#### Introduction

#### Security Best Practices for Protocols

#### The Rekt Test

#### Common Hacks - Donation Attack

#### Best practices for working with Auditors

#### Q&A and Wrap-Up

---

### Why Security matters?

<img alt="DeFi Hack Statistics" src="https://www.chainalysis.com/wp-content/uploads/2024/01/yearly-value-stolen-1500x1030.png" height="300px" />

Risk of:

financial loss

reputation loss

**Sherlock’s mission is to create an open, uncensorable financial system that is secure enough for billions of people to trust with their life savings.**

---

### The promise of Web3

- Empower user control by decentralization
- Foster innovation

--> Secure Smart Contracts are inevitable to ensure Trust and resilence in these Systems.

---

## Security Best Practices - Smart Contract Development


- Coding with Security in Mind
- Avoid common vulnerabilities
    - Reentrancy -> Follow Checks-Effects-Interactions (CEI) pattern
    - Inflation Attacks
    - Rounding errors -> Round in favor of the protocol
    - Input validation

|||

## Security Best Practices - Smart Contract Development

### Security by Design 

- **Reduce attack surface:** Keep smart contracts as simple as possible. KISS 
- **Continuous monitoring:** Use tools to monitor smart contract activity for unusual patterns.
- **Secure defaults:** Default to the least privilege necessary.
- **Minimal privileges:** Restrict access to privileged functions and variables to the smallest possible audience.
- **Zero Trust:** No inherent trusted entity, assuming nobody is trusted.

|||

## Security Best Practices - Operational Security

### Build with a trusted team

- People are still the weakest part of the security chain.
- Ensure your team is trusted & skilled.
- Maintain clear security roles.

|||

## Security Best Practices - Operational Security

### External Dependencies

Mapping & Monitoring of external dependencies:

- services
- contracts
- oracles

Regularly monitoring for changes or vulnerabilities

---

## The Rekt Test

Set of 12 questions to test your security 

Developed by leading Web3 Security Researchers

Response to the lack of cybersecurity standards in the blockchain ecosystem

<pre>Credit: https://www.fireblocks.com/blog/the-rekt-test-12-questions-to-assess-your-blockchain-security/</pre>

|||


## Rekt Test

### Do you have all actors, roles, and privileges documented?

- Clarifies who can access system resources and their authorized actions.
- Facilitates comprehensive testing to identify security gaps and improper access controls.
- Serves as a reference point for auditors to identify discrepancies and potential security risks.

|||

### Do you keep documentation of all the external services, contracts, and oracles you rely on?

- Identifies risk exposure from dependencies such as cloud services and DeFi protocols.
- Helps prepare for security incidents affecting external dependencies.
- Ensures continuous operation by mitigating the security impact of external incidents.

|||

### Do you have a written and tested incident response plan?

- Includes steps to identify, contain, and remediate security incidents.
- Ensures team members are familiar with the plan through training and regular testing.
- Mitigates key person risk and anticipates scenarios where key personnel are unavailable.

|||


### Do you document the best ways to attack your system?

- Constructs a threat model to document potential attack avenues.
- Helps understand if existing security controls are sufficient to mitigate attacks.
- Aligns efforts to mitigate attacks where they are most likely to occur.

|||

### Do you perform identity verification and background checks on all employees?

- Ensures accountability and trust in the development team.
- Protects against malicious actors who could interfere with development or steal funds.
- Considers local laws and jurisdiction when making operational security decisions.

|||

### Do you have a team member with security defined in their role?

- Ensures there is accountability for the safety and security of the system.
- Helps identify threats, triage incidents, and remediate vulnerabilities.
- Oversees cross-departmental efforts to include security practices in all aspects of the organization.

|||

### Do you require hardware security keys for production systems?

- Protects against credential stuffing, SIM swap attacks, and spear phishing.
- Uses hardware keys to access company resources, ensuring strong protection.
- Examples of hardware keys include U2F tokens like YubiKey and Google Titan.

|||

### Does your key management system require multiple humans and physical steps?

- Prevents unilateral control by requiring consensus for important decisions.
- Employs multi-signature or multi-party computation (MPC) controls.
- Reduces risks of fraud, theft, or misuse by requiring physical key management.

|||

### Do you define key invariants for your system and test them on every commit?

- Helps ensure the system's expected behaviors and reduces unexpected outcomes.
- Documents assumptions about the system and targets critical properties.
- Uses automated tools to test invariants throughout the development process.

|||

### Do you use the best automated tools for discovering security issues in your code?

- Automated security tools are a baseline requirement for security strategy.
- Tools like static analyzers and fuzzers help catch critical security bugs before deployment.
- Recommended tools include Echidna for fuzzing and Slither for static analysis.

|||

### Do you undergo external audits and maintain a vulnerability disclosure or bug bounty program?

- External auditors provide in-depth knowledge and identify vulnerabilities.
- Audits help restructure development and testing workflows to prevent vulnerabilities.
- Bug bounty programs offer a public option for reporting bugs and enhancing security posture.

|||

### Have you considered and mitigated avenues for abusing users of your system?

- Security strategy should include abusability testing to consider social, psychological, and physical harm.
- Creates guidance for users to minimize risks like phishing and social engineering attacks.
- Evaluates processes and mitigations for protecting high-impact stakeholders and users.

---

## Common Hacks - Donation Attack

Well known vulnerabiliy in CompoundV2 forks.

Inflating the exchange rate of cTokens for new/empty markets


```javascript
    function exchangeRateStoredInternal() virtual internal view returns (uint) {
        uint _totalSupply = totalSupply;
        if (_totalSupply == 0) {
            /*
             * If there are no tokens minted:
             *  exchangeRate = initialExchangeRate
             */
            return initialExchangeRateMantissa;
        } else {
            /*
             * Otherwise:
             *  exchangeRate = (totalCash + totalBorrows - totalReserves) / totalSupply
             */
            uint totalCash = getCashPrior();
            uint cashPlusBorrowsMinusReserves = totalCash + totalBorrows - totalReserves;
            uint exchangeRate = cashPlusBorrowsMinusReserves * expScale / _totalSupply;

            return exchangeRate;
        }
    }

// ...

function getCashPrior() virtual override internal view returns (uint) {
        EIP20Interface token = EIP20Interface(underlying);
        return token.balanceOf(address(this));
    }
```

|||


### Attack Steps

1. Wait for a new market with empty liquidity deployed.
2. Mint the smallest possible amount of cTokens.
3. Donate a large amount of underlying tokens to inflate the exchange rate.
4. Borrow a different asset with the manipulated exchange rate.
5. Burn the cTokens to recover the donation.

|||

### Sonne Finance Exploit - May 2024

Sonne Finance was exploited on this vulnerabiltity

Loss around $20 million in `WETH`, `VELO` & `USDC`

Sonne Team was aware of this vulnerability and had mitigational steps:
1. adding markets with no collateral
2. adding and burning collateral
3. finally, increasing the c-factors associated with the markets

|||

### What went wrong?

Creating VELO Markets via Governance proposal

separate permissionless time-locked transactions.

Attacker was able to control, when the transactions got executed.

|||

### Learnings

- Common Attack vector - Inflation Attack
- Operational Security matters
- "False Security" with Forks of battle-tested protocols

---

## Working with Auditors

### Choosing the right Auditor


|||

## Working with Auditors

### Preparation for the Audit

- **Comprehensive documentation**
    - Ensure all documentation is thorough and up-to-date.
- **Clean and understandable code**
    - Maintain clean, well-commented, and structured code.
- **Working test suite**
    - Develop and maintain a comprehensive test suite to facilitate the auditing process.
- **Define Scope & code freeze**
    - Define a clear scope and stop making changes before the audit

|||

## Working with Auditors

### During the Audit

- **Open Communication** 
    - Keep lines of communication open with auditors.
- **Prompt response to queries**
    - Respond quickly and accurately to auditor inquiries.
- **No changing scope**
    - Not changing the scope during the Audit.

|||

## Working with Auditors

### Post Audit

- **Implementing fixes**
    - Apply recommended fixes promptly.
- **Fix-review or follow-up audit**
    - Conduct a fix-review and/or additional audits to verify the effectiveness of fixes.
- **Bug Bounty program**
    - Set up and maintain a bug bounty program to incentivize the whitehats to report vulnerabilities.
- **Thread Monitoring solution**
    - Implement continuous threat monitoring solutions.

|||

## Working with Auditors

### Continuous Improvement

- **Regularly scheduled audits**
    - Schedule regular audits to keep up with evolving threats.
- **Staying updated with latest security trends and threats**
    - Continuously educate the team about new security trends and threats.

---

## Q&A 

#### Any Questions?
