# Git-Like Workflow Design

## Concept
Users see changes immediately in their local view (like `git checkout -b feature`), but changes are staged until a PR is created. No manual branch management - the system handles it automatically.

## User Experience

### Current State (Main Branch)
- User sees the "real" state from main branch
- All docs/folders reflect what's in GitHub main

### Staging Changes (Local Branch)
- User creates folder → **Appears immediately** in file navigator
- User moves doc → **Moves immediately** in file navigator  
- User renames doc → **Renamed immediately** in file navigator
- All changes are "staged" and shown in Changes Panel
- Changes are local only - not in GitHub yet

### Creating PR
- User clicks "Create PR" → All staged changes committed to feature branch
- PR created on GitHub
- User's local view stays the same (still on feature branch view)

### After PR Merge
- Webhook triggers → Sync updates database
- User's view updates to reflect merged changes
- Staged changes cleared

## Implementation

### 1. Optimistic UI Updates
- `DocsNav` maintains local state of file structure
- When operation is staged, immediately update local view
- Track which items are "local" (not yet in GitHub)

### 2. Branch Management
- Each set of staged changes = one feature branch
- Branch name: `docs/user-{timestamp}-{random}`
- User never sees branch names
- System automatically creates/manages branches

### 3. Visual Indicators
- Staged items could have a subtle indicator (dot, border)
- Changes Panel shows what will be in PR
- Clear distinction between "real" and "staged" changes

### 4. State Management
- `useDocsList` - fetches from API (real state)
- `useFileChanges` - tracks staged operations
- `useLocalDocsView` - merges real + staged for display
- Optimistic updates applied immediately

## Benefits
- ✅ Familiar Git workflow (staging → commit → PR)
- ✅ Immediate feedback (see changes right away)
- ✅ No branch management complexity
- ✅ Can review all changes before creating PR
- ✅ Can undo/remove staged changes

