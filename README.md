# Project 6: Azure Identity and Access Management — Group-Based RBAC and Least Privilege

**Author:** Taiwo H Ibrahim  
**Platform:** Microsoft Azure  
**Identity service:** Microsoft Entra ID  
**Access control:** Azure Role-Based Access Control (Azure RBAC)  
**Project type:** Practical Cloud Security Lab

## 1. Project Overview

This project demonstrates a practical implementation of identity and access management controls in Microsoft Azure. It uses a dedicated Microsoft Entra ID analyst account, a security group, and an Azure RBAC role assignment to explore group-based access and least-privilege authorization.

The lab also examines the analyst's subscription visibility, Azure Activity Log, sign-in records, and command-line authorization behavior.

## 2. Objectives

- Create a Microsoft Entra ID security group for cloud security analysts.
- Create a dedicated analyst user account and add it to the group.
- Configure a group-based Azure RBAC assignment.
- Examine the analyst's effective subscription permissions.
- Review Azure Activity Log and Entra sign-in records.
- Test a permitted read operation and a resource-creation operation.
- Document the observed results and the limits of the evidence.

## 3. Lab Architecture

The implementation uses one Microsoft Entra tenant and one Azure subscription.

| Component | Lab configuration |
|---|---|
| Directory | Default Directory |
| Subscription | Azure subscription 1 |
| Analyst account | `hti-p6-analyst` |
| User type | Member |
| Security group | `HTI-P6-Cloud-Security-Analysts` |
| Group membership | Assigned |
| Azure RBAC role | Reader |
| Assignment scope | Subscription |

### Access model

1. The analyst signs in using the dedicated Entra ID account.
2. The analyst is a member of the cloud security analysts security group.
3. The group has a Reader role assignment at subscription scope.
4. The analyst's role-assignment and My permissions views are used to inspect the resulting access.
5. CLI tests are used to observe the behavior of a read operation and a resource-creation operation.

## 4. Implementation and Configuration

### Microsoft Entra ID

A security group named `HTI-P6-Cloud-Security-Analysts` was created. A dedicated Member account, `hti-p6-analyst`, was also created and added to the group.

The group listing, user listing, and analyst's Groups page are included in the evidence register below.

### Azure RBAC

The Azure role-assignment workflow was used to select the built-in Reader role and the `HTI-P6-Cloud-Security-Analysts` group as the member. The analyst's Azure role assignments view subsequently displays the Reader assignment at subscription scope. The analyst's My permissions view is also captured as supporting evidence of the access view available to that identity.

The configuration wizard screenshot records the selected role and group during the workflow; the separate role-assignment and My permissions screenshots provide the subsequent views.

## 5. Validation and Investigation

### Portal validation

The evidence captures the following portal views while working with the analyst identity:

- Subscription overview.
- Analyst's Azure role assignments.
- Subscription My permissions.
- Subscription Activity Log.
- Access control (IAM) overview.
- Resource groups blade, which displayed no resource groups at capture time.
- Analyst sign-in logs and a selected sign-in detail view.

These screenshots document the views captured during the lab. They should not be interpreted as proof of controls or tests that are not visible in those views.

### Command-line validation

The CLI checks were performed in the analyst's Azure context.

**Identity and subscription context**

```bash
az account show --query "{subscription:name, id:id, user:user.name}" -o table
```

The captured output displays the Azure subscription and the `hti-p6-analyst` user context.

**Read operation**

```bash
az group list --output table
```

The command completed and returned no resource groups. This is consistent with the empty Resource groups view captured in the portal.

**Resource-creation attempt**

```bash
az group create --name rg-reader-permission-test --location eastus
```

The captured output shows an `AuthorizationFailed` response for the resource-group creation operation, including the permission:

```text
Microsoft.Resources/subscriptions/resourceGroups/write
```

No successful resource-group creation is shown in the evidence.

### What the tests establish

The captured CLI results show that the analyst context could execute the tested resource-group listing command and that the tested resource-group creation attempt was denied by Azure authorization.

These are specific test results. They do not establish that every possible read operation succeeds or that every possible write operation is denied.

## 6. Sign-in and Activity Investigation

The lab includes screenshots of the analyst's Entra sign-in log and a selected sign-in detail view, as well as the subscription Activity Log.

These views provide investigation context for the identity and subscription activity captured during the exercise. The screenshots should be interpreted as records visible at the time of capture; they are not, by themselves, a complete assessment of tenant-wide authentication policy, Conditional Access, or MFA enforcement.

## 7. Security Observations

The lab provides practical evidence of:

- A dedicated analyst identity separate from the administrator identity.
- Assigned membership of the analyst in a purpose-specific Entra security group.
- A group-based Reader role assignment at subscription scope.
- Visibility into the analyst's Azure role assignments and My permissions views.
- A successful resource-group listing command that returned an empty result.
- An explicit authorization denial for the tested resource-group creation operation.
- Access to selected Azure Activity Log and Entra sign-in views.

At the time of the captured resource inventory, no resource groups were displayed. Therefore, inspection of an existing deployed workload was not part of the demonstrated validation.

## 8. Limitations

- The lab uses a single Microsoft Entra tenant; cross-tenant B2B collaboration was not implemented.
- No production workload is demonstrated in the evidence.
- The CLI validation covers the specific list and create operations shown, not every Azure permission or operation.
- The sign-in screenshots are selected records and do not establish tenant-wide MFA or Conditional Access enforcement.
- The Activity Log and IAM overview screenshots document portal views; they do not independently prove a complete security posture assessment.
- A zero-resource inventory limits the scope of resource-level inspection in this exercise.

## 9. Evidence Register

The following register maps each filename to the screenshot currently committed in the repository.

| Evidence ID | Screenshot content |
|---|---|
| [P6-E01](evidence/P6-E01.png) | Microsoft Entra security groups listing, including the cloud security analysts group. |
| [P6-E02](evidence/P6-E02.png) | Microsoft Entra users listing, including the dedicated analyst account. |
| [P6-E03](evidence/P6-E03.png) | The `hti-p6-analyst` Groups page showing membership in `HTI-P6-Cloud-Security-Analysts`. |
| [P6-E04](evidence/P6-E04.png) | Azure role-assignment wizard showing Reader selected and the security group selected as the member. |
| [P6-E05](evidence/P6-E05.png) | Azure subscription overview. |
| [P6-E06](evidence/P6-E06.png) | The analyst's Azure role assignments view, showing the subscription-level Reader assignment. |
| [P6-E07](evidence/P6-E07.png) | Subscription My permissions view for the analyst. |
| [P6-E08](evidence/P6-E08.png) | Azure subscription Activity Log. |
| [P6-E09](evidence/P6-E09.png) | Azure subscription Access control (IAM) overview. |
| [P6-E10](evidence/P6-E10.png) | Resource groups view, displaying no resource groups at capture time. |
| [P6-E11](evidence/P6-E11.png) | Microsoft Entra sign-in logs for the analyst. |
| [P6-E12](evidence/P6-E12.png) | Selected sign-in record details for the analyst. |
| [P6-E13](evidence/P6-E13.png) | CLI output from `az account show`, displaying the subscription and analyst identity context. |
| [P6-E14](evidence/P6-E14.png) | CLI resource-group creation attempt, showing an `AuthorizationFailed` response for `resourceGroups/write`. |
| [P6-E15](evidence/P6-E15.png) | CLI output from `az group list --output table`, completing with no resource groups listed. |

## 10. Conclusion

This project provided practical experience with Microsoft Entra ID identities and groups, Azure RBAC, subscription-level access inspection, and authorization testing.

The evidence demonstrates the analyst's group membership, the Reader role assignment workflow and resulting role-assignment view, and the observed outcomes of the selected CLI read and resource-creation tests.

The project is part of a cybersecurity portfolio focused on cloud security, IAM hardening, SOC operations, and defensive security.

---

**GitHub Portfolio Author:** Taiwo H Ibrahim
