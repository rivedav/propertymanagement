# Frontend Ideas and References

## UI Direction for MVP
- Use a utilitarian dashboard style focused on clarity and task speed.
- Build role-aware navigation (hide inaccessible modules).
- Keep forms and tables consistent across modules.

## References
- Next.js App Router docs: https://nextjs.org/docs/app
- Route groups and layout organization: https://nextjs.org/docs/14/app/building-your-application/routing/route-groups
- Image remote patterns hardening: https://nextjs.org/docs/pages/api-reference/components/image

## Frontend Checklist
- [ ] Authenticated app shell
- [ ] Tenant context header and switcher
- [ ] Role-based menu visibility
- [ ] Standard list/detail/create/edit patterns
- [ ] Empty states and loading states
- [ ] Mobile-safe table/card fallback

## v5 UI Concepts
- Project tracking dashboard with milestones, status, and budget indicators.
- Reserve fund dashboard with forecast vs funding target visuals.
- Board voting UI with eligibility, deadline, result, and resolution display.
- Package tracking UI with log, status timeline, and pickup confirmation.
- Amenity reservation calendar with approval status and closures.
- Inspection calendar with due/overdue/completed states.
- Audit trail data grid with column filters, date range, and CSV/PDF export.


## v6 Access-Control UI Concepts
- Administrative Access Dashboard for tenant user administration.
- Permission matrix table (role x module x action) with bulk-edit controls.
- Role template management + custom role override editor.
- Owner payment status panel with overdue and reminder indicators.
- Property manager limited-finance panel with explicit permission badges.
- Approval workflow rules editor with preview mode.


## Drupal-Inspired UI Concepts
- Role/permission matrix table with role x module x action.
- Content editor with rich text + file/media attachments.
- Publish/schedule/approve workflow panel.
- Reusable filterable grid components across admin modules.
- Reusable dashboard widget framework for role-context dashboards.


## Platform Operations UI Ideas
- Monitoring dashboard with tenant health cards and incident timeline
- Tenant feature flag panel with safe rollout indicators
- Maintenance interruption notice composer and preview
- Backup/restore execution screens with status timeline
- Issue reporting intake and triage board


## Monetization UI Concepts
- add-on marketplace/storefront
- tenant billing center and account-state badges
- module entitlement indicators on navigation
- suspension/reactivation journey UX
- listing publication scope selector (tenant-local vs platform-wide)


## New UI Ideas
- plan comparison and assignment console
- add-on commercial rule editor
- tenant lifecycle timeline panel
- export/offboarding wizard
- impersonation warning banner and breadcrumb
- workflow publishing dashboard with approvals
- branding/landing page visual builder


## Platform Capability UI Ideas
- global activity feed timeline with filters
- import center with row-level validation feedback
- export center with async status and retries
- tenant config profile screen (timezone/currency/locale)
- analytics cards for module adoption and usage
- support-safe demo generator/reset console


## Document Management UX Additions
- document metadata form (title/description/category/tags/visibility)
- bulk action bar (publish/unpublish/delete/visibility change)
- schedule panel (publish_date/unpublish_date)
- storage usage meter (used vs quota)

## Import Engine UX Additions
- guided import wizard (8-step flow)
- column mapping table with sample preview
- validation badges by severity
- import result dashboard (created/updated/skipped/failed)
- downloadable template and error report links


## Analytics and Dashboard UI Ideas
- reusable metric cards and trend indicators
- bar/line/pie/donut/stacked chart widgets
- timeline and calendar widgets
- dynamic cross-widget filter controls
- platform admin analytics console
- tenant admin analytics dashboard
- owner financial analytics widgets
- resident activity widgets


## v7 UI Additions
- preventive maintenance schedule board
- asset register table with lifecycle timeline
- leasing applications pipeline board
- showing schedule calendar
- unit marketing card/grid
- owner ledger statement widget
- inspection due/overdue KPI widget
- operational documents workspace for property manager/staff


## Workforce UI Ideas
- employee dashboard cards (leave, hours, pending approvals)
- time entry grid with duplicate-previous action
- leave request timeline with approval state
- holiday/legal notice acknowledgment panel
- pay statement list and statement viewer
- workforce analytics widgets (hours/leave/attendance trends)


## Workforce Payroll UI Ideas
- withholding rule editor with threshold/percentage/effective-date fields
- rule version history timeline with active/inactive badges
- paystub breakdown panel (gross, deductions, net, YTD)
- compare-current-vs-prior statement view
- payroll scope banners (platform workforce vs tenant workforce)



## Professional Services Billing UI Ideas
- client billing workspace with status, payment terms, and active billing profile
- time-entry grid with approval badges and billable/non-billable toggle
- mixed invoice builder showing hourly, fixed-fee, and expense lines together
- invoice preview modes: detailed daily breakdown, summary, grouped
- payment drawer with partial allocation and remaining-balance preview
- invoice table with filters for client, service/project, date range, and status
- payments table with unapplied-amount visibility
- rate history ribbon and withholding snapshot section on invoice detail


## Dashboard Configuration UI Ideas
- dashboard profile editor for platform, tenant, module, and role scopes
- drag-and-drop widget layout editor with size presets
- widget settings drawer with title, chart type, refresh interval, export/fullscreen toggles
- reusable dynamic filter bar with saved filter state
- multi-series chart legend with default-visible and on/off controls
- chart-switch control to move between line/bar/area/table when allowed
- fullscreen analytics workspace for dense widgets and drilldowns
- future user-saved dashboard views without breaking role-based defaults


## Historical Review UI Ideas
- tenant users history table with active/current badges and date-range filters
- ownership timeline view showing prior and current owners by property/unit
- tenant administration history table for administrator succession
- board member term history list with prior/current term markers
- tenant name/path history panel with current flag and effective date
- side-by-side current-state vs historical-state detail view for tenant, property, and unit records


## Consolidation Note
This file is for UI ideas and reference patterns.
Canonical architecture, access, module, and entity definitions belong in:
- `project-docs/04-architecture.md`
- `project-docs/05-data-model.md`
- `project-docs/14-admin-access-menu-matrix.md`
