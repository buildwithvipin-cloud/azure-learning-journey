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

# Lesson 2: Azure Regions, Availability Zones, and Resource Groups

## Learning Objectives

By completing this lesson, you will be able to:

- Explain Azure regions and how to select a deployment region.
- Differentiate zonal, zone-redundant, and regional deployments.
- Design a basic architecture that tolerates an availability-zone failure.
- Explain Azure Resource Groups and their lifecycle behavior.
- Understand resource-group location versus resource location.
- Identify dependencies and risks before moving or deleting resources.
- Apply naming, security, governance, and operational best practices.

---

## 1. Azure Regions

### What is an Azure region?

An Azure region is a geographic area containing Azure infrastructure used to host cloud services. A region can contain multiple datacenters.

When deploying a resource, you generally select a region supported by that service and SKU.

Examples include:

- Central India
- South India
- India South Central
- Southeast Asia
- East US

Always check Microsoft's current region and service availability documentation before designing a production deployment.

### Why does region selection matter?

Region selection affects:

- **Latency:** Network distance and routing influence response times.
- **Data residency:** Organizations may have geographic or regulatory requirements.
- **Service availability:** Not every Azure service or SKU is available in every region.
- **Resilience:** Availability-zone support varies by region and service.
- **Cost:** Prices and cross-region data-transfer costs can differ.
- **Disaster recovery:** A secondary region may be required to recover from a regional outage.

### Region selection checklist

Before selecting a region, verify:

1. Required services and SKUs are available.
2. Availability-zone support meets the workload's requirements.
3. Data residency and compliance requirements are satisfied.
4. Network latency to users and dependent systems is acceptable.
5. Cost and cross-region transfer requirements are understood.
6. Backup and disaster recovery requirements can be met.

**Best practice:** Do not select a region based only on proximity to users. Validate service availability, compliance, resilience, connectivity, and cost.

---

## 2. Azure Availability Zones

### What is an Availability Zone?

An Availability Zone is a physically separate grouping of one or more datacenters within an Azure region.

Zones have independent power, cooling, and networking infrastructure to reduce the likelihood that a localized failure affects all zones.

Availability Zones help protect workloads against zone-level failures. They do not automatically protect against every application failure, data corruption event, or full regional outage.

### Conceptual architecture

```text
Azure Region
|
+-- Availability Zone 1
|   +-- Application instance
|
+-- Availability Zone 2
|   +-- Application instance
|
+-- Availability Zone 3
    +-- Application instance
```

This diagram illustrates a possible architecture. Actual resource placement depends on the Azure service, SKU, region, and deployment configuration.

### Zonal versus zone-redundant deployments

| Feature | Zonal deployment | Zone-redundant deployment |
|---|---|---|
| Placement | A specific availability zone | Multiple zones through the service's supported design |
| Zone-failure protection | Not automatic | Provided for the supported zone-redundant components |
| Failover | Must be designed for the workload | Managed by the service where supported |
| Configuration | Select a zone when the service supports zonal placement | Enable or select zone redundancy when required |
| Operational effort | Can be higher for resilient designs | Often lower for the components covered by the service |
| Important consideration | Additional instances and failover may be required | Availability depends on service capabilities and configuration |

A zonal resource is deployed in a particular zone. To make a workload resilient, you may deploy separate zonal instances in multiple zones and configure traffic routing, application health checks, state management, and data replication.

A zone-redundant service distributes or replicates its components across multiple zones according to its service design. Some services enable this automatically; others require configuration.

**Important:** The number of zones in a region does not necessarily equal the number of zones used by an individual service.

### Real-world example

An online retail application must continue operating if one datacenter fails.

A possible design includes:

- Azure Front Door for global HTTP routing and health-based routing where configured.
- Application instances distributed across multiple supported availability zones.
- A suitable regional routing layer.
- Azure SQL Database configured for zone redundancy when supported by the selected region and service tier.
- Azure Monitor alerts and operational runbooks.

If one zone fails, healthy application instances can continue serving traffic if sufficient capacity remains and routing, health checks, and application dependencies are configured correctly.

For a full regional outage, the organization needs a separate multi-region recovery design. Azure Front Door does not create the secondary application deployment or replicate application data automatically.

### When to use Availability Zones

Use multi-zone designs when the workload requires resilience against a zone failure and the selected region and services support the required configuration.

For mission-critical applications, evaluate whether multi-region disaster recovery is also necessary.

---

## 3. Azure Resource Groups

### What is a Resource Group?

An Azure Resource Group is a logical container for Azure resources that you want to manage together.

Examples of resources include:

- Azure App Service
- Azure SQL Database
- Azure Key Vault
- Application Insights
- Storage accounts

Resource groups help organize deployments, access control, operational responsibilities, and lifecycle management.

### Example hierarchy

```text
Microsoft Entra Tenant
|
+-- Management Group
    |
    +-- Azure Subscription
        |
        +-- rg-retail-prod-app
        |   +-- App Service
        |   +-- Application Insights
        |
        +-- rg-retail-prod-data
        |   +-- Azure SQL Database
        |
        +-- rg-platform-monitoring
            +-- Shared monitoring resources
```

This is an illustrative structure. The appropriate design depends on ownership, security, deployment, and operational requirements.

### Key Resource Group characteristics

- A resource group belongs to one Azure subscription.
- A resource belongs to one resource group at a time.
- Supported resources in one resource group can be deployed in different Azure regions.
- A resource group has a location for its metadata and control-plane operations.
- Resources can have dependencies on other resources in the same or different resource groups.
- Deleting a resource group initiates deletion of its contained resources, subject to service behavior, locks, and dependency constraints.

### Resource Group location versus resource location

These are different concepts.

**Resource Group location:** Determines where the resource group's metadata is stored and is relevant to Azure Resource Manager control-plane operations.

**Resource location:** Determines where an individual Azure resource is deployed, subject to that service's location rules.

For example, a resource group with metadata in Central India can contain supported resources deployed in Central India and another region.

The resource group's location does not automatically determine where every resource or its data is stored.

---

## 4. Resource Group Lifecycle Design

### Group resources by lifecycle

A useful design principle is to place resources together when they share deployment, management, and deletion lifecycles.

Consider a production application:

| Resource Group | Example contents | Lifecycle |
|---|---|---|
| rg-retail-dev-app | Development application resources | Development changes |
| rg-retail-prod-app | Production application resources | Application releases |
| rg-retail-prod-data | Production database resources | Data lifecycle and database changes |
| rg-platform-monitoring | Shared monitoring resources | Platform lifecycle |

These are example groupings, not mandatory rules. Separate resources according to actual dependencies, security boundaries, ownership, and operational needs.

### Why separate application and database resources?

Suppose the application is redeployed regularly, but the database contains important customer orders.

Keeping the database in a separate resource group can reduce the risk that an application cleanup operation deletes the database.

However, separate resource groups alone do not guarantee data protection. Use backups, appropriate permissions, deletion safeguards, and tested recovery procedures.

### What happens when a Resource Group is deleted?

Deleting a resource group is an irreversible operation for the group itself. Azure Resource Manager attempts to delete the resources it contains.

Some individual resource types support separate recovery mechanisms, but recovery is not guaranteed.

Before deleting a resource group:

- Confirm the tenant, subscription, and resource group.
- Review all contained resources.
- Identify dependencies on other resource groups.
- Verify backups and retention requirements.
- Check resource locks and applicable deletion protections.
- Obtain the required approval.
- Confirm the exact target before executing the operation.

**Never use an unreviewed cleanup script against production resources.**

---

## 5. Resource Dependencies and Resource Moves

Resources can depend on one another even when they are located in separate resource groups.

Example:

```text
rg-retail-prod-app
|
+-- App Service
      |
      +-- Connects to Azure SQL Database
          in rg-retail-prod-data
```

The application may depend on database connectivity, DNS, managed identities, role assignments, private endpoints, firewall rules, and connection settings.

Before moving or deleting resources, review:

1. Which applications or services depend on the resource?
2. Are resource IDs referenced by other configurations?
3. Are private endpoints, DNS records, or role assignments affected?
4. Does the resource type support the intended move?
5. Must dependent resources move together?
6. Is downtime expected?
7. Are deployment templates and state files updated where necessary?

Resource-move support and restrictions vary by resource type. Check Microsoft's documentation before attempting a move.

---

## 6. Naming Standards and Governance

Use a consistent naming standard that communicates the workload, environment, purpose, region, and sequence where appropriate.

Example:

`rg-retail-prod-app-cin-001`

Illustrative components:

- `rg`: Resource Group
- `retail`: Workload
- `prod`: Environment
- `app`: Purpose
- `cin`: Organization-defined region abbreviation
- `001`: Sequence number

Define and document region abbreviations rather than assuming every team uses the same convention.

### Governance best practices

- Separate production and non-production workloads.
- Use Azure RBAC and least privilege.
- Apply Azure Policy at appropriate management-group, subscription, or resource-group scopes.
- Use tags for ownership, environment, cost center, and workload classification.
- Do not assume resource-group tags automatically propagate to resources.
- Use Azure Policy or another governed mechanism when tag inheritance is required.
- Use Terraform, Bicep, or ARM templates for repeatable deployments.
- Apply resource locks to critical resources where appropriate.
- Monitor deployment and resource-management activities.
- Review permissions and deletion safeguards periodically.

---

## 7. Practical Exercises

### Exercise 1: Inspect Azure Regions

1. Open the Azure portal.
2. Review the list of available regions.
3. Select a service you use regularly.
4. Check whether the required SKU is available in your intended region.
5. Check the service's availability-zone support.
6. Record the region and service constraints in your notes.

**Expected result:** You can explain why a region is suitable for a particular workload.

### Exercise 2: Inspect a Resource Group

1. Open the Azure portal.
2. Navigate to Resource groups.
3. Select a non-production resource group you are authorized to inspect.
4. Record its subscription and location.
5. Review its resources and their individual locations.
6. Identify the application's dependencies.
7. Record the naming convention and available tags.

**Expected result:** You can distinguish resource-group metadata location from individual resource deployment locations.

### Exercise 3: Design a production resource-group structure

Design a resource-group structure for an online application with:

- A web application
- A production database
- A Key Vault
- Monitoring resources
- A development environment

Document:

- Resource group names
- Resource ownership
- Lifecycle and deployment boundaries
- Required RBAC scopes
- Dependencies
- Deletion safeguards
- Backup and recovery considerations

**Expected result:** A design that supports operational management while reducing accidental deletion risk.

### Exercise 4: Review zone resilience

For a production application:

1. Identify the application components.
2. Check which components support availability zones.
3. Decide which resources need zonal or zone-redundant configurations.
4. Identify single points of failure.
5. Document how routing and data recovery would work during a zone failure.
6. Identify whether a secondary region is required.

**Expected result:** A basic zone-resilient design with known limitations and recovery requirements.

---

## 8. Knowledge Check and Answers

### Question 1: Region selection

A production application requires a specific Azure service and protection against a datacenter-level failure. What should the architect verify first?

**Answer:** Verify service and SKU availability in the intended region, including availability-zone support, before designing the resilient deployment.

### Question 2: Zonal deployment

A team deploys one VM in Zone 1. Will Azure automatically move that VM to Zone 2 if Zone 1 fails?

**Answer:** No. A single zonal VM is not automatically zone-resilient. The team must design redundant instances and appropriate routing and recovery.

### Question 3: Resource Group location

Does a Resource Group's location force every resource it contains to be deployed in the same region?

**Answer:** No. The Resource Group location applies to its metadata and control-plane operations. Individual resources have their own location rules.

### Question 4: Resource lifecycle

An application is redeployed frequently, but its production database must be preserved. What is a sensible design improvement?

**Answer:** Consider separate application and database resource groups based on their lifecycles, and ensure the deployment process targets only the intended application resources.

### Question 5: Resource Group deletion

What is the main risk of deleting a production Resource Group?

**Answer:** Azure Resource Manager attempts to delete the contained resources. The deletion is irreversible for the Resource Group, so dependencies, backups, permissions, and recovery requirements must be reviewed beforehand.

### Question 6: Multi-region recovery

Do Availability Zones automatically protect an application against a complete regional outage?

**Answer:** No. Regional recovery requires an appropriately designed secondary-region deployment, data replication or restore strategy, traffic failover, and recovery testing.

---

## 9. Common Mistakes

- Assuming every Azure service supports Availability Zones in every region.
- Assuming one zonal VM automatically fails over to another zone.
- Treating zone redundancy as a complete disaster recovery solution.
- Confusing Resource Group location with resource location.
- Putting long-lived data resources in the same deletion lifecycle as frequently redeployed application resources.
- Assuming separate resource groups eliminate dependencies.
- Deleting resource groups without reviewing their contents.
- Assuming tags automatically propagate from a Resource Group.
- Moving resources without checking service-specific restrictions.
- Relying on resource locks without maintaining backups and tested recovery procedures.

---

## 10. Revision Summary

- **Region:** Geographic area hosting Azure infrastructure and services.
- **Availability Zone:** Isolated failure domain within a supported region.
- **Zonal deployment:** Resource deployed to a specific zone.
- **Zone-redundant deployment:** Supported service design distributes or replicates components across multiple zones.
- **Resource Group:** Logical container for resources within one subscription.
- **Resource Group location:** Location for group metadata and control-plane operations.
- **Resource location:** Deployment location of an individual resource.
- **Lifecycle-based grouping:** Organize resources according to deployment, management, and deletion requirements.
- **Zone resilience:** Requires suitable service configuration and a workload design that tolerates zone failure.
- **Regional disaster recovery:** Requires additional planning beyond Availability Zones.

---

## 11. Official Microsoft Learn References

1. [Azure regions list and availability-zone support](https://learn.microsoft.com/en-us/azure/reliability/regions-list)
2. [What are Azure Availability Zones?](https://learn.microsoft.com/en-us/azure/availability-zones/az-overview)
3. [Zonal resources and zone resiliency](https://learn.microsoft.com/en-us/azure/reliability/availability-zones-zonal-resource-resiliency)
4. [Azure Resource Manager overview](https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/overview)
5. [Resource Group and resource deletion](https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/delete-resource-group)
6. [Resource Group and resource management FAQ](https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/frequently-asked-questions)
7. [Move Azure resources](https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/move-resources-overview)

---

## Lesson Status

- [x] Lesson 2 concepts reviewed
- [ ] Practical exercises completed
- [ ] Knowledge check revised independently
- [ ] README committed to GitHub
- [ ] Lesson 2 reviewed after commit

