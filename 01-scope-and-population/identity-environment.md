# Identity Environment

## Platform

Meridian runs Microsoft 365 (E3 and E5 licensing, mixed by role) with Entra ID as the identity
provider for all corporate applications: email, file storage, the internal engineering tools
that support single sign-on, the HRIS, and the customer-facing analytics platform's admin
console. Entra ID is the single source of truth for who can authenticate into company systems.

License mix: most staff hold M365 E3. E5 is assigned to executives, people-managers, and a
subset of IT & Security staff whose role benefits from E5-only features (Defender for
Identity, Privileged Identity Management-adjacent reporting). This is visible in the
`license_type` column of `user-population.csv`.

## Provisioning process (as documented)

1. Hiring manager submits a new-hire request through the HRIS, which is supposed to trigger an
   automated sync (`svc-hris-sync`) that creates the Entra ID account, assigns the correct
   license, and adds the user to their department's baseline security group.
2. IT & Security is expected to manually assign any role-specific access (shared mailboxes,
   application roles, elevated permissions) within the first week, based on a checklist tied
   to job title.
3. New hires are enrolled in MFA during their first login, enforced by a Conditional Access
   policy (see below).

## Deprovisioning process (as documented)

1. People (HR) notifies IT & Security of a termination, same day for involuntary
   terminations, with as much notice as available for voluntary resignations.
2. IT & Security disables the Entra ID account (not deletes, to preserve mailbox and OneDrive
   data for legal hold purposes) and removes the user from all security groups.
3. The disabled account is reviewed after 90 days for final deletion or archival.

Whether this process is actually followed within a reasonable timeframe, and for every
termination, is exactly what the deprovisioning control test in this project checks.

## Conditional Access policies in effect

| Policy name | Applies to | Conditions | Grant control |
|---|---|---|---|
| CA01: Require MFA for all users | All users | Any application, any location | Require multi-factor authentication |
| CA02: Block legacy authentication | All users | Legacy auth clients (POP, IMAP, basic auth) | Block access |
| CA03: Require compliant device for admin roles | Global Administrator, Privileged Role Administrator, Security Administrator, Exchange Administrator, SharePoint Administrator | Any location | Require Microsoft Entra hybrid joined or compliant device |
| CA04: Require MFA for Azure management | Any user accessing the Azure portal, Azure CLI, or Azure PowerShell | Any location | Require multi-factor authentication |
| CA05: Block access from unsupported locations | All users | Sign-in from outside the United States and Australia (Meridian's two office locations) | Block access, with a break-glass exception group |
| CA06: Session sign-in frequency for privileged roles | Global Administrator, Privileged Role Administrator | Any location | Sign-in frequency every 4 hours |
| CA07: Exclude break-glass accounts from all Conditional Access | `bg-admin1`, `bg-admin2` | N/A | Exclude from all other CA policies (by design, per Microsoft's documented break-glass guidance) |

**Note on CA02 (block legacy authentication):** this policy is documented as applying to all
users, but `svc-backup` is recorded as still permitting legacy authentication in
`service-accounts.csv`. That is a real gap between documented policy and actual configuration,
carried forward as a finding in the control testing rather than corrected in the population
data, because the point of this project is to test whether the environment matches its own
documented controls.

## Privileged access model (as documented)

Meridian's IT & Security policy states that standing (always-on) access to Global
Administrator and other tenant-wide roles should be limited to a small number of named,
justified accounts, with Privileged Identity Management (PIM) used for just-in-time elevation
wherever practical, plus two break-glass emergency access accounts excluded from Conditional
Access per Microsoft's own guidance and tested quarterly.

Whether the actual roster of privileged accounts in `privileged-accounts.csv` matches that
policy, in particular the number of standing Global Administrators and whether PIM is
actually in use, is tested in the privileged access management control area.

## Access review process (as documented)

IT & Security is expected to run a quarterly access review of all privileged roles and an
annual review of standard user access, coordinated with department managers to confirm each
person's access still matches their current role. Evidence of whether these reviews actually
happen on schedule, and whether they catch access creep from internal transfers, is tested in
the access reviews and joiner/mover/leaver control areas.
