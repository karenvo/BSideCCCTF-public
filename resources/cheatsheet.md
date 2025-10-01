# Cheatsheet

## IMPORTANT
I'd like to note that the TimeGenerated field for Log Analytics Workspace is not accurate. Please refrain from using this to correlate data.


## Assigned Teams

** = Team Leader

| CyberTeam 1   | CyberTeam 2      | CyberTeam 3      |
| ------------- | ---------------- | ---------------- |
| Lupe **       | Dylan Golden **  | Thomas Y **      |
| Mason Webb    | Ben Heng         | Jacob Valdivia   |
| Tommy Yang    | Luke Kimes       | Luke Gutierrez   |
| Chris Duncan  | Dien Nguyen      | Vera Grayeb      |
| Justin Emig   | Jennifer Chandoo | Grayson Gardnier |
| Abi           | Piunik           | Dat Nguyen       |
|               | Devin            | Nancy C          |


## Links
| Application Name      | URL                       | Purpose                                            |
| --------------------- | ------------------------- |  -------------------------------------------------- |
| Microsoft Entra       | entra.microsoft.com       | Entra for users                                    |
| Microsoft Defender    | security.microsoft.com    |                                                    |
| Azure Portal          | portal.azure.com          | Locate your azure resources, such as Log Analytics |
| Microsoft 365         | M365.cloud.microsoft.com  | m365 resources such as excel, word, etc            |
| Login                 | login.microsoftonline.com | First step of OAuth at Microsoft                   |
| Purview               | purview.microsoft.com     | ex. audits logs for passwords                      |
| Exchange Admin Center | exchange.microsoft.com    | Mail                                               |

[Log Analytics Workspace](https://portal.azure.com/#@churchofmemeology.com/resource/subscriptions/96159d73-8c62-4276-8abc-46ccd8071a6d/resourceGroups/BsideCCCTF/providers/Microsoft.OperationalInsights/workspaces/Logs4Days/logs)


## Instructions

### Create Your Teams First
1. Log into TryHackMe
2. Go into your profile
3. Add your team members, this is how we'll be keeping score between teams.
4. Message me for your creds.

### Log Analytics Workspace
1. Log into Azure Portal
2. Type "Log Analytics Workspace"
3. Click into "Logs4Days"
4. Navigate to "Custom Logs"


## Terms and Definitions
### Types of Logs
> For general use and info, I found some really helpful resources here: 
[Azure Monitor Log Tables](https://github.com/MicrosoftDocs/azure-monitor-docs/tree/4c3ba1d87b9a4ba2da97e04f861a77b340637051/articles/azure-monitor/reference/tables)


**AADNonInteractiveSignInLogs_CL**
: Sign in logs where users are not interacting with a page, etc. 

**SignInLogs_CL**
: Sign in logs for all users and applications. Also known as "interactive sign in".

**AADManagedIdentitySignInLogs_CL**
: These logs capture sign-in events for Azure Active Directory managed identities, typically used by applications or services to authenticate securely without credentials. They help track usage, failures, and access patterns of managed identities across your Azure environment.

**MicrosoftAzureBastionAuditLogs_CL**
: These logs record audit events for Azure Bastion, a secure service that provides RDP/SSH access to virtual machines without exposing them to the public internet. They include session start/stop times, user identities, and connection metadata for compliance and security monitoring.


| Feature    | AADServicePrincipalSignInLogs_CL  | MicrosoftServicePrincipalSignInLogs_CL |
| ---------- | --------------------------------- | -------------------------------------- |
| Tracks     | All service principals            | Microsoft-owned service principals     |
| Includes   | Custom apps, third-party apps     | Microsoft Graph, Azure services        |
| Use Case   | Security auditing, app monitoring | Service-to-service diagnostics         |
| Log Source | Entra ID sign-in logs             | Entra ID diagnostic settings           |

**AADServicePrincipalSignInLogs_CL**
: AADServicePrincipalSignInLogs is a log table in Microsoft Entra ID (formerly Azure Active Directory) that records sign-in activities by service principals, which are non-user identities used by applications, services, or automation tools to access resources.

**MicrosoftServicePrincipalSignInLogs_CL**
: These logs detail sign-in activity for service principals, which are identities used by applications, automation scripts, or services to access Azure resources. They help monitor authentication attempts, failures, and access behavior of non-human identities.

**MicrosoftGraphActivityLogs_CL**
:These logs provide a detailed audit trail of Microsoft Graph API calls made within your tenant, including request URIs, response codes, and caller identities. They are essential for tracking API usage, troubleshooting issues, and ensuring secure access to Microsoft 365 data.

~~**OfficeActivity**~~
: ~~These logs are apart of Microsoft Purview Audit solutions. The table provides metadata with context to user actions, actor/target details, ip addresses and device info, application and service context, meeting and chat details, sensitivity labels and sharing permissions.~~

-------------------------

## KQL
| Function / Operator | Purpose                                        | Example Usage                                     | Notes                                      |
| ------------------- | ---------------------------------------------- | ------------------------------------------------- | ------------------------------------------ |
| where               | Filters rows based on a condition              | where servicePrincipalName contains "GraphReader" | Similar to SQL WHERE                       |
| project             | Selects specific columns                       | project uniqueTokenIdentifier, servicePrincipalId | Like SQL SELECT                            |
| join kind=inner     | Combines rows from two tables where keys match | join kind=inner (...) on servicePrincipalId       | Use leftouter, inner, rightouter as needed |
| distinct            | Returns unique values                          | distinct uniqueTokenIdentifier                    | Removes duplicates                         |
| summarize           | Aggregates data                                | summarize count() by servicePrincipalName         | Useful for grouping                        |
| extend              | Adds calculated columns                        | extend tokenAge = now() - tokenIssuedTime         | Like SQL SELECT ... AS                     |
| order by            | Sorts result                                   | sorder by timestamp desc                          | Use asc or desc                            |
| top                 | Returns top N rows                             | top 10 by timestamp                               | Often used with order by                   |
| parse_json()        | Parses JSON strings into structured data       | parse_json(properties).clientAppUsed              | Useful for nested fields                   |
| project-away        | Removes columns                                | project-away tenantId                             | Opposite of project                        |