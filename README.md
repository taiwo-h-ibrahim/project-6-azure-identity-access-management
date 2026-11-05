# Project 6: Azure Identity and Access Management — Group-Based RBAC, Least Privilege and B2B External Access

**Author:** Taiwo H Ibrahim  
**Platform:** Microsoft Azure  
**Identity service:** Microsoft Entra ID  
**Access control:** Azure Role-Based Access Control (Azure RBAC)  
**Project type:** Practical Cloud Security Lab

## 1. Project Overview

This project demonstrates practical identity and access management controls in Microsoft Azure. It began with a dedicated Microsoft Entra ID analyst account, a security group, and an Azure RBAC role assignment to explore group-based access and least-privilege authorization.

The project has now been extended with a separate **B2B External Guest RBAC** exercise. The extension demonstrates least-privilege access for an externally authenticated Gmail identity to a controlled Azure Storage resource, including successful read access and denied write/delete operations.

The original single-tenant exercise and its evidence remain unchanged. The B2B work is documented as a separate extension under the project evidence directory.

## 2. Objectives

### Core Azure RBAC exercise

- Create a Microsoft Entra ID security group for cloud security analysts.
- Create a dedicated analyst user account and add it to the group.
- Configure a group-based Azure RBAC assignment.
- Examine the analyst's effective subscription permissions.
- Review Azure Activity Log and Entra sign-in records.
- Inspect the analyst's available role-management controls in the subscription IAM portal.
- Test a permitted read operation and a resource-creation operation.
- Document the observed results and the limits of the evidence.

### B2B External Guest RBAC extension

- Create and validate an external B2B guest identity.
- Complete the guest invitation redemption flow.
- Confirm the guest can authenticate to the resource tenant.
- Assign least-privilege Azure RBAC permissions at storage-account scope.
- Demonstrate an allowed Blob read operation.
- Demonstrate that Blob write and delete operations are denied.
- Document the observed portal and CLI behavior without generalizing the result beyond the tested configuration.

## 3. Lab Architecture

The core implementation uses one Microsoft Entra tenant and one Azure subscription.

| Component | Core lab configuration |
|---|---|
| Directory | Default Directory |
| Subscription | Azure subscription 1 |
| Analyst account | `hti-p6-analyst` |
| User type | Member |
| Security group | `HTI-P6-Cloud-Security-Analysts` |
| Azure RBAC role | Reader |
| Assignment scope | Subscription |

The B2B extension adds an external Gmail identity and a controlled Azure Storage resource:

| Component | B2B extension configuration |
|---|---|
| External identity | Gmail B2B guest |
| Guest display name | `HTI External Guest Test` |
| Guest user type | Guest |
| Resource group | `HTI-ExternalGuest-Lab` |
| Storage account | `htiexternalguestlab01` |
| Container | `guest-access-test` |
| Test blob | `guest-test.txt` |
| Azure RBAC role | Reader |
| Storage data role | Storage Blob Data Reader |
| Assignment scope | Storage account |

## 4. Core Implementation

The core lab demonstrates:

1. A dedicated analyst account is created in Microsoft Entra ID.
2. The analyst is added to the cloud security analysts security group.
3. The group receives the built-in Reader role at subscription scope.
4. The analyst's role assignments and My permissions views are examined.
5. The subscription IAM view is examined for role-management controls.
6. CLI tests validate the selected read and resource-creation operations.

See the [core evidence register](evidence/README.md) for the original P6-E01–P6-E15 evidence.

## 5. B2B External Guest RBAC Extension

The B2B extension changes the identity boundary while keeping the access-control objective focused on least privilege.

The access path tested was:

**External Gmail identity → Microsoft Entra B2B guest → authentication/MFA → Azure OAuth token → Azure RBAC → Storage account → Blob read**

The external guest was assigned **Reader** and **Storage Blob Data Reader** at the storage-account scope.

### Validation

The external guest authenticated with Azure CLI using:

```powershell
az login --tenant d67ea4f0-cc08-452f-85f6-297bf798fb3d
```

The authenticated context was confirmed with:

```powershell
az account show
```

The Blob read test succeeded:

```powershell
az storage blob show --account-name htiexternalguestlab01 --container-name guest-access-test --name guest-test.txt --auth-mode login
```

The resulting command returned Blob metadata for `guest-test.txt`.

A controlled Blob upload attempt was denied by Azure authorization, and a Blob delete attempt was also denied.

This provides bounded evidence that the external guest could perform the tested read operation but could not perform the tested write or delete operations.

### Portal observation

The guest could authenticate to the lab tenant and use Azure CLI for the resource-access validation. The browser-based guest experience was more limited in this configuration, so CLI was used as the practical validation interface.

This is an observation about the configuration and access path tested in this lab. It should not be generalized as a Microsoft rule that B2B guests can only access Azure resources through CLI.

The full extension documentation and evidence register are in [evidence/b2b-external-guest-rbac](evidence/b2b-external-guest-rbac/README.md).

## 6. Security Observations

Across the core and extension exercises, the project provides practical evidence of:

- Dedicated Entra identities and purpose-specific security groups.
- Group-based Azure RBAC at subscription scope.
- Least-privilege Reader access for the core analyst.
- External B2B guest authentication.
- Resource-scoped RBAC for an external identity.
- Successful authorization for a tested Blob read operation.
- Authorization denial for the tested Blob write operation.
- Authorization denial for the tested Blob delete operation.
- Visibility into selected Azure Activity Log and Entra sign-in records.
- The distinction between identity authentication and resource authorization.

## 7. Limitations

- The core lab uses a single Microsoft Entra tenant.
- The B2B extension uses an external Gmail identity rather than a second organizational tenant.
- The validation covers the specific operations shown; it does not test every Azure permission or Storage operation.
- The B2B browser/portal behavior is specific to the observed lab configuration and should not be generalized to all B2B configurations or licensing tiers.
- The B2B test resource is a controlled lab resource, not a production workload.
- The sign-in evidence consists of selected records and does not establish tenant-wide MFA or Conditional Access enforcement.
- The original core project evidence remains unchanged.

## 8. Evidence

### Core exercise

P6-E01 through P6-E15 document the original single-tenant Azure IAM implementation.

### B2B External Guest RBAC

The extension has its own evidence sequence:

| Evidence ID | Validation |
|---|---|
| P6-B2B-E01 | External B2B guest invitation/user configuration |
| P6-B2B-E02 | Successful invitation redemption |
| P6-B2B-E03 | Guest RBAC assignment / Check access |
| P6-B2B-E04 | External guest Azure CLI authentication |
| P6-B2B-E05 | Authenticated guest identity and tenant context |
| P6-B2B-E06 | Blob read succeeds |
| P6-B2B-E07 | Blob write denied |
| P6-B2B-E08 | Blob delete denied |

Only screenshots actually captured and committed should be mapped to these IDs.

## 9. Conclusion

Project 6 now demonstrates two complementary IAM scenarios: group-based least-privilege access for an internal analyst identity and narrowly scoped least-privilege access for an externally authenticated B2B guest.

The B2B extension adds practical evidence of external identity authentication, resource-scoped Azure RBAC, successful read authorization, and denied write/delete operations while preserving the original project implementation and evidence.
