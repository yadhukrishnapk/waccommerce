# LockedAttributeItem vs DraggableAttributeItem

## LockedAttributeItem (Lines 146–151)
```jsx
<LockedAttributeItem
  key={child.id}
  child={child}
  selectedAttributes={selectedAttributes}
  onAttributeSelect={onAttributeSelect}
/>
```

### Purpose
Renders attributes that are locked/default and cannot be dragged.

### Key Characteristics
✅ Has a checkbox for selection  
🔒 Shows a lock icon on the right side  
🔒 Shows a `file_locked` icon instead of regular file icon  
❌ **Not draggable** – static item  
📌 Used when `child.isDefault === true`  

**Visual:** `[✓] 🔒 Attribute Name 🔒`

---

## DraggableAttributeItem (Lines 156–162)
```jsx
<DraggableAttributeItem
  key={child.id}
  child={child}
  index={index}  // ← Extra prop for drag order
  selectedAttributes={selectedAttributes}
  onAttributeSelect={onAttributeSelect}
/>
```

### Purpose
Renders attributes that can be dragged and reordered.

### Key Characteristics
✅ Has a checkbox for selection  
📄 Shows a regular file icon (not locked)  
✋ **Draggable** – wrapped in `<Draggable>` component from `@hello-pangea/dnd`  
🎨 Shows visual feedback when dragging (blue highlight, shadow)  
🔢 Requires `index` prop for drag-and-drop ordering  
📌 Used when `child.isDefault === false` (or undefined)  

**Visual:** `[✓] 📄 Attribute Name ← Can be grabbed and moved`

---

## 🧩 Side-by-Side Comparison

| Feature | LockedAttributeItem | DraggableAttributeItem |
|----------|--------------------|------------------------|
| Checkbox | ✅ Yes | ✅ Yes |
| Draggable | ❌ No | ✅ Yes |
| Lock Icon | 🔒 Yes (right side) | ❌ No |
| File Icon | 🔒 `file_locked` | 📄 `file` (regular) |
| Use Case | Default/system attributes | User-added attributes |
| Can Reorder | ❌ No | ✅ Yes |
| Props | 3 props | 4 props (includes `index`) |
| Wrapper | `<li>` only | `<Draggable>` + `<li>` |

---

## 🧠 When Each Is Used

The `TreeNode` component decides which one to render based on the `isDefault` property:

```js
const isLocked = child.isDefault === true;

if (isLocked) {
  return <LockedAttributeItem ... />;  // ← Can't drag, has lock icon
}

return <DraggableAttributeItem ... />;  // ← Can drag and reorder
```

---

## ✅ In Summary

| Type | Description |
|------|--------------|
| **LockedAttributeItem** | Static, protected, system attributes |
| **DraggableAttributeItem** | Dynamic, movable, user-managed attributes |

Both support checkbox selection, but only **DraggableAttributeItem** can be reordered via drag-and-drop!
