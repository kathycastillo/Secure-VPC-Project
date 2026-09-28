# Risk Assessment: Secure VPC Architecture Project

**Author:** Kathy Castillo
**Scope:** AWS VPC (us-east-2) with one public bastion host and one private server

## 1. System Description

A custom VPC with a public subnet (Public Admin Server, used as a bastion host) and a private subnet (Private Test Server, no public IP). Access is controlled by two security groups and key-based SSH authentication.

## 2. Assets

| Asset | Why it matters |
|---|---|
| Private SSH key (secure-vpc-key) | Grants access to both servers |
| Public Admin Server (bastion) | Only internet-facing entry point |
| Private Test Server | Protected internal system |
| AWS account | Controls all resources and billing |

## 3. Threats, Risks, and Controls

Likelihood and impact are rated Low / Medium / High.

| # | Risk | Likelihood | Impact | Current control | Recommended control |
|---|---|---|---|---|---|
| 1 | Private key copied onto the bastion is stolen if the bastion is compromised | Medium | High | Key-based auth only | Keep key on local machine; use SSH ProxyJump or agent forwarding |
| 2 | Bastion SSH is exposed to the internet and targeted by brute force or exploits | Medium | High | SSH restricted to my IP; no password login | Patching, fail2ban, or AWS Systems Manager Session Manager (no open port 22) |
| 3 | My home IP changes and I open SSH to 0.0.0.0/0 to regain access | Medium | High | None | Update the rule to the new IP; never use 0.0.0.0/0 |
| 4 | Single bastion is a single point of failure and single point of attack | Low | Medium | None | Second bastion or Session Manager |
| 5 | Private server has no outbound internet, so it cannot receive patches | Medium | Medium | Isolation by design | NAT gateway or VPC endpoints (weigh cost) |
| 6 | No logging, so attempted access goes unnoticed | Medium | Medium | None | Enable VPC Flow Logs and CloudTrail |
| 7 | Lost or leaked key file (for example, committed to GitHub) | Low | High | None | Rotate the key pair if exposed; use .gitignore for *.pem |

## 4. Summary

The design meets its goal of isolating the private server behind a bastion host. The highest-priority finding is risk #1: copying the key to the bastion. Next priorities are adding logging (#6) and reducing the bastion's exposure (#2).

## 5. Recommended Next Steps

1. Switch to ProxyJump so the key stays local.
2. Enable VPC Flow Logs and CloudTrail.
3. Evaluate Session Manager to eliminate open SSH.
4. Add a NAT gateway or VPC endpoints if the private server needs updates.
