# Security Specification - FoundHub

## Data Invariants
1. An item cannot exist without a valid `finderId` that matches the authenticated user.
2. The `meritPoints` of a user can only be incremented by the system or during a valid item return process.
3. Once an item is marked as `returned`, its status cannot be changed back to `available` (terminal state locking).
4. `finderId` and `id` are immutable once an item is created.

## The "Dirty Dozen" Payloads (Denial Tests)
1. **Identity Spoofing**: Attempt to create an item with a `finderId` different from the current user.
2. **Merit Point Injection**: Attempt to directly update `meritPoints` in a user profile to a large number.
3. **Ghost Field Update**: Attempt to add an `isVerified` field to an item document.
4. **ID Poisoning**: Attempt to use an extremely long or invalid string as a document ID.
5. **State Shortcut**: Attempt to update an item status to `returned` by someone other than the finder.
6. **Terminal State Reversal**: Attempt to change a `returned` item back to `available`.
7. **Blanket Read**: Attempt to read all user profiles without being the owner.
8. **PII Leak**: Attempt to read the email of another student through a list query.
9. **Unauthenticated Write**: Attempt to create an item without being signed in.
10. **Resource Poisoning**: Attempt to update a `description` with a 1MB string.
11. **Immutability Breach**: Attempt to change the `finderId` of an existing item.
12. **Orphaned Record**: Attempt to create an item referencing a non-existent `finderId`.

## The Test Plan
The `firestore.rules` will be tested against these payloads using the Firestore Emulator/Local Testing if possible, or manually through logic verification in Phase 5.
