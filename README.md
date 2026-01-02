# TaskGlitch - Bug Fixes & Improvements

This project is a Task Management Web App for sales teams, prioritizing tasks based on ROI. This repository contains the fixed version of the app, addressing several critical bugs and security issues.

## 🛠️ Bug Fixes

### 1. Double Fetch Issue
- **Problem:** The app was calling the task fetch API twice on load due to a duplicate `useEffect` and React Strict Mode.
- **Solution:** Removed the redundant `useEffect` in `useTasks.ts` and cleaned up the initialization logic to ensure only one fetch occurs. Also removed intentional malformed data injection.

### 2. Undo Snackbar Bug
- **Problem:** Closing the snackbar didn't clear the "last deleted task" state, allowing old tasks to be recovered incorrectly.
- **Solution:** Implemented `clearLastDeleted` in the `TasksContext` and `useTasks` hook. Updated `App.tsx` to trigger this cleanup whenever the snackbar is closed.

### 3. Unstable Sorting
- **Problem:** Tasks with identical ROI and Priority shuffled randomly on re-renders.
- **Solution:** Added a deterministic tie-breaker in `logic.ts`. Tasks are now sorted by ROI (desc), then Priority (desc), and finally by Title (alphabetical) and ID (alphabetical) for consistent ordering.

### 4. Double Dialog Opening
- **Problem:** Clicking "Edit" or "Delete" triggered both the action dialog and the "View Details" dialog due to event bubbling.
- **Solution:** Added `e.stopPropagation()` to the click handlers of the Edit and Delete buttons in `TaskTable.tsx`.

### 5. ROI Calculation & Validation
- **Problem:** Division by zero errors occurred when time taken was 0, and inputs were too restrictive.
- **Solution:** Fixed `computeROI` in `logic.ts` to return `0` if `timeTaken` is `0`. Updated `TaskForm.tsx` and `TaskDetailsDialog.tsx` to allow `0` as a valid input for Time Taken, ensuring safe and accurate calculations.

### 🛡️ Security Improvement (XSS Fix)
- **Problem:** Task notes were rendered using `dangerouslySetInnerHTML`, posing a Cross-Site Scripting risk.
- **Solution:** Switched to standard Typography children rendering to ensure all user input is properly escaped.

## 🚀 How to Run the Project

### Prerequisites
- Node.js (v18+ recommended)
- npm

### Installation
```bash
npm install
```

### Development Server
```bash
npm run dev
```
The app will be available at `http://localhost:5173`.

### Production Build
```bash
npm run build
```
The production-ready files will be in the `dist` directory.

## 🧪 Testing

The project's core logic has been verified for:
1. **ROI Safety:** Verified that `revenue / 0` results in `0` rather than `Infinity`.
2. **Stable Sorting:** Verified that identical tasks maintain a consistent order.
3. **Type Safety:** The project passes `tsc` (TypeScript) checks without errors.
