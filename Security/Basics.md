# Risk Analysis
**threat**: possibility of damage
**vulnerability**: weakness in the system
**attack**: exploitation of vulnerability to realize a threat
**countermeasure**: disable attacks, remove vulnerabilities or mitigate threats
## Security Properties: CIA
**Confidentiality**: info is disclosed to legitimate users: AES, 3DES, RSA
**Integrity**: info is created and modified by legitimate users: MD5, SHA series
	**Authenticity**: user is legitimate: PSK Pre-Shared Key, RSA
**Availability**: info is accessible to legitimate users
## Authentication

> The process of knowing who is the interactor

**knowledge factors**: something you know
**possession factors**: something you have
**inherence factors**: something you are
  
SFA single factor authentication
2FA two(multi)-factor authentication: use multiple **distinct** factors
## Quantitative Terms
Exposure Factor EF: Percentage of asset loss
Single Loss Expectancy SLE: EF \* Asset Value 
Annualized Rate of Occurrence ARO: Estimated frequency a threat will occur within a year
Annualized Loss Expectancy ALE: SLE \* ARO
## Access Control
**Permissions/Access Control List ACL**
R read W write E execute

> **Bell-Lapadula Model:** no read up (cannot read anything from above your level), no write down (cannot disclose anything to below your level)
## Risk Management
Risk Reduction: avoid the risk
Risk Mitigation: damage control
Risk Transference: through insurance and user warning
Risk Acceptance
# Cyber Attacks
Social Engineering: Phishing, Pretexting
Exploit: Buffer Overflow(Bounds Checking), Code Injection
Brute Force