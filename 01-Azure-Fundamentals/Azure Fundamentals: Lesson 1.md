# Azure Fundamentals: Lesson 1
## Understanding the Azure Resource Hierarchy

## 1. Learning Objectives

By completing this lesson, I should be able to:

- Explain Microsoft Entra tenants and Azure resource hierarchy.
- Understand management groups, subscriptions, resource groups, and resources.
- Describe how governance and access control work at different scopes.
- Design a basic enterprise subscription structure.
- Explain how Azure Policy supports centralized governance.

## 2. Azure Resource Hierarchy

Azure uses a hierarchy to organize and govern cloud resources.

```text
Microsoft Entra Tenant
        |
        v
Tenant Root Management Group
        |
        v
Child Management Groups
        |
        v
Subscriptions
        |
        v
Resource Groups
        |
        v
Azure Resources
```

**Important:** The Microsoft Entra tenant is the identity directory associated with the Azure management hierarchy. The management group hierarchy begins at the tenant root management group. The tenant is not itself a parent management group.

## 3. Microsoft Entra Tenant

### What is it?

A Microsoft Entra tenant is a dedicated identity and directory boundary for an organization.

It contains directory objects such as:

- Users and groups
- Application registrations and service principals
- Managed identities and other directory objects

### Why is it used?

- Authenticate users and applications.
- Manage identities and access.
- Integrate with Azure role-based access control (RBAC).
- Support identity governance and security.

### Example

Employees sign in using their corporate identities. Azure administrators receive appropriate permissions to manage subscriptions and resources.

**Key point:** A tenant is primarily an identity boundary, not a resource container.

## 4. Management Groups

### What are they?

Management groups are containers used to organize multiple Azure subscriptions into a hierarchy.

### Why are they used?

- Apply Azure Policy assignments across subscriptions.
- Assign Azure RBAC roles at an appropriate scope.
- Organize subscriptions by environment, business unit, or governance requirement.
- Inherit applicable policy assignments and role assignments through the hierarchy.

### Example

A company can create the following management groups:

```text
Tenant Root Management Group
|
+-- Production
|   +-- Production Subscription 01
|   +-- Production Subscription 02
|
+-- NonProduction
|   +-- Development Subscription
|   +-- Testing Subscription
|
+-- Sandbox
    +-- Sandbox Subscription
```

**Key point:** A subscription belongs to one management group at a time within the management group hierarchy. Subscriptions can be moved between management groups when permissions and governance requirements allow.

## 5. Azure Subscriptions

### What is a subscription?

An Azure subscription is a boundary used for resource management, access control, cost tracking, and service quotas.

### Why use multiple subscriptions?

- Separate production and non-production workloads.
- Isolate workloads for access control and governance.
- Track costs for different business units or applications.
- Manage quotas and resource deployment limits.
- Reduce the impact of operational mistakes across environments.

### Example

A company may use:

- `sub-production`
- `sub-development`
- `sub-testing`
- `sub-shared-services`

The appropriate design depends on security boundaries, billing arrangements, governance, operational ownership, and service limits.

**Key point:** A subscription is not the same as a Microsoft Entra tenant. Multiple subscriptions can be associated with the same tenant.

## 6. Resource Groups

### What is a resource group?

A resource group is a logical container for Azure resources.

Every Azure resource that supports the Azure Resource Manager model belongs to a resource group.

### Why use resource groups?

- Organize resources by application or workload.
- Manage resource lifecycle and deployments.
- Apply RBAC and Azure Policy at resource-group scope.
- Simplify operational ownership and resource management.

### Example

A payment application could use:

| Resource group | Example resources |
|---|---|
| `rg-payment-prod` | App Service, Key Vault, Application Insights |
| `rg-network-prod` | Virtual network, subnets, network security groups |
| `rg-data-prod` | Azure SQL Database, storage account |

A resource group is a logical grouping, not a physical network or security boundary.

Resources in a resource group can be located in different Azure regions. However, a resource group has its own location for storing its metadata.

When deleting a resource group, Azure generally deletes the resources it contains. Check dependencies and deletion behavior before performing destructive operations.

## 7. Azure Resources

### What is a resource?

An Azure resource is an individual instance of an Azure service.

Examples include:

- Virtual machine
- Virtual network
- Storage account
- Azure SQL Database
- App Service
- Key Vault
- Log Analytics workspace

### Example

A production payment application may use:

- `app-payment-prod` for the application
- `vnet-payment-prod` for network connectivity
- `kv-payment-prod` for secrets
- `sql-payment-prod` for application data

Names are illustrative. Actual naming must follow organizational standards and Azure service naming rules.

## 8. Enterprise Architecture Example

### Business requirement

An enterprise operates several applications across production, development, testing, and shared infrastructure. It needs centralized governance, separate operational ownership, and cost visibility.

### Proposed design

```text
Microsoft Entra Tenant
|
+-- Tenant Root Management Group
    |
    +-- Production Management Group
    |   +-- Production Subscription
    |
    +-- NonProduction Management Group
    |   +-- Development Subscription
    |   +-- Testing Subscription
    |
    +-- Shared Services Management Group
    |   +-- Shared Services Subscription
    |
    +-- Sandbox Management Group
        +-- Sandbox Subscription
```

### Governance approach

- Assign approved-region Azure Policy initiatives at suitable management group scopes.
- Apply RBAC according to least-privilege principles.
- Use separate subscriptions where workload isolation and ownership justify them.
- Apply consistent naming, tagging, monitoring, and cost-management practices.
- Review policy exemptions and inherited permissions regularly.

This is an example architecture, not a mandatory Azure structure. Management groups should reflect governance requirements rather than creating unnecessary complexity.

## 9. Knowledge Check: Centralized Governance

### Scenario

An organization has 50 Azure subscriptions. Management wants to restrict deployments to approved Azure regions across all subscriptions.

### Correct answer

**Place the subscriptions under an appropriate management group and assign an Azure Policy initiative at that scope.**

### Why is this correct?

Management groups support governance across multiple subscriptions. An Azure Policy initiative groups related policy definitions, and assigning it at an appropriate management group scope can enforce or audit regional restrictions across eligible resources in descendant subscriptions.

The exact outcome depends on the policy definitions, assignment parameters, exemptions, and enforcement mode.

### Why the other options are incorrect

- **Configure policy on every VM:** This is incomplete because regional restrictions must cover supported resource types, not just virtual machines.
- **Create one resource group for all subscriptions:** Resource groups contain resources, not subscriptions.
- **Create a tenant for every subscription:** This introduces unnecessary identity complexity and does not solve centralized governance within a shared tenant.

## 10. Practical Exercises

- [ ] Explain the difference between a tenant and a management group.
- [ ] Draw an enterprise hierarchy with production and non-production subscriptions.
- [ ] Identify three reasons to use multiple subscriptions.
- [ ] Explain why resource groups are logical containers rather than physical network boundaries.
- [ ] Describe how an Azure Policy assignment can govern multiple subscriptions.
- [ ] Explore the resource hierarchy in the Azure portal, if access is available.

## 11. Common Mistakes

1. Treating a Microsoft Entra tenant as a management group.
2. Assuming subscriptions are contained inside resource groups.
3. Assuming resource groups must contain resources from only one region.
4. Using too many subscriptions or management groups without a clear governance reason.
5. Granting excessive RBAC permissions at broad scopes.
6. Assuming an Azure Policy assignment automatically guarantees compliance in every situation without checking its definition, parameters, exemptions, and enforcement behavior.

## 12. Revision Summary

| Component | Primary purpose |
|---|---|
| Microsoft Entra tenant | Identity and directory management |
| Management group | Organize subscriptions and apply centralized governance |
| Subscription | Resource management, access control, cost tracking, and quotas |
| Resource group | Logically group and manage related resources |
| Resource | Individual Azure service instance |

## 13. Official Microsoft References

-Azure subscription design	                 [Subscription considerations and recommendations](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ready/landing-zone/design-area/resource-org-subscriptions?utm_source=chatgpt.com)
-Management groups and resource organization	 [Resource organization](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ready/landing-zone/design-area/resource-org?utm_source=chatgpt.com)
-Landing zone design areas	                 [Azure landing zone design areas](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ready/landing-zone/design-areas?utm_source=chatgpt.com)
-Azure Policy	                                 [Azure Policy overview](https://learn.microsoft.com/en-us/azure/governance/policy/overview?utm_source=chatgpt.com)
-Resource groups and Azure Resource Manager	 [Azure Resource Manager overview
](https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/overview?utm_source=chatgpt.com)
## Lesson Status

- [x] Lesson notes prepared
- [x] Enterprise example documented
- [x] Knowledge check explained
- [ ] Notes committed to GitHub
- [ ] Practical exercises completed
