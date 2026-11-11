# Infrastructure › Prosight infrastructure

owner: bohdantanasov (most confirmed records) · records: 7 (7 confirmed) · rules guarded by a test: 0 of 7 · last change: 2026-11-06

> Compiled by DevDeck from the team memory’s records — do not edit by hand: a change here is a record.

## Must not break
- The service-tokens DynamoDB table ARN in PlatformIntegrationsStack must be built from the table name constant (CommonResourcesStack.ServiceTokensTableName), not from a cross-stack token reference to the CommonResourcesStack construct. [mem_6c7fd47e394f]
  why: Reviewer noted a cross-stack token reference would make CDK create a generated export in stack-common that both environments' stacks depend on, instead of following the repo's explicit commonStack.ExportValue(...) pattern.
  do: When referencing resources from CommonResourcesStack elsewhere, pass the resource name/ARN as a string constant or explicit ExportValue, never a direct cross-stack construct/token reference.
  guarded: no test yet
  where: infrastructure:src/Prosight.Infrastructure/Stacks/PlatformIntegrationsStack.cs:308
  PRO-10028
- The run queue's message retention, the DLQ's retention, the STALE_QUEUED_MS threshold, and the MetricApproximateAgeOfOldestMessage alarm threshold must stay ordered consistently (alarm threshold < STALE_QUEUED_MS < queue retention < DLQ retention) so stale-message detection and the age alarm both have time to fire before messages are dropped. [mem_86114534cabe]
  why: Reviewer flagged in infrastructure#169 that the run queue's Duration.Hours(1) retention was shorter than the 45-minute STALE_QUEUED_MS while the alarm fires at 20 minutes, and the fix aligned these values across PlatformIntegrationsStack.cs.
  do: When touching queue retention, DLQ retention, STALE_QUEUED_MS, or the age-of-oldest-message alarm threshold in PlatformIntegrationsStack.cs, re-check that all four values stay in the correct relative order.
  guarded: no test yet
  where: infrastructure:src/Prosight.Infrastructure/Stacks/PlatformIntegrationsStack.cs:174
  PRO-10028
- Secrets Manager paths for integration clients must stay under a prefix already covered by an IAM policy (e.g. `integrations/*` granted in CommonResourcesStack.cs:519 and :971); the rotation role and deploy policy only have read access to prefixes explicitly referenced there. [mem_8809a14d60fa]
  why: Reviewer noted CommonResourcesStack.cs:519 and :971 grant read on `integrations/*`, while the new `partner-access-web` client maps to `partner-access/{env}/…`, a prefix no policy in src references.
  do: When adding a new integrations client/secret prefix in PlatformIntegrationsStack.cs, add or extend a matching IAM policy grant in CommonResourcesStack.cs (or reuse the `integrations/*` prefix) so the rotation role and deploy policy can read it.
  guarded: no test yet
  where: infrastructure:src/Prosight.Infrastructure/Stacks/PlatformIntegrationsStack.cs:394
  PRO-10028
- In the platform integrations stack, the KMS key and the credentials table must keep RemovalPolicy.RETAIN, while the runs table and the files bucket are RemovalPolicy.DESTROY by design. [mem_a6f09db8aa5a]
  why: Review on infrastructure#169 confirmed that only the KMS key (PlatformIntegrationsStack.cs:129) and credentials table (:145) are RETAIN, while the runs table (:159) and files bucket (:233) are DESTROY.
  do: Before changing removal policies on this stack, check which resources are meant to survive a stack delete/redeploy (key, credentials) versus which are expected to be wiped (runs, files), and update the rollback doc if the set changes.
  guarded: no test yet
  where: infrastructure:src/Prosight.Infrastructure/Stacks/PlatformIntegrationsStack.cs:129
  PRO-10028
- The Platform API resource server exposes a single scope, `{platformApiHost}/salesmen`, and this one scope also covers write operations — there is no separate scope for writes to restrict to. [mem_c0c4405cd346]
  why: Review thread on PlatformIntegrationsStack.cs:371 confirmed this is the Platform API resource server's only scope and that it covers writes too.
  do: When granting or auditing access for clients like eic-cache against the Platform API, assume `salesmen` is the sole scope and covers both reads and writes rather than looking for a write-specific scope.
  guarded: no test yet
  where: infrastructure:src/Prosight.Infrastructure/Stacks/PlatformIntegrationsStack.cs:371
  PRO-10028
- The KMS key policy used by PlatformIntegrationsStack has no root `kms:*` statement, so any IAM identity statement granting `kms:Encrypt`/`kms:Decrypt`/`DescribeKey` on this key must itself carry the `Null` condition on `kms:EncryptionContext:salesman_id` — the key policy will not scope access for you. [mem_e5abcc413ce9]
  why: Review remark on infrastructure#169 pointed out the identity statement granted kms:Encrypt/Decrypt/DescribeKey with no condition while the key policy lacks a root kms:* statement.
  do: When adding or editing any IAM statement that grants kms actions on this key, always include the salesman_id EncryptionContext condition rather than relying on the key policy to restrict access.
  guarded: no test yet
  where: infrastructure:src/Prosight.Infrastructure/Stacks/PlatformIntegrationsStack.cs:261
  PRO-10028
- The PassRole statement `PassDefaultLambdaSamRole` in the shared `policy-lambda-sam-deploy` policy adds partner-access role ARNs without an `iam:PassedToService` condition, so any principal with that policy can pass those roles to any service, not just Lambda. [mem_b356d7b76145]
  why: Review remark on infrastructure#169 (PRO-10028) flagged this gap; the follow-up fix only documented the deploy principal as a party that can decrypt (docs/platform-integrations-stack.md:35-43, PlatformIntegrationsStack.cs:81-83) but left the PassRole statement itself unscoped (CommonResourcesStack.cs:895-908, :1499-1507).
  do: Add an `iam:PassedToService` condition (or split the partner-access role ARNs into a separate, scoped policy) before granting further roles via `policy-lambda-sam-deploy`.
  guarded: no test yet
  where: infrastructure:src/Prosight.Infrastructure/Stacks/CommonResourcesStack.cs:908
  PRO-10028
