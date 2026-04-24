# Risk Acceptance Document  

## Document Header  

| Field | Value |  
|-------|-------|  
| Risk ID | RA-2026-001 |  
| System/Asset | Online Banking System (XYZ Group of Companies) |  
| Risk Owner | Head of IT |  
| Prepared By | Manish Shrestha |  
| Date | 2026-04-24 |  
| Review Date | 2026-10-24 |  
| Version | 1.0 |  

---  

## 1. Executive Summary  

The organization is currently operating an online banking platform without Multi-Factor Authentication (MFA), increasing the risk of unauthorized access. Due to cost and integration constraints, full remediation is delayed.  

**Recommendation:**  
- [ ] Option A: Implement full MFA immediately  
- [x] Option B: Implement partial MFA for high-risk users  
- [ ] Option C: Accept risk fully  
- [ ] Option D: Accept with minimal controls  

---  

## 2. Risk Description  

### 2.1 What Could Happen  

Unauthorized users may gain access to customer accounts and perform fraudulent transactions.  

| Scenario | Description |  
|----------|-------------|  
| Scenario 1 | Customer accounts compromised via password theft |  
| Scenario 2 | Internal staff misuse access privileges |  
| Scenario 3 | Automated credential stuffing attack |  

### 2.2 How It Could Happen  

1. Weak or reused passwords  
2. Phishing attacks targeting customers  
3. Lack of secondary authentication controls  

### 2.3 Why We Might Not Know  

Limited real-time monitoring and absence of anomaly detection tools delay detection.  

---  

## 3. Risk Assessment  

### 3.1 Likelihood Assessment  

| Factor | Assessment | Evidence |  
|--------|------------|----------|  
| Threat Actor Interest | High | Financial systems are prime targets |  
| Attack Complexity | Low | Widely available attack tools |  
| Detection Capability | Medium | Basic logging exists |  
| Historical Incidents | 2 | Past phishing-related cases |  

**Overall Likelihood:** High  

**Basis:**  
High-value target with weak authentication controls increases probability of attack.  

### 3.2 Impact Assessment  

| Impact Category | Potential Consequence | Estimated Cost |  
|-----------------|----------------------|----------------|  
| Data Breach | Exposure of customer data | $50,000 |  
| Financial Loss | Fraudulent transactions | $100,000 |  
| Regulatory Action | NRB penalties | $30,000 |  
| Reputation | Loss of customer trust | $80,000 |  
| Business Disruption | Temporary service shutdown | $20,000 |  

**Total Potential Impact:** $100,000 - $280,000 range  
**Most Likely Scenario:** $150,000  

### 3.3 Risk Rating  

| Likelihood \ Impact | Low | Medium | High | Critical |
|---------------------|-----|--------|------|----------|
| High                | M   | H      | H    | C        |
| Medium              | L   | M      | H    | H        |
| Low                 | L   | L      | M    | H        |

**Current Risk Position:** HIGH

---  

## 4. Existing Controls Assessment  

### 4.1 Controls Currently in Place  

| Control | Description | Effectiveness | Honest Assessment |  
|---------|-------------|---------------|-------------------|  
| Password Policy | Minimum complexity rules | Weak | Users reuse passwords |  
| Firewall | Basic perimeter protection | Adequate | Does not stop credential attacks |  
| Logging | System activity logs | Weak | No active monitoring |  

### 4.2 Controls NOT in Place  

| Control | Why Missing | Impact |  
|---------|-------------|--------|  
| Multi-Factor Authentication | Budget constraints | High risk exposure |  
| SIEM Monitoring | No dedicated team | Delayed detection |  

### 4.3 Overall Control Effectiveness  

Current controls are insufficient to prevent modern attacks.  

**Rating:** [ ] Strong [ ] Adequate [x] Weak [ ] Insufficient  

---  

## 5. Options Analysis  

### Option A: Full Remediation  

| Aspect | Detail |  
|--------|--------|  
| Description | Implement MFA across all users |  
| Actions Required | Deploy MFA system, user training |  
| Cost | $60,000 |  
| Timeline | 3 months |  
| Residual Risk | Low |  
| Pros | Strong security |  
| Cons | High cost, user resistance |  

### Option B: Partial Remediation  

| Aspect | Detail |  
|--------|--------|  
| Description | MFA for high-risk users only |  
| Actions Required | Identify users, deploy limited MFA |  
| Cost | $25,000 |  
| Timeline | 1.5 months |  
| Residual Risk | Medium |  
| Pros | Cost-effective, quick |  
| Cons | Not full coverage |  

### Option C: Accept Risk  

| Aspect | Detail |  
|--------|--------|  
| Description | No changes implemented |  
| Actions Required | None |  
| Direct Cost | $0 |  
| Potential Cost | $150,000 |  
| Residual Risk | High |  
| Pros | No cost |  
| Cons | High exposure |  

### Option D: Accept with Minimal Improvements  

| Aspect | Detail |  
|--------|--------|  
| Description | Improve password policies only |  
| Actions Required | Enforce stricter rules |  
| Cost | $5,000 |  
| Timeline | 2 weeks |  
| Residual Risk | Med-High |  
| Pros | Quick implementation |  
| Cons | Weak mitigation |  

### Options Comparison  

| Factor | Option A | Option B | Option C | Option D |  
|--------|----------|----------|----------|----------|  
| Direct Cost | High | Medium | None | Low |  
| Timeline | Long | Medium | None | Short |  
| Residual Risk | Low | Medium | High | Med-High |  
| Compliance Impact | Strong | Moderate | Weak | Weak |  
| Business Impact | Medium | Low | High | Medium |  

---  

## 6. Recommendation  

### 6.1 Selected Option  
Option B: Partial Remediation  

### 6.2 Rationale  

1. Balances cost and security improvement  
2. Addresses highest-risk users first  
3. Enables phased future implementation  

### 6.3 Immediate Actions  

- [x] Identify high-risk users  
- [x] Procure MFA solution  
- [ ] Begin deployment  

### 6.4 Future Commitments  

| Action | Timeline | Owner | Budget |  
|--------|----------|-------|--------|  
| Full MFA rollout | 6 months | IT Head | $35,000 |  
| SIEM implementation | 9 months | Security Team | $20,000 |  

---  

## 7. Residual Risk Acknowledgment  

| Residual Risk | Description | Why Accepted |  
|---------------|-------------|--------------|  
| Partial user exposure | Non-MFA users still vulnerable | Budget limitation |  

### 7.1 What We Will NOT Be Able to Do if Incident Occurs  

1. Prevent all unauthorized access  
2. Detect attacks in real-time  
3. Fully avoid financial loss  

---  

## 8. Conditions and Validity  

### 8.1 This Acceptance is Valid Only If:  
- [x] MFA implemented for high-risk users  
- [x] Logs are reviewed weekly  
- [x] No major incidents occur  

### 8.2 This Acceptance Expires On:  
2026-10-24  

### 8.3 Re-evaluation Triggers  

- [x] Security incident occurs  
- [x] Regulatory requirement changes  
- [x] System upgrade  

---  

## 9. Compliance Implications  

| Framework | Requirement | Impact of Acceptance |  
|-----------|-------------|---------------------|  
| NRB Guidelines | Strong authentication | Partial non-compliance |  
| ISO 27001 | Access control | Gap exists |  

---  

## 10. Signatures  

### Risk Owner (Business)  

| Field | Value |  
|-------|-------|  
| Name | |  
| Title | Head of IT |  
| Date | |  
| Signature | |  

### Security Review  

| Field | Value |  
|-------|-------|  
| Name | |  
| Title | Security Manager |  
| Date | |  
| Signature | |  

### Compliance Review  

| Field | Value |  
|-------|-------|  
| Name | |  
| Title | Compliance Officer |  
| Date | |  
| Signature | |  

### Executive Approval (if required)  

| Field | Value |  
|-------|-------|  
| Name | |  
| Title | CEO |  
| Date | |  
| Signature | |  

---  

## Version History  

| Version | Date | Author | Changes |  
|---------|------|--------|---------|  
| 1.0 | 2026-04-24 | Manish Shrestha | Initial document |  

---  

## Appendices  

### A. Technical Details  
MFA solution based on OTP and mobile authentication.  

### B. Reference Documents  

| Document | Location |  
|----------|----------|  
| Security Policy | Internal Repository |  

### C. Meeting Minutes  
Risk discussed in IT Governance Committee meeting (April 2026).  
