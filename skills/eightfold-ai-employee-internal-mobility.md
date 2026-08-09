---
name: Find internal mobility matches for an employee
description: Surface matching demands for an employee and book them onto one.
api: https://apiv2.eightfold.ai/api/v2/
operations: [get_employee_by_id, list_employee_matched_demands, create_booking]
generated: '2026-07-19'
method: generated
source: https://apidocs.eightfold.ai/reference
---

# Find internal mobility matches for an employee

Use Eightfold's talent-management surface to match an employee to internal demands and
book them, powering internal mobility and resource management.

## Auth

Bearer token per `authentication/eightfold-authentication.yml`. Needs `employee:READ`,
`demand:READ`, and `booking:WRITE` scopes.

## Steps

1. **Load the employee.** Call `get_employee_by_id` with the employee's profile id.
2. **List matched demands.** Call `list_employee_matched_demands` for that employee. This
   works for employee profiles only (not candidates). By default it filters by the
   employee's location and a minimum star threshold of 3.5, sorted by match score; pass
   `ignoreLocation=true` to search globally and `minStarThreshold` to tune quality.
3. **Book the employee.** Call `create_booking` to book the employee onto a chosen demand.
   Bookings require at least one of `employeeEmail` or `demandId` on subsequent list calls.

## Conventions

- Match endpoints sort by score; some return a fixed cap (e.g. 100) without pagination.
- Honor async transaction semantics for writes (see conventions artifact).
