# Complete Table Component Structure & Working Guide

## Overview

Your project uses a custom **Table** component built on top of **@tanstack/react-table v8.21.3**, a headless UI library for building powerful and customizable tables. The system is modular, scalable, and packed with advanced features.

---

## ✅ Core Architecture

### 📁 Main File Structure

```
src/components/Table/table.jsx                 → Main table component
src/components/Table/pagination.jsx            → Pagination controls
src/components/Table/ColumnVisibilityControls.jsx → Column visibility toggle
src/components/Table/tableShimmer.jsx          → Loading skeleton
src/customHooks/useTableListData.js            → Custom SWR-based fetch hook
src/components/TableTabs/TableTabs.jsx         → Tab filters
src/components/TableText/TableText.jsx         → Cell text renderer
src/components/TableTextWrapper/TableTextWrapper.jsx → Cell wrapper
```

---

## ✅ Key Features

### **1. TanStack React Table Integration**

```js
const table = useReactTable({
  data: rowData || [],
  columns: finalColumns,
  getCoreRowModel: getCoreRowModel(),
  state: { rowSelection, columnVisibility },
  enableRowSelection: isSelectable,
  onRowSelectionChange: setRowSelection,
  onColumnVisibilityChange: setColumnVisibility,
});
```

### ✔ What TanStack Provides

* Headless UI (you fully control markup)
* Row selection
* Column visibility
* Flexible rendering (`flexRender`)
* Controlled state

---

## ✅ 2. Row Selection (Single & Multi-Select)

### **Multi-select mode**

```jsx
<SelectionHeader
  table={table}
  isMultiselectable={true}
  headerCheckboxRef={headerCheckboxRef}
/>
```

✔ Select all rows
✔ Indeterminate checkbox state

### **Single-select mode**

```js
const handleSingleRowSelection = (rowId) => {
  setRowSelection((prev) => (prev[rowId] ? {} : { [rowId]: true }));
};
```

✔ Only one row selected at a time

### Usage

```jsx
<Table
  isSelectable
  isMultiselectable
  onSelectChange={(rows) => setSelectedRows(rows)}
/>
```

---

## ✅ 3. Column Visibility Controls

Located in: **ColumnVisibilityControls.jsx**

### Features

* Toggle column visibility
* Prevent hiding the last visible column
* Reset to defaults
* Dropdown with Framer Motion

---

## ✅ 4. Pagination System

Component: **pagination.jsx**

### Features

* Previous / Next buttons
* Page indicator (e.g., `1 of 10`)
* Per-page selector
* Syncs with URL params automatically

---

## ✅ 5. Data Fetching Hook: `useTableListData`

A powerful SWR-based fetching hook.

### Example

```js
const {
  data,
  isLoading,
  pageCount,
  currentPage,
  totalCount,
  countValue,
  countAndTabData,
  exportEndpointWithParams,
  mutate,
} = useTableListData("/attributes/sets", {
  additionalParams: ['custom_filter'],
  defaultValues: { per_page: 20 },
  enableFilterApi: true,
});
```

### How queries are built

```js
const params = new URLSearchParams({ page_size: perPageItems, page });
```

Supports:
✅ Pagination
✅ Search
✅ Multiple filters
✅ Custom params

---

## ✅ 6. Table Tabs (Status Filters)

Example:

```jsx
tabs=[
  { value: "", label: "All", key: "status" },
  { value: "active", label: "Active", key: "status", count: 10 },
]
```

✔ Syncs with URL
✔ Badge counts

---

## ✅ 7. Search & Dropdown Filters

```jsx
<DropdownSearch options={dropDownTabData || []} />
```

Search synced with route params.

---

## ✅ 8. Row Click Navigation

```js
<Table onRowClick="/products/:id" />
```

Replaces params automatically.

Or pass a function:

```js
<Table onRowClick={(row) => console.log(row)} />
```

---

## ✅ 9. Sticky Actions Column

Automatically wrapped:

```js
cell: (props) => <ActionsCell cellRenderer={column.cell} props={props} />
```

✔ Sticky right
✔ Shadow
✔ Click propagation blocked

---

## ✅ 10. Loading State

Using **TableShimmer.jsx** (react-loading-skeleton).

---

## ✅ 11. Empty States

```jsx
<NoResults label={queryParam} />
```

Or custom label.

---

## ✅ 12. Selected Row Actions

```jsx
<Table
  selectedRowActionsComponent={
    selectedRows.length > 0 && (
      <BulkDeleteButton selectedRows={selectedRows} />
    )
  }
/>
```

---

## ✅ 13. Cell Rendering Components

### **TableText.jsx**

✔ Text with optional image
✔ Line clamping

### **TableTextWrapper.jsx**

✔ Uniform padding

---

# ✅ How to Use the Table

## ✅ Basic Example

```jsx
<Table
  rowData={rowData}
  columns={columns}
  isLoading={false}
  pageCount={1}
  currentPage={1}
  totalCount={2}
  countValue={10}
/>
```

---

# ✅ Advanced Example (Full Features)

```jsx
<Table
  rowData={data?.data?.items || []}
  columns={columns}
  currentPage={currentPage}
  totalCount={totalCount}
  pageCount={pageCount}
  countValue={countValue}
  isLoading={isLoading}
  isSelectable
  isMultiselectable
  onSelectChange={setSelectedRows}
  tabs={tabs}
  onRowClick="/products/:id"
  selectedRowActionsComponent={selectedRows.length > 0 && <BulkActions />}
  filterChildComponent={<CustomFilter />}
  modalChild={<AddNewButton />}
  classNameWrapper="custom-wrapper"
  hasOuterPadding
/>
```

---

# ✅ Props Reference

| Prop                        | Type            | Default  | Description             |
| --------------------------- | --------------- | -------- | ----------------------- |
| rowData                     | Array           | []       | Table data              |
| columns                     | Array           | []       | Column definitions      |
| currentPage                 | Number          | -        | Active page             |
| totalCount                  | Number          | -        | Total items             |
| isLoading                   | Boolean         | false    | Loading shimmer         |
| pageCount                   | Number          | -        | Total pages             |
| countValue                  | Number          | -        | Items per page          |
| tabs                        | Array           | -        | Tabs for filtering      |
| dropDownTabData             | Array           | []       | Dropdown search options |
| isSelectable                | Boolean         | false    | Enable row selection    |
| isMultiselectable           | Boolean         | false    | Multi-select mode       |
| onSelectChange              | Function        | () => {} | Selection callback      |
| onRowClick                  | String/Function | null     | Row click handler       |
| disableRowClick             | Boolean         | false    | Disable navigation      |
| filterChildComponent        | Node            | -        | Custom filter UI        |
| modalChild                  | Node            | null     | Add button / modal      |
| selectedRowActionsComponent | Node            | null     | Bulk actions UI         |
| noDataLabel                 | String          | -        | Custom empty text       |
| clampText                   | Boolean         | false    | Clamp cell text         |
| classNameWrapper            | String          | ""       | Wrapper CSS             |
| hasOuterPadding             | Boolean         | true     | Add container padding   |

---

# ✅ Key TanStack Features Used

* Core row model
* Row selection
* Column visibility
* flexRender
* Controlled state

---

# ✅ Data Flow Diagram

```
URL Params → useSearchParams()
       ↓
useTableListData() → Builds API query → SWR fetch
       ↓
Response → rowData + pagination + filters
       ↓
<Table /> renders using TanStack
       ↓
User Actions → Update URL → Refetch
```

---

# ✅ Advanced Patterns

### Custom status cell

```js
const renderStatusCell = (info) => (
  <Status variant={info.getValue() ? "success" : "error"} />
);
```

### Action cell with refetch

```js
<ActionCell mutate={mutate} row={info.row.original} />
```

### Conditional UI

```jsx
{selectedRows.length > 0 && <SelectedRowActions />}
```

---

# ✅ Conclusion

This Table system is **production-ready**, scalable, and handles complex enterprise-level requirements including pagination, filtering, search, row selection, visibility controls, and more — powered by SWR and TanStack Table.
