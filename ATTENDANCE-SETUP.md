# Office Desk Attendance / Hourly Payroll — setup and limitations

## Included in this version
- Attendance tab for Owner and Staff, self Check IN/OUT and owner manual corrections.
- Owner sets each staff member's Gross Monthly Salary and Per Hour Rate independently.
- Salary of the Month = sum of completed daily working hours × hourly rate.
- Month picker, date-wise entries, CSV export (opens in Excel), and browser Print/Save as PDF.
- Attendance records store source (`self`, `manual`, `manual correction`), editor UID and updated time.
- Owner appears as a separate attendance person; owner salary is not calculated.

## Important: Firestore security rules must be configured BEFORE using with staff
The app attempts to read/write the `attendance` and `attendance_payroll` Firestore collections. Existing Firebase project rules may deny these operations. **Do not make the collections public or use `allow read, write: if true`.** Configure rules that:
1. Require authenticated users.
2. Permit the owner UID (from `office/meta.owner`) to read and write all attendance and payroll records.
3. Permit active staff (from `members/{uid}`) to read only their own attendance records, identified by `members/{uid}.sid`, and create/update only their own record for the current day; block changing other people's data and manual records.
4. Permit staff to read only their own payroll settings if staff salary display is enabled; never let staff edit payroll.
5. Validate allowed fields, dates, time strings, record ownership and prevent deletion or unwanted overwrites.
6. Test with separate owner and staff accounts in the Firebase emulator before deploying.

**Current implementation is a functional preview, not a hardened payroll product.** Its device clock is used for self check-in and check-out and therefore is not tamper-proof. Server timestamps, server-side validation, overnight shift handling, breaks, paid leave, overtime, payroll locking and audit history require further work. Existing Firebase rules may reject attendance operations. Staff can view their hours but salary amounts are owner-only in this version.

## Installation (phone)
Upload the updated `index.html` to the GitHub repository root. Keep existing icons, manifest and `sw.js`. Refresh the app after GitHub Pages updates; close/reopen the installed app if necessary. Before making any change, back up the original repository and existing Office Desk data.

## Excel and PDF
CSV opens in Excel. PDF uses the phone browser's Print / Save as PDF, and may include the rest of the app interface. Dedicated printable payslip/PDF templates are not included.
