
# User Management Screen — UI Specification

**Version:** 1.0  
**Audience:** Front-end and Full-stack Software Developers  
**Purpose:** Define UI components, behaviors, states, and validation rules for the User Management screen.

---

## 1. Overview

The **User Management** screen allows authorized users ,admins, to manage system users.  
Core capabilities include:

- Viewing a list of users
- Filtering and sorting users
- Creating a new user
- Editing an existing user
- Assigning roles
- Enabling or disabling users
- Hiding disabled users from the list

This document specifies **UI behavior only**. Authentication, password management, and invitation flows are out of scope.

---

## 2. Page Layout

The page consists of three main areas:

1. **Top Action Bar**
2. **Left Panel – User List**
3. **Right Panel – User Details Form**

### 2.1 Layout Structure

- Desktop / Tablet:
  - Two-column layout
  - Left: User list table
  - Right: User detail form
- Mobile (optional implementation):
  - Stacked layout or tab-based navigation

---

## 3. Initial Page State (On Load)

### 3.1 Default View

When the page loads:

- The **User List Table** is populated with users.
- The **Hide Disabled User** checkbox is **checked by default**.
- The **User Details Panel** is in **New User mode** with empty fields.
- No user row is selected initially.

### 3.2 Loading State

- While loading users:
  - Show a loading indicator or skeleton rows in the table.
  - Disable row selection.
- While loading roles:
  - Disable the User Roles dropdown and show a loading indicator.

### 3.3 Empty State

If no users exist or match filters:

- User list shows:  
  > “No users found.”
- User details panel remains available for creating a new user.

---

## 4. Top Action Bar

### 4.1 New User Button

- **Label:** `+ New User`
- **Location:** Top-left
- **Behavior:**
  - Clears the form
  - Switches to **Create (New User) mode**
  - Clears any table selection
- **Unsaved Changes:**
  - If form has unsaved changes, prompt user before clearing

---

### 4.2 Hide Disabled User Checkbox

- **Label:** `Hide Disabled User`
- **Default State:** Checked
- **Behavior:**
  - Checked → show only enabled users
  - Unchecked → show all users
- **Scope:** Affects only the user list table
- **Note:**
  - If the currently edited user becomes hidden due to this filter, the detail form remains unchanged

---

### 4.3 Save User Button

- **Label:** `Save User`
- **Location:** Top-right
- **Enabled When:**
  - Form is valid
  - Form has changes 
- **Behavior:**
  - Shows loading state while saving
  - Disables form inputs during save
- **On Success:**
  - Show success message
  - Update or insert row in the table
  - Select newly created user
- **On Error:**
  - Show form-level error message
  - Highlight field-level errors

---

## 5. Left Panel – User List Table

### 5.1 Columns

| Column Name | Description |
|------------|-------------|
| ID | Unique numeric identifier |
| User Name | Username |
| Email | User email address |
| Enabled | Boolean status (Enabled / Disabled) |

---

### 5.2 Sorting

- Each column is sortable
- Sorting toggles between ascending and descending
- Default sort: configurable 

---

### 5.3 Filtering

Each column supports filtering:

- **ID:** Numeric filter
- **User Name:** Text contains
- **Email:** Text contains
- **Enabled:** Boolean selector (Enabled / Disabled / All)

Filters are cumulative (AND logic).

---

### 5.4 Row Selection

- Single row selection only
- Selected row is visually highlighted
- Selecting a row:
  - Loads user data into the detail form
  - Switches form to **Edit mode**

---

### 5.5 Table States

- **Loading:** Skeleton or spinner
- **Empty:** “No users match your filters.”
- **Error:** Error message with retry option

---

## 6. Right Panel – User Detail Form

### 6.1 Modes

| Mode | Description |
|-----|------------|
| New User | Creating a new user |
| Edit User | Editing an existing user |

Form title reflects current mode.

---

### 6.2 Form Fields

#### 6.2.1 Username
- Required
- Must be unique
- Allowed characters: letters, numbers, `.`, `_`, `-`
- Length: 3–50 characters

---

#### 6.2.2 Display Name
- Optional
- Length: up to 100 characters

---

#### 6.2.3 Phone
- Optional
- Supports international format
- Accepts `+`, spaces, `-`, parentheses

---

#### 6.2.4 Email
- Required
- Must be valid email format
- Max length: 254 characters

---

#### 6.2.5 User Roles
- Multi-select dropdown
- Available roles:
  - Guest
  - Admin
  - SuperAdmin
- At least one role required
- Selected roles displayed as tags
- Supports search/filter inside dropdown

---

#### 6.2.6 Enabled
- Checkbox
- Default:
  - New User: Checked
  - Edit User: Reflects existing value

---

## 7. Page Behavior

### 7.1 Selecting a User

- Loads user data into form
- Switches to Edit mode
- Save button disabled until a change is made

---

### 7.2 Creating a User

- Click **New User**
- Enter required fields
- Click **Save User**
- New user appears in table and is selected

---

### 7.3 Editing a User

- Select a user
- Modify fields
- Click **Save User**
- Table updates immediately

---

### 7.4 Unsaved Changes

If user attempts to:
- Select another row
- Click New User
- Navigate away

Show confirmation dialog:

**Title:** Discard changes?  
**Message:** You have unsaved changes. Do you want to discard them?  
**Actions:** Discard / Cancel

---

## 8. Validation and Error Handling

### 8.1 Client-Side Validation

- Required fields must be filled
- Email format validation
- At least one role selected

---

### 8.2 Server-Side Validation 

- Username already exists
- Email already exists
- Invalid role assignment
- Cannot disable last active admin

---

### 8.3 Error Display

- Field-level errors shown below inputs
- Form-level error summary at top
- User input preserved on error

---

## 9. Permissions

- Only Admin/SuperAdmin can access page
- Only SuperAdmin can assign SuperAdmin role
- Prevent self-removal of last admin role

UI should:
- Disable restricted role options
- Show tooltip explaining restriction

---

## 10. Accessibility Requirements

- All inputs have labels
- Keyboard navigation supported
- Proper ARIA attributes for errors
- Sufficient contrast for text and highlights

---

## 11. Non-Functional Requirements

- Supports large user lists 
- No full-page reloads
- Responsive layout
- Clear feedback for all async actions

---

## 12. Data Model

```json
{
  "id": 1,
  "username": "AdminUser",
  "displayName": "Admin User",
  "phone": "+1 555 0100",
  "email": "admin@piworks.net",
  "roles": ["Admin"],
  "enabled": true
}
