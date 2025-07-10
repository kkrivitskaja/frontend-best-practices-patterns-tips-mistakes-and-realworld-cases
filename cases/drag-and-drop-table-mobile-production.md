# Production-ready Drag-and-Drop Table with Mobile Support in React

****  
How to build a robust, accessible, and mobile-friendly drag-and-drop table in React, with real-world solutions for overlays, touch UX, and clean DOM.

---

## Problem

- Need a sortable table with drag-and-drop for files/items.
- Must work perfectly on both desktop and mobile.
- Clean, production-quality DOM (no div-in-table, no layout bugs).
- Mobile UX: drag only by handle, scroll everywhere else, no accidental drags.

---

## Challenges

- dnd-kit overlays not matching table rows (especially with resizable columns).
- Mobile drag interfering with scroll (dnd-kit vs react-movable).
- Making drag handle accessible and touch-friendly.
- Avoiding DOM nesting errors (div inside table, etc).
- Keeping code clean and maintainable.

---

## Solutions

### 1. Unified Row Rendering for Overlay
- Use the same row rendering logic for both table and drag overlay.
- Make the original row transparent/hidden during drag.

### 2. Mobile Drag Handle
- Only the handle is draggable on mobile (`touch-action: none`).
- The rest of the card/table row is scrollable.
- Large, accessible handle (min 40x40px), with `cursor-grab` and ARIA label.

### 3. Sensors and UX
- Use only `TouchSensor` on mobile, `PointerSensor` on desktop.
- Increase `tolerance` for touch to avoid accidental drags.
- No drag on tap, only on explicit swipe/handle.

### 4. Clean DOM
- No divs inside `<table>`, only valid table structure.
- All styling via Tailwind, no inline styles except for transforms.

### 5. State Management
- Local state for mobile drag-and-drop (mock data for testing).
- Redux or global state for desktop.

---

## Key Code Snippets

```tsx
// Example: Draggable handle for mobile
<span
  {...dragHandleProps}
  style={{ touchAction: 'none' }}
  className="w-14 h-14 cursor-grab"
  aria-label="Drag to reorder"
>
  <span className="text-2xl">⠿</span>
</span>
```

```tsx
// Sensors setup
const isMobile = typeof window !== 'undefined' && window.innerWidth < 640;
const sensors = useSensors(
  isMobile
    ? useSensor(TouchSensor, { activationConstraint: { delay: 0, tolerance: 15 } })
    : useSensor(PointerSensor, { activationConstraint: { distance: 5 } })
);
```

---

## Lessons Learned

- Always separate mobile and desktop drag logic for best UX.
- Use a dedicated drag handle on mobile to avoid scroll conflicts.
- Memoize overlay row rendering to avoid DOM glitches.
- Validate your DOM structure for accessibility and production-readiness.
- Test on real devices, not just in browser emulators.

---

## UX Considerations

- **Drag handle must be large and touch-friendly**: Minimum 40x40px, visually distinct, and easy to tap. Users should never have to "hunt" for the handle.
- **Only the handle should be draggable on mobile**: The rest of the row/card should allow scrolling. This prevents accidental drags and preserves natural scroll behavior.
- **Show clear visual feedback**: Change the opacity or style of the row/card being dragged. Use overlays that match the dragged item exactly.
- **Keep the DOM clean**: Avoid extra wrappers or divs inside tables. This helps with accessibility and prevents layout bugs.
- **Accessible by keyboard and screen readers**: Add ARIA labels to drag handles, and ensure focus states are visible.
- **No drag on tap**: Drag should only start on explicit swipe or handle interaction, never on a simple tap/click.
- **Auto-scroll support**: When dragging near the edge of a scrollable container, auto-scroll should activate smoothly.
- **Performance**: Avoid unnecessary re-renders during drag. Memoize overlay rows and use lightweight state updates.
- **Test on real devices**: Emulators are not enough. Test on both iOS and Android, with different browsers and screen sizes.
- **Communicate limitations**: If drag-and-drop is not available (e.g., on some browsers or devices), provide alternative controls or clear messaging.

---

## References

- [dnd-kit documentation](https://docs.dndkit.com/)
- [React Table](https://tanstack.com/table/v8)
- [Drag-and-Drop UX: Guidelines and Best Practices](https://smart-interface-design-patterns.com/articles/drag-and-drop-ux/)
- [Drag and Drop for Design Systems](https://marvelapp.com/blog/drag-drop-design-systems/)
- [Exploring the challenges in creating an accessible sortable list (drag-and-drop)](https://github.blog/engineering/user-experience/exploring-the-challenges-in-creating-an-accessible-sortable-list-drag-and-drop/)
- [The Road to Accessible Drag and Drop (Part 1)](https://www.tpgi.com/the-road-to-accessible-drag-and-drop-part-1/)
- [The Road to Accessible Drag and Drop (Part 2)](https://www.tpgi.com/the-road-to-accessible-drag-and-drop-part-2/)
- [10 Tips for Accessible Drag-and-Drop Interfaces](https://fleexy.dev/blog/10-tips-for-accessible-drag-and-drop-interfaces/)
- [Drag and Drop Accessibility: A Complete Guide](https://www.continualengine.com/blog/drag-and-drop-accessibility/)
- [Understanding WCAG Guidelines for Drag and Drop Accessibility Compliance]([https://www.continualengine.com/blog/drag-and-drop-accessibility/](https://accessibilityspark.com/drag-and-drop-accessibility/))



---

**If you found this case useful, please ⭐️ the repo** 

