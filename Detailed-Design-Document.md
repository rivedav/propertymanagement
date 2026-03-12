# Detailed Design Document

**Version:** 1.0  
**Status:** Draft / Architecture Phase  
**Last Updated:** 2026-03-10

---

## 1. Executive Summary

### Project Vision
This project is a comprehensive, multi-tenant Property Management Platform designed to serve residential communities, condominiums, and property management companies. Unlike traditional siloed systems, this platform treats user identity as a global concept, allowing a single user to interact with multiple tenants (organizations) simultaneously in different capacities (e.g., as an Owner in one building and a Board Member in another).

### Key Differentiators
*   **Global Identity:** One login for all property relationships.
*   **Contextual Dashboards:** The user interface adapts dynamically based on the active tenant and the user's specific role context (Admin, Owner, Resident, Staff).
*   **Unified Governance & Finance:** Integrated support for board governance, voting, and complex financial operations (receivables/payables/budgets) alongside standard property operations.

---

## 2. System Architecture

### Frontend Stack
*   **Framework:** Next.js 16 (App Router)
*   **Language:** TypeScript
*   **UI Library:** React 19
*   **Styling:** Tailwind CSS v4

### Backend & Database Strategy
*   **Database:** Relational Database (PostgreSQL-compatible).
*   **Infrastructure:** Provider agnostic (compatible with Supabase, AWS RDS, Azure, or self-hosted).
*   **Schema Design:** Shared schema architecture.

### Multi-tenancy Architecture
*   **Isolation Strategy:** Tenant isolation is enforced through strict `tenant_id` filtering at both the application service layer and the database query layer.
*   **Data Partitioning:** All business records (units, leases, finances, logs) are stamped with a `tenant_id`.
*   **Global Records:** Only `users` and system-level configuration exist outside of tenant boundaries.

---

## 3. User Context Resolution Model

A fundamental architectural component is how the system resolves what a user sees.

### 3.1 Global Identity
*   A single `users` table record represents a human identity.
*   Authentication is global; Authorization is tenant-scoped.

### 3.2 Relationship Matrix
Users connect to Tenants via three primary vectors:
1.  **Membership:** Administrative or staff roles (Tenant Admin, Property Manager).
2.  **Unit Relationship:** Property-based connection (Owner, Resident, Landlord).
3.  **Contract Assignment:** Time-bound operational scope (Leasing Agent, Contractor).

### 3.3 Resolution Logic
When a user logs in, the system performs the following resolution flow:
1.  **Active Tenant Context:** User selects which Tenant organization to view.
2.  **Active Role Context:** System aggregates permissions based on valid, active relationships within that Tenant.
3.  **Dashboard Resolution:**
    *   *Admin Context* $\rightarrow$ Tenant Admin / Property Manager Dashboard.
    *   *Resident Context* $\rightarrow$ Resident Dashboard.
    *   *Owner Context* $\rightarrow$ Owner Dashboard.
4.  **Permission Resolution:** Final access is determined by the intersection of **Role + Module + Action**.

---

## 4. Core Modules & Functional Scope

### Module 1: Identity & Access Management
*   **Entities:** Users, Tenants, Roles, Permissions, Tenant Memberships.
*   **Scope:** Authentication, profile management, tenant switching, role assignment.

### Module 2: Financial Management
*   **Receivables:** Tracking charges (rent, fees), payment processing, and allocation of payments to charges.
*   **Payables:** Vendor bill tracking, approval workflows, and outgoing payments.
*   **Budgeting:** Fiscal year planning, budget vs. actuals analysis.
*   **Reserve Fund:** Long-term asset tracking, funding targets, and reserve analysis.
*   **Reporting:** Financial statements, balance sheets, operational reports.

### Module 3: Property Operations
*   **Maintenance:** Work order lifecycle (Request $\rightarrow$ Approval $\rightarrow$ Assignment $\rightarrow$ Completion).
*   **Vendor Management:** Management of vendor entities, insurance compliance, and ratings (Note: Vendors are entities, not system users).
*   **Inspections:** Compliance and safety inspection tracking.
*   **Capital Projects:** Management of large-scale renovation or repair projects.

### Module 4: Governance & Administration
*   **Board Management:** Board member terms, roles, and permissions.
*   **Meetings:** Scheduling, agenda creation, and minute taking.
*   **Voting:** Digital resolutions and voting records.
*   **Audit:** Comprehensive audit trails for business and security events.

### Module 5: Community & Amenities
*   **Amenities:** Resource configuration (pools, gazebos), reservation rules, and booking management.
*   **Activities:** Community calendar, event publishing.
*   **Packages:** Delivery tracking and resident notification.
*   **Digital Forms:** Standardized intake for renovation requests, pet registration, etc.

### Module 6: Content & Communications
*   **Document Library:** Folder-based storage with role-based visibility (Public, Board, Owner, Resident).
*   **Media Gallery:** Photo/Video albums.
*   **Communications:** Email templates, announcements, and notification center.

---

## 5. Access Control & Security

### Role Matrix
The system supports the following primary user roles:
1.  **Platform Admin:** System-wide oversight.
2.  **Tenant Admin:** Full control within a specific tenant organization.
3.  **Property Manager / Staff:** Operational access (manage maintenance, bookings, limited finance).
4.  **Board Member:** Governance access (meetings, votes, privileged documents).
5.  **Owner:** Financials (receivables), notices, and property-specific docs.
6.  **Resident / Renter:** Lease-specific view, maintenance requests, community features.
7.  **Leasing Agent:** Scoped access to assigned units and contracts.

### Permission Granularity
Permissions are defined by **Module + Action**.
*   **Actions:** View, Create, Edit, Approve, Delete, Manage, Export, Publish, Schedule.
*   **Example:** `Announcements:Create`, `Budget:Approve`, `WorkOrder:Edit`.

---

## 6. User Experience & Navigation

### Navigation Structure
Navigation is derived from the v6 Mindmap and the Active Dashboard Context.

### Dashboard Layouts
*   **Admin/Staff Layout:** Sidebar navigation with dense data tables and operational metrics.
*   **Resident/Owner Layout:** Mobile-responsive, card-based layout focusing on "My Unit," "My Payments," and "Community."
*   **Context Switcher:** Prominent UI element allowing users to toggle between Tenants and Roles (e.g., switching from "Board Member" view to "Resident" view).
## Source-of-Truth Note
This document is a contextual design narrative.
Canonical architecture/scope/rules live in `project-docs/`.
If differences exist, follow `project-docs/*`.

