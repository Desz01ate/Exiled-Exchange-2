# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Architecture Overview

This is **Exiled Exchange 2**, a Path of Exile 2 overlay application forked from Awakened PoE Trade. The project uses an **Electron-based architecture** with two main components that depend on each other:

### Core Components

1. **renderer/** - Vue.js frontend application (HTML/JavaScript UI)
   - Uses Vue 3.2.37 with TypeScript
   - Built with Vite and Tailwind CSS
   - Contains all UI components, widgets, and price checking logic
   - Located at `renderer/src/web/`

2. **main/** - Electron main process (Node.js backend)
   - Handles system integration, keyboard shortcuts, window management
   - Manages game integration and overlay functionality
   - Located at `main/src/`

### Key Architecture Patterns

- **Widget System**: Core UI components are organized as widgets (price-check, item-check, map-check, stash-search, etc.)
- **Parser System**: Item parsing logic in `renderer/src/parser/` handles Path of Exile item text parsing
- **Trade Integration**: Multiple trade APIs supported (pathofexile.com, poe.ninja, poeprices.info)
- **IPC Communication**: Electron IPC for renderer-main communication via `renderer/src/web/background/IPC.ts`

## Development Commands

### Development Workflow
```bash
# Start development (requires 2 terminals)
# Terminal 1:
cd renderer
npm install
npm run make-index-files
npm run dev

# Terminal 2:
cd main
npm install
npm run dev
```

### Build Commands
```bash
# Full production build
cd renderer
npm install
npm run make-index-files
npm run build

cd ../main
npm install
npm run build
npm run package
```

### Testing and Linting
```bash
# Renderer testing/linting
cd renderer
npm run test      # Run Vitest tests
npm run lint      # ESLint checking
npm run lint-fix  # Fix ESLint issues
npm run format    # Prettier formatting

# Main linting
cd main
npm run lint      # ESLint checking
npm run fix       # Fix ESLint issues
npm run format    # Prettier formatting
```

### Quick Build Script
```bash
# Use the provided build script
./testUpdate.sh
```

## Key Directories

- `renderer/src/web/price-check/` - Main price checking functionality
- `renderer/src/web/overlay/` - Overlay window and widget management
- `renderer/src/parser/` - Item parsing and text analysis
- `main/src/shortcuts/` - Keyboard shortcut handling
- `main/src/windowing/` - Window management and overlay positioning
- `dataParser/` - External data processing scripts (Python/TypeScript)

## Data Flow

1. User triggers hotkey → Main process captures → IPC to renderer
2. Renderer parses item text → Queries trade APIs → Displays results
3. Widget system manages overlay positioning and user interactions
4. Configuration stored in Electron's app data directory

## Important Notes

- Both `renderer` and `main` must be running for development
- Always run `npm run make-index-files` in renderer before building
- The app integrates with Path of Exile 2 game client
- Extensive internationalization support (multiple languages in `renderer/public/data/`)