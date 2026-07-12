# Intended resource access matrix

A compact, explicit specification of who should be able to do what. Derived from project context or from clearly marked assumptions — never invented.

## Columns

Actor | Tenant relation | Resource | Read | Create | Update | Delete | Special actions

## Actors to include (at minimum)

- Normal user (member of a tenant)
- Tenant admin
- Owner
- Removed user (previously a member)
- Invited user (invitation not yet accepted)
- User of a different tenant
- Global support/admin, if such a role exists
- Background job / service path

Add anonymous/public if the app exposes any public surface.

## Operations

Always evaluate Read, Create, Update, Delete separately. Add as applicable: Invite, Accept invitation, Change role, Remove member, Transfer ownership, Export, Search, Upload, Download, Move, Share, Generate signed URL, Run job, Trigger webhook, View analytics, Refresh aggregate, Delete tenant.

## Derivation

Use the matrix as the source for both positive tests ("this MUST be allowed") and negative tests ("this MUST be denied"). Any cell whose expected value is unclear becomes a blocking question or a marked assumption. Any deliberately allowed cross-tenant cell must be documented with reason and bound scope.
