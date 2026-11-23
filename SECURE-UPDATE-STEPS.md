# Secure attendance update — steps

1. Upload `index.html` from this ZIP to the existing `attendance-update` branch (not main). It fixes the staff attendance listener to query only their own `sid`.
2. In Firebase Firestore Rules, retain all current rules. Paste the blocks from `FIRESTORE-ATTENDANCE-RULES.txt` inside `match /databases/{database}/documents { ... }`, just before that match's final closing brace. Do not paste after it. Existing functions `signedIn`, `isOwner`, `isMember`, and `memberPath` are used.
3. Verify syntax with Firebase Rules editor, test owner and staff read/write cases with Emulator Suite, then Publish. Do not publish if errors or unexpected access.
4. Only after testing, consider merging branch to main.

LIMITATIONS: This is a functional preview, not tamper-proof payroll. Client device controls check-in clock, date, and updatedAt; backend validation and server timestamps are still needed. Rules limit staff to their own record, only self check-in creation and check-out completion, but cannot guarantee a genuine current-day check-in without a trusted server. Do not use for production payroll or sensitive personnel records until hardened and tested.

Note: Staff queries must use `.where('sid','==',attIdentity())`; a collection-wide listener will fail for staff with these rules.
