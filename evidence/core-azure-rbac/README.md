# Project 6 – Core Azure RBAC Evidence

This directory contains the original evidence set for Project 6: Azure Identity and Access Management.

The core exercise demonstrates Azure identity and access-control configuration and validation for the internal cloud security analyst scenario.

## Core Evidence Register

| Evidence ID | Evidence Description | What It Demonstrates |
|---|---|---|
| P6-E01 | Microsoft Entra security groups listing | The cloud security analysts security group is present. |
| P6-E02 | Microsoft Entra users listing | The dedicated analyst account is present. |
| P6-E03 | Analyst group membership | The analyst account is a member of the cloud security analysts group. |
| P6-E04 | Azure RBAC role-assignment wizard | Reader is selected as the role and the security group is selected as the member in the assignment workflow. This is the configuration stage, not standalone proof of a completed assignment. |
| P6-E05 | Azure subscription overview | The Azure subscription overview page. |
| P6-E06 | Analyst's Azure role assignments | The analyst's role assignments view displays the subscription-level Reader assignment. |
| P6-E07 | Subscription My permissions | The My permissions view for the analyst at subscription scope. |
| P6-E08 | Azure subscription Activity Log | The subscription Activity Log view. |
| P6-E09 | Subscription Access control (IAM) controls | The captured IAM view shows Add role assignment and Add custom role disabled for the analyst session. |
| P6-E10 | Azure Resource groups view | The Resource groups page displays no resource groups at the time of capture. |
| P6-E11 | Microsoft Entra sign-in logs | Sign-in log entries for the analyst. |
| P6-E12 | Selected sign-in record details | The details view for a selected analyst sign-in record. |
| P6-E13 | Azure CLI identity and subscription context | Output from `az account show`, displaying the subscription and analyst identity context. |
| P6-E14 | Azure CLI resource-group creation attempt | The attempted resource-group creation returns `AuthorizationFailed` for the required `resourceGroups/write` permission. |
| P6-E15 | Azure CLI resource-group listing | `az group list --output table` completes and returns no resource groups. |

## Evidence Handling

- Preserve the original evidence IDs and sequence.
- Redact personal email addresses, tenant identifiers, subscription IDs, and other unnecessary sensitive information before public publication.
- Never publish passwords, access tokens, client secrets, or recovery codes.
- Preserve relevant role names, permission details, command output, and error messages.
- Do not alter evidence in a way that changes the meaning of the result.

## Relationship to the B2B Extension

The original core exercise is kept separate from the later B2B External Guest RBAC extension.

The B2B evidence is maintained in:

[b2b-external-guest-rbac/](../b2b-external-guest-rbac/)

The two evidence sets are part of Project 6 but are reviewed independently.
