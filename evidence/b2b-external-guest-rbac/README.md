# Project 6 Extension – B2B External Guest RBAC

## 1. Exercise Overview

This extension adds a cross-identity access-control exercise to Project 6.

The objective was to demonstrate **least-privilege Azure RBAC access for an external B2B guest identity** to a specific Azure management/resource scope, rather than granting the external identity broad subscription or directory administration rights.

A Gmail identity was invited into the lab's Microsoft Entra tenant as a B2B guest and then given narrowly scoped Azure RBAC permissions to a controlled Azure Storage account.

The exercise validates the resulting access through Azure CLI using Microsoft Entra/OAuth authentication.

## 2. Objectives

- Create and validate an external B2B guest identity.
- Complete the guest invitation redemption flow.
- Confirm the guest can authenticate to the resource tenant.
- Assign least-privilege Azure RBAC permissions to the guest at the storage-account scope.
- Demonstrate an allowed Blob read operation.
- Demonstrate that Blob write and delete operations are denied.
- Document the observed portal and CLI behavior without generalizing the lab result beyond the tested configuration.

## 3. Lab Configuration

| Component | Configuration |
|---|---|
| Microsoft Entra tenant | Default Directory |
| Tenant ID | d67ea4f0-cc08-452f-85f6-297bf798fb3d |
| Tenant license | Microsoft Entra ID Free |
| External identity | Gmail B2B guest |
| Guest display name | HTI External Guest Test |
| Guest user type | Guest |
| Guest invitation source | One Time Passcode |
| Azure resource group | HTI-ExternalGuest-Lab |
| Storage account | htiexternalguestlab01 |
| Container | guest-access-test |
| Test blob | guest-test.txt |
| Azure RBAC role | Reader |
| Storage data role | Storage Blob Data Reader |
| Assignment scope | Storage account |

## 4. Access Model

The access path tested in this exercise was:

**External Gmail identity → Microsoft Entra B2B guest → authentication/MFA → Azure OAuth token → Azure RBAC → Storage account → Blob read**

The external identity was not granted a directory administrative role or broad subscription-level Contributor/Owner access.

The Reader assignment was used alongside Storage Blob Data Reader at the storage-account scope. The data role provides the tested Blob data read permission, while the Reader role was retained as part of the resource-access configuration used for the exercise.

## 5. External Guest Validation

The B2B invitation was redeemed successfully by the external Gmail identity.

The guest subsequently authenticated with Azure CLI using:

```powershell
az login --tenant d67ea4f0-cc08-452f-85f6-297bf798fb3d
```

MFA setup was completed during the authentication flow.

The authenticated context was then confirmed with:

```powershell
az account show
```

The resulting account context identified:

- the external Gmail address as the authenticated user;
- the lab Microsoft Entra tenant;
- Azure subscription 1.

## 6. RBAC Validation

Azure Check access showed the external guest had:

- **Reader**
- **Storage Blob Data Reader**

at the storage-account scope.

The key validation tests were performed using Azure CLI with Entra authentication.

### 6.1 Allowed read operation

```powershell
az storage blob show --account-name htiexternalguestlab01 --container-name guest-access-test --name guest-test.txt --auth-mode login
```

**Observed result:** The command succeeded and returned the Blob metadata for guest-test.txt.

This demonstrates that the authenticated external guest could read the test Blob using Entra/OAuth authentication and the assigned RBAC permissions.

### 6.2 Denied write operation

A harmless test file was used for the write test:

```powershell
$tempFile = "$env:TEMP\rbac-write-test.txt"; Set-Content -Path $tempFile -Value "RBAC write test"; az storage blob upload --account-name htiexternalguestlab01 --container-name guest-access-test --name rbac-write-test.txt --file $tempFile --auth-mode login --overwrite
```

**Observed result:** Azure returned an authorization failure stating that the required permissions were not available.

No successful upload is shown in the evidence.

### 6.3 Denied delete operation

```powershell
az storage blob delete --account-name htiexternalguestlab01 --container-name guest-access-test --name guest-test.txt --auth-mode login
```

**Observed result:** Azure returned an authorization failure.

This demonstrates that the guest's tested access did not include permission to delete the Blob.

## 7. Portal vs CLI Observation

The external guest could authenticate to the lab tenant and use the Azure CLI for the resource-access validation.

The guest's browser-based experience was more limited in this lab configuration. The guest could access the Microsoft My Apps experience and see the lab directory as a signed-in organization, but the expected normal Azure Portal resource-management experience was not available through that path.

Therefore, **CLI was used as the practical validation interface for this exercise**.

This is a finding about the configuration and access path tested in this lab. It should **not** be documented as a general Microsoft rule that B2B guests can only access Azure resources through CLI.

No Enterprise Application was created solely to force the guest into My Apps, because doing so would have changed the scope of the experiment.

## 8. Security Findings

### Finding 1 – External identity successfully authenticated

The Gmail identity completed B2B invitation redemption and successfully authenticated to the resource tenant.

### Finding 2 – Access was scoped to a specific resource

The guest received Azure RBAC permissions at the storage-account scope rather than broad subscription-level administrative access.

### Finding 3 – Read access was permitted

The external guest successfully retrieved the test Blob using Entra authentication.

### Finding 4 – Write access was denied

The upload attempt was rejected by Azure authorization.

### Finding 5 – Delete access was denied

The delete attempt was rejected by Azure authorization.

Together, these results provide practical evidence of **least-privilege external access**: the external identity could perform the tested read operation but could not perform the tested write or delete operations.

## 9. Limitations

- The exercise was performed within a single Azure/Microsoft Entra resource tenant.
- The external identity was a Gmail B2B guest; no second organizational tenant was used.
- The validation covers the specific Blob read, write, and delete operations shown.
- The portal experience observed during the exercise should not be generalized to all B2B configurations, Azure portal experiences, or Microsoft licensing tiers.
- The exercise did not test every possible Azure Storage permission or every Azure RBAC role.
- The test resource was created specifically for controlled lab validation and is not a production workload.
- The original Project 6 single-tenant RBAC exercise remains unchanged; this is a separate extension exercise.

## 10. Evidence Register

| Evidence ID | Screenshot content |
|---|---|
| P6-B2B-E01 | B2B external guest invitation/user configuration. |
| P6-B2B-E02 | Successful external guest invitation redemption. |
| P6-B2B-E03 | Guest RBAC assignment / Check access showing the configured storage-account permissions. |
| P6-B2B-E04 | Azure CLI authentication with the external Gmail guest. |
| P6-B2B-E05 | az account show confirming the authenticated external identity and resource-tenant context. |
| P6-B2B-E06 | Successful Blob read using --auth-mode login. |
| P6-B2B-E07 | Blob upload/write attempt denied by Azure authorization. |
| P6-B2B-E08 | Blob delete attempt denied by Azure authorization. |

Only screenshots actually captured and committed should be mapped to these evidence IDs. If an additional screenshot is later added, extend the evidence register rather than renumbering existing evidence.

## 11. Conclusion

This extension demonstrates a practical B2B external-access scenario using Microsoft Entra ID and Azure RBAC.

The exercise shows that an external Gmail identity can be invited and authenticated as a guest, assigned narrowly scoped Azure permissions, successfully perform the tested Blob read operation, and be denied the tested write and delete operations.

The result strengthens Project 6 by extending the original single-tenant group-based RBAC lab into an externally authenticated least-privilege access scenario while preserving the original project evidence.