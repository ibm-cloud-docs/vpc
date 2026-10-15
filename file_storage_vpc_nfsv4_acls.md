---

copyright:
  years: 2026
lastupdated: "2026-10-15"

keywords: file storage, file share, NFSv4 ACL, access control lists, POSIX ACL, rfs profile, dp2 profile, permissions, ACE, access control

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# NFSv4 ACL support and limitations in {{site.data.keyword.filestorage_vpc_short}}
{: #file-storage-vpc-nfsv4-acls}

Review NFSv4 Access Control List (ACL) support, mapping behavior to POSIX ACLs, and known limitations across {{site.data.keyword.filestorage_vpc_full}} profiles.
{: shortdesc}

## Profile support overview
{: #fs-acl-profile-support}

{{site.data.keyword.filestorage_vpc_full}} is available in two profile families: **`dp2`** and **`rfs`**.

`dp2` profile
:   NFSv4 ACLs are not supported. Access control on `dp2` file shares is managed exclusively through standard POSIX permissions (mode bits) and POSIX ACLs by using standard NFS client tools.

`rfs` profile
:   NFSv4 ACLs are supported, but support is limited. The underlying storage backend for `rfs` file shares natively supports POSIX ACLs only. When NFS clients configure NFSv4 ACLs, the storage layer translates those Access Control Entries (ACEs) to POSIX ACLs. This translation is lossy because several NFSv4 ACL constructs and fine-grained permission bits have no POSIX equivalent and cannot be preserved.

Review these limitations before you use NFSv4 ACLs on `rfs` file shares. Fine-grained access control semantics, such as explicit DENY ACEs, ordered ACE evaluation, or per-file delete permissions independent of write permissions, are not fully supported.
{: important}

For more information about share profiles, see [{{site.data.keyword.filestorage_vpc_short}} profiles](/docs/vpc?topic=vpc-file-storage-profiles).

## Summary of supported and unsupported NFSv4 ACL constructs
{: #fs-acl-summary}

NFSv4 ACL construct support applies to the `rfs` profile only. The `dp2` profile does not support NFSv4 ACL operations. POSIX permissions and POSIX ACLs apply to `dp2` file shares regardless of any NFSv4 ACL operations attempted by an NFS client.

| NFSv4 ACL constructs | Support status | Notes |
|---|---|---|
| `ACE4_READ_DATA` | Supported | Maps to the POSIX `read` bit. |
| `ACE4_WRITE_DATA` and `ACE4_APPEND_DATA` | Supported | Map to the POSIX `write` bit. |
| `ACE4_EXECUTE` | Supported | Maps to the POSIX `execute` bit. |
| `ACE4_DELETE_CHILD` (directories) | Supported | Maps to the POSIX `write` bit on the directory. |
| `ACE4_READ_ACL`, `ACE4_READ_ATTRIBUTES`, `ACE4_SYNCHRONIZE` | Supported | Granted unconditionally per the mapping specification. |
| `ACE4_WRITE_ACL` and `ACE4_WRITE_ATTRIBUTES` | Supported (owner only) | Granted to `OWNER@`, but dropped for all other principals. |
| `OWNER@` principal | Supported | Maps to the POSIX owner (`ACL_USER_OBJ`). |
| `GROUP@` principal | Supported | Maps to the POSIX owning group (`ACL_GROUP_OBJ`). |
| `EVERYONE@` principal | Supported | Maps to POSIX other (`ACL_OTHER`). |
| Named user ACEs | Supported | Preserved with UID mapping. |
| Named group ACEs | Supported | Preserved with GID mapping, subject to the POSIX mask. |
| Directory default ACL (inheritance) | Supported | Translated from ACEs that have both `FILE_INHERIT` and `DIRECTORY_INHERIT` set. |
| ALLOW-only ACLs in canonical order | Supported | Round-trip accurately with no loss beyond unmappable bits. |
| `ACE4_DELETE` (per-file delete) | Unsupported | No POSIX equivalent. Deletion is governed by the parent directory write bit, so write-without-delete cannot be enforced. |
| `ACE4_WRITE_ATTRIBUTES` (nonowners) | Unsupported | Dropped. No POSIX equivalent exists for principals other than the owner. |
| `ACE4_READ_NAMED_ATTRS` and `ACE4_WRITE_NAMED_ATTRS` | Unsupported | Dropped. No named attribute concept exists in POSIX. |
| `ACE4_WRITE_RETENTION` and `ACE4_WRITE_RETENTION_HOLD` | Unsupported | Dropped. No retention concept exists in POSIX. |
| `ACE4_WRITE_OWNER` | Unsupported | Dropped. Not representable in POSIX. |
| Explicit DENY ACEs | Unsupported | POSIX ACLs do not support deny entries. Deny semantics cannot be expressed, and access policies that rely on explicit denials are not enforced. |
| Interleaved ALLOW and DENY ordering | Unsupported | POSIX evaluates access by fixed type precedence rather than list order. Noncanonical ACE ordering cannot be faithfully represented. |
| `ACE4_INHERIT_ONLY_ACE` flag | Unsupported | Cannot be distinctly represented in POSIX default ACLs. |
| `NO_PROPAGATE_INHERIT` flag | Unsupported | POSIX default ACLs apply to all descendant levels. Single-level propagation cannot be expressed. |
| `FILE_INHERIT` without `DIRECTORY_INHERIT` (or vice versa) | Unsupported | POSIX default ACLs apply to both files and subdirectories. Per-type inheritance differentiation cannot be represented. |
| `INTERACTIVE@`, `NETWORK@`, `DIALUP@`, `ANONYMOUS@`, `AUTHENTICATED@`, `SERVICE@` | Unsupported | No POSIX equivalent. Silently discarded. |
| POSIX mask truncation of named entries | Partially supported | The POSIX mask caps named user and group permissions at the union of all named entry permissions. If a principal is granted more permissions than the group class, the storage layer might reduce those permissions. |
| Multi-group simultaneous access | Partially supported | POSIX requires all requested bits to be granted by a single matching group ACE. NFSv4 allows bits to be granted individually across different entries, which can cause unexpected denials for users who belong to multiple groups. |
{: caption="NFSv4 ACL constructs and POSIX mapping support on rfs file shares" caption-side="bottom"}

Simple use cases (such as owner read and write, group read, and other none) that use named user and group ALLOW ACEs in canonical order map cleanly and work as expected on `rfs` file shares. Complex multi-principal or deny-based policies are affected by these limitations.
{: note}

## Details of POSIX-mapped NFSv4 ACL limitations
{: #fs-acl-posix-mapping-limitations}

### Loss of fine-grained permission bits
{: #fs-acl-loss-permission-bits}

NFSv4 defines 14 distinct file permission bits. POSIX ACLs provide only three permissions: `read`, `write`, and `execute`. Several NFSv4 permission bits have no direct POSIX equivalent and are silently lost or collapsed during translation:

| NFSv4 permission bit | POSIX equivalent | Notes |
|---|---|---|
| `ACE4_DELETE` | None | Controlled by parent directory permissions in POSIX. |
| `ACE4_DELETE_CHILD` | None | No child-deletion permission bit exists in POSIX. |
| `ACE4_WRITE_ATTRIBUTES` | None | Folded into `write` or lost. |
| `ACE4_WRITE_NAMED_ATTRS` | None | No named attribute concept exists in POSIX. |
| `ACE4_READ_NAMED_ATTRS` | None | No named attribute concept exists in POSIX. |
| `ACE4_WRITE_RETENTION` | None | No retention concept exists in POSIX. |
| `ACE4_READ_DATA` and `ACE4_EXECUTE` | `read` and `execute` | These permission bits map directly. |
{: caption="NFSv4 permission bit mapping to POSIX" caption-side="bottom"}

**Practical impact:** An administrator might grant a user write permissions for a file, but attempt to deny delete permissions by using NFSv4 ACEs. With POSIX mapping, this distinction cannot be expressed. Both `write` and `delete` operations for a file are governed by the write permission on the parent directory. Therefore, write-without-delete semantics are not enforced at the storage layer.

### DENY ACEs are largely unsupported
{: #fs-acl-deny-aces}

NFSv4 ACLs support explicit `DENY` ACEs, enabling policies such as granting read access to all users in a group except a specific user. POSIX ACLs have no concept of a DENY entry. Access is determined solely by matching permission bits against the user, group, and mask entries.

**Consequence:** Explicit `DENY` ACEs configured by NFSv4 clients are silently dropped or cannot be represented in the underlying storage backend. Access control policies that rely on explicit deny semantics are not enforced.

### ACE ordering is not preserved
{: #fs-acl-ace-ordering}

NFSv4 ACLs are evaluated sequentially in list order, where the first matching ACE determines the permission decision. This order enables interleaved ALLOW and DENY rules. POSIX ACLs do not evaluate entries in list order; they evaluate by matching against a fixed precedence order:

1. Owner (`ACL_USER_OBJ`)
1. Named user (`ACL_USER`)
1. Owning group (`ACL_GROUP_OBJ`) or named group (`ACL_GROUP`)
1. Other (`ACL_OTHER`)

After matching, the POSIX mask (`ACL_MASK`) is applied.

**Consequence:** Complex ordered ACL policies do not retain their evaluation semantics. After a write-back through the POSIX layer, effective permissions can differ from what the NFSv4 client configured.

### POSIX mask truncates named entry permissions
{: #fs-acl-posix-mask}

POSIX ACLs apply a `mask` entry that caps the maximum effective permissions of all named user and named group entries. When an NFSv4 ACL is translated to POSIX, the mask is computed from the union of permissions across all named entries.

**Consequence:** If a named user is granted more permissions than the group class, the POSIX layer silently masks off the extra permissions.

### Inheritance flags are not fully representable
{: #fs-acl-inheritance-flags}

NFSv4 ACEs support inheritance flags (`FILE_INHERIT`, `DIRECTORY_INHERIT`, `INHERIT_ONLY`, `NO_PROPAGATE_INHERIT`) that control how permissions propagate to newly created child files and subdirectories. POSIX ACLs support only a single default ACL per directory, which applies uniformly to all child items.

**Consequence:**
- `INHERIT_ONLY` ACEs (configured on a directory to govern inheritance without granting access to the directory itself) cannot be represented.
- `NO_PROPAGATE_INHERIT` (inheriting permissions only to direct children and not deeper descendants) has no POSIX equivalent.
- Mixed file and directory inheritance rules collapse into a single default ACL that applies equally to both files and subdirectories.

### Special principals with no POSIX equivalent
{: #fs-acl-special-principals}

NFSv4 defines special principal identifiers (`who` values). These principals map to POSIX support status:

| NFSv4 special principal | POSIX support status | Notes |
|---|---|---|
| `OWNER@` | Supported | Maps to the POSIX file owner (`ACL_USER_OBJ`). |
| `GROUP@` | Supported | Maps to the POSIX owning group (`ACL_GROUP_OBJ`). |
| `EVERYONE@` | Supported | Maps to POSIX other (`ACL_OTHER`), losing group membership nuance. |
| `INTERACTIVE@`, `NETWORK@`, `DIALUP@` | Unsupported | No POSIX equivalent. Silently discarded. |
| `ANONYMOUS@` | Unsupported | No POSIX equivalent. Silently discarded. |
| `AUTHENTICATED@` | Unsupported | No POSIX equivalent. Silently discarded. |
| `SERVICE@` | Unsupported | No POSIX equivalent. Silently discarded. |
{: caption="NFSv4 special principals and POSIX mapping behavior" caption-side="bottom"}

**Consequence:** ACEs targeted at network-origin or authentication-state principals are discarded entirely at the storage layer.

### Round-trip fidelity is not preserved
{: #fs-acl-round-trip-fidelity}

Because of architectural differences between NFSv4 and POSIX ACLs, ACLs configured by an NFSv4 client are translated by the POSIX-mapped backend on `rfs` file shares. When an NFS client reads back the ACL, the resulting entries often differ from what was originally written.

Applications, backup and restore utilities, and ACL auditing tools that depend on strict ACL round-trip consistency might encounter unexpected discrepancies.
