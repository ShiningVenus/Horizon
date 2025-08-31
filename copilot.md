# Copilot Development Guide for F-Chat Horizon

This document provides comprehensive guidance for AI-assisted development with GitHub Copilot or other AI coding assistants working on the F-Chat Horizon project.

## Table of Contents

- [Copilot Development Guide for F-Chat Horizon](#copilot-development-guide-for-f-chat-horizon)
  - [Table of Contents](#table-of-contents)
  - [Project Overview](#project-overview)
    - [What is F-Chat Horizon?](#what-is-f-chat-horizon)
    - [Technology Stack](#technology-stack)
    - [Architecture Overview](#architecture-overview)
  - [Codebase Structure](#codebase-structure)
    - [Key Directories](#key-directories)
    - [Component Organization](#component-organization)
    - [File Naming Conventions](#file-naming-conventions)
  - [Development Environment](#development-environment)
    - [Prerequisites](#prerequisites)
    - [Setup Commands](#setup-commands)
    - [Available Scripts](#available-scripts)
  - [Code Style and Conventions](#code-style-and-conventions)
    - [TypeScript Guidelines](#typescript-guidelines)
    - [Vue.js Patterns](#vuejs-patterns)
    - [Formatting Standards](#formatting-standards)
  - [Key Components and Patterns](#key-components-and-patterns)
    - [Core Architecture](#core-architecture)
    - [State Management](#state-management)
    - [Communication Patterns](#communication-patterns)
    - [BBCode System](#bbcode-system)
  - [Common Development Tasks](#common-development-tasks)
    - [Adding New Features](#adding-new-features)
    - [UI Components](#ui-components)
    - [Chat Functionality](#chat-functionality)
    - [Character Management](#character-management)
  - [AI-Assisted Development Best Practices](#ai-assisted-development-best-practices)
    - [Context Awareness](#context-awareness)
    - [Code Generation Guidelines](#code-generation-guidelines)
    - [Testing and Validation](#testing-and-validation)
  - [Build and Deployment](#build-and-deployment)
    - [Electron Builds](#electron-builds)
    - [Mobile Builds](#mobile-builds)
    - [Release Process](#release-process)
  - [Troubleshooting](#troubleshooting)
    - [Common Issues](#common-issues)
    - [Debugging Tips](#debugging-tips)

## Project Overview

### What is F-Chat Horizon?

F-Chat Horizon is a continuation of the heavily customized F-Chat Rising, a modern chat client for F-List. It's an opinionated fork that focuses on:

- **Profile matching** - Automatic compatibility comparison between users
- **Automatic ad posting** - Rotating advertisements on selected channels
- **Link previews** - Image/video previews on hover
- **Caching** - Optimized profile loading and performance
- **Smart filters** - Advanced filtering for ads and posts
- **Enhanced customization** - Extensive theming and personalization options

### Technology Stack

- **Frontend Framework**: Vue.js 2.x with TypeScript
- **Desktop Platform**: Electron
- **Mobile Platform**: Apache Cordova
- **Build System**: Webpack with custom configuration
- **Package Manager**: PNPM
- **Code Quality**: Prettier, TSLint
- **Node.js Version**: v22.13.0 (managed via NVM)

### Architecture Overview

The application follows a modular Vue.js architecture with TypeScript:

```
┌─────────────────────────────────────────┐
│              Frontend (Vue.js)          │
├─────────────────────────────────────────┤
│  Components │  Chat │  Character │ UI   │
├─────────────────────────────────────────┤
│           Core Services Layer           │
│  WebSocket │ API │ BBCode │ Cache │ Ads │
├─────────────────────────────────────────┤
│            Platform Layer               │
│     Electron Desktop │ Mobile Web       │
└─────────────────────────────────────────┘
```

## Codebase Structure

### Key Directories

```
├── chat/                    # Main chat functionality and UI
│   ├── *.vue               # Vue components for chat features
│   ├── ads/                # Advertisement management
│   ├── character/          # Character-related components
│   └── preview/            # Link preview functionality
├── components/             # Reusable UI components
│   ├── Modal.vue           # Modal dialog component
│   ├── Dropdown.vue        # Dropdown component
│   └── *.vue              # Other shared components
├── fchat/                  # Core F-Chat protocol implementation
│   ├── characters.ts       # Character data management
│   ├── channels.ts         # Channel functionality
│   └── connection.ts       # WebSocket connection handling
├── bbcode/                 # BBCode parsing and rendering
│   ├── core.ts            # Core BBCode parser
│   └── standard.ts        # Standard BBCode tags
├── helpers/               # Utility functions and helpers
├── scss/                  # Styling and themes
│   ├── themes/            # Theme definitions
│   └── abstracts/         # SCSS mixins and variables
├── electron/              # Electron-specific code and build system
└── mobile/                # Mobile platform code
```

### Component Organization

- **75 Vue Components** - Organized by feature and reusability
- **94 TypeScript Files** - Strong typing throughout the codebase
- **Modular Structure** - Clear separation between UI, logic, and data layers

### File Naming Conventions

- **Vue Components**: PascalCase (e.g., `ChatView.vue`, `UserMenu.vue`)
- **TypeScript Files**: camelCase (e.g., `common.ts`, `profile_api.ts`)
- **Directories**: lowercase with hyphens (e.g., `sound-themes/`)

## Development Environment

### Prerequisites

```bash
# Required tools
Node.js v22.13.0 (via NVM)
PNPM package manager
Git
```

### Setup Commands

```bash
# Clone and setup
git clone https://github.com/Fchat-Horizon/Horizon.git
cd Horizon
pnpm install

# Start development
pnpm watch    # Watch mode for development
pnpm start    # Start Electron application
```

### Available Scripts

| Script       | Description                          |
| ------------ | ------------------------------------ |
| `pnpm watch` | Development build with file watching |
| `pnpm start` | Launch Electron application          |
| `pnpm lint`  | Format code with Prettier            |
| `pnpm check` | Check code formatting                |

## Code Style and Conventions

### TypeScript Guidelines

```typescript
// Interfaces should be clearly defined
export interface Character {
  id: number;
  name: string;
  status: 'online' | 'looking' | 'busy' | 'away' | 'dnd';
}

// Use strict typing
class CharacterManager {
  private characters: Map<number, Character> = new Map();

  public getCharacter(id: number): Character | undefined {
    return this.characters.get(id);
  }
}

// Prefer readonly where appropriate
export const CONFIG = {
  maxMessageLength: 50000,
  defaultTheme: 'default'
} as const;
```

### Vue.js Patterns

```vue
<template>
  <!-- Use semantic HTML structure -->
  <div class="chat-message" :class="messageClasses">
    <div class="message-content">
      {{ formattedMessage }}
    </div>
  </div>
</template>

<script lang="ts">
  import Vue from 'vue';
  import { Component, Prop } from 'vue-property-decorator';

  @Component
  export default class ChatMessage extends Vue {
    @Prop({ required: true })
    public readonly message!: string;

    @Prop({ default: 'normal' })
    public readonly type!: 'normal' | 'system' | 'warning';

    // Computed properties for reactive data
    public get messageClasses(): Record<string, boolean> {
      return {
        'system-message': this.type === 'system',
        'warning-message': this.type === 'warning'
      };
    }

    public get formattedMessage(): string {
      return this.message.trim();
    }
  }
</script>
```

### Formatting Standards

Based on `.prettierrc` configuration:

```javascript
{
  "useTabs": false,           // Use spaces, not tabs
  "tabWidth": 2,              // 2 space indentation
  "printWidth": 80,           // 80 character line limit
  "semi": true,               // Always use semicolons
  "singleQuote": true,        // Single quotes for strings
  "trailingComma": "none",    // No trailing commas
  "arrowParens": "avoid",     // Avoid parens for single params
  "vueIndentScriptAndStyle": true  // Indent Vue script/style blocks
}
```

## Key Components and Patterns

### Core Architecture

**WebSocket Communication**:

```typescript
// chat/WebSocket.ts pattern
export class WebSocketManager {
  private socket: WebSocket | null = null;
  private messageQueue: string[] = [];

  public send(command: string, data?: any): void {
    const message = JSON.stringify({ command, data });
    if (this.socket?.readyState === WebSocket.OPEN) {
      this.socket.send(message);
    } else {
      this.messageQueue.push(message);
    }
  }
}
```

**Character Management**:

```typescript
// Pattern from fchat/characters.ts
export class CharacterStore {
  private characters = new Map<string, Character>();

  public updateCharacter(name: string, data: Partial<Character>): void {
    const existing = this.characters.get(name);
    if (existing) {
      Object.assign(existing, data);
    } else {
      this.characters.set(name, data as Character);
    }
  }
}
```

### State Management

The application uses Vue's reactive system with a store pattern:

```typescript
// Common pattern for state management
export class GlobalState {
  @observable public currentCharacter: Character | null = null;
  @observable public channels: Channel[] = [];
  @observable public conversations: Conversation[] = [];

  public setCurrentCharacter(character: Character): void {
    this.currentCharacter = character;
    // Trigger reactive updates
  }
}
```

### Communication Patterns

**Event-driven Architecture**:

```typescript
// Use Vue's event system for component communication
export default class ChatComponent extends Vue {
  public sendMessage(message: string): void {
    this.$emit('message-sent', { message, timestamp: Date.now() });
  }
}

// Parent component listens
<ChatComponent @message-sent="handleMessage" />
```

### BBCode System

The BBCode system is modular and extensible:

```typescript
// bbcode/core.ts pattern
export class BBCodeParser {
  private tags = new Map<string, BBCodeTag>();

  public addTag(tag: BBCodeTag): void {
    this.tags.set(tag.name, tag);
  }

  public parse(text: string): HTMLElement {
    // Parsing logic
    return this.processText(text);
  }
}

// Adding custom tags
parser.addTag(new BBCodeSimpleTag('b', 'strong'));
parser.addTag(
  new BBCodeCustomTag('spoiler', (parser, parent) => {
    // Custom spoiler logic
  })
);
```

## Common Development Tasks

### Adding New Features

1. **Identify the Feature Scope**:
   - UI components needed
   - Backend integration required
   - State management changes

2. **Create Component Structure**:

   ```bash
   # For new chat features
   touch chat/NewFeature.vue
   touch chat/interfaces.ts  # Add interfaces if needed
   ```

3. **Follow the Pattern**:

   ```vue
   <!-- chat/NewFeature.vue -->
   <template>
     <div class="new-feature">
       <!-- UI implementation -->
     </div>
   </template>

   <script lang="ts">
     import Vue from 'vue';
     import { Component } from 'vue-property-decorator';

     @Component
     export default class NewFeature extends Vue {
       // Component logic
     }
   </script>

   <style scoped>
     .new-feature {
       /* Component-specific styles */
     }
   </style>
   ```

### UI Components

**Reusable Component Pattern**:

```vue
<!-- components/CustomButton.vue -->
<template>
  <button :class="buttonClasses" :disabled="disabled" @click="handleClick">
    <slot />
  </button>
</template>

<script lang="ts">
  import Vue from 'vue';
  import { Component, Prop, Emit } from 'vue-property-decorator';

  @Component
  export default class CustomButton extends Vue {
    @Prop({ default: 'primary' })
    public readonly variant!: 'primary' | 'secondary' | 'danger';

    @Prop({ default: false })
    public readonly disabled!: boolean;

    @Emit('click')
    public handleClick(event: Event): Event {
      return event;
    }

    public get buttonClasses(): Record<string, boolean> {
      return {
        btn: true,
        [`btn-${this.variant}`]: true,
        disabled: this.disabled
      };
    }
  }
</script>
```

### Chat Functionality

**Message Processing Pattern**:

```typescript
// chat/message_view.ts pattern
export class MessageProcessor {
  public processMessage(message: string): ProcessedMessage {
    // 1. Parse BBCode
    const parsed = this.bbcodeParser.parse(message);

    // 2. Apply filters
    const filtered = this.applyFilters(parsed);

    // 3. Generate display elements
    return this.createDisplayMessage(filtered);
  }

  private applyFilters(message: ParsedMessage): FilteredMessage {
    // Smart filtering logic
    return message;
  }
}
```

### Character Management

**Character Data Handling**:

```typescript
// Pattern for character operations
export class CharacterService {
  public async loadCharacterProfile(name: string): Promise<Character> {
    // 1. Check cache first
    const cached = this.cache.get(name);
    if (cached && !this.isStale(cached)) {
      return cached;
    }

    // 2. Fetch from API
    const profile = await this.api.getCharacter(name);

    // 3. Update cache
    this.cache.set(name, profile);

    return profile;
  }
}
```

## AI-Assisted Development Best Practices

### Context Awareness

When working with AI tools on this project, provide this context:

```markdown
Project: F-Chat Horizon - Vue.js chat client with TypeScript
Architecture: Vue 2.x + TypeScript + Electron
Patterns: Component-based, reactive state, event-driven
Constraints: 80-char lines, single quotes, no trailing commas
Focus: Chat features, character management, BBCode parsing
```

### Code Generation Guidelines

**Good AI Prompts**:

```
"Create a Vue component for displaying user status with TypeScript decorators,
following the project's patterns from ChatView.vue"

"Add a new BBCode tag for highlighting text, similar to the existing 'color'
tag in bbcode/standard.ts"

"Implement character search filtering using the existing CharacterService
pattern with proper TypeScript types"
```

**Provide Context Files**:

- Reference similar existing components
- Include relevant interface definitions
- Show the expected pattern usage

### Testing and Validation

Always validate AI-generated code:

1. **Type Checking**: `tsc --noEmit` (implied by build process)
2. **Formatting**: `pnpm lint` to auto-format
3. **Manual Testing**: Start the app and test the feature
4. **Integration**: Ensure it works with existing components

## Build and Deployment

### Electron Builds

Development builds:

```bash
cd electron
pnpm install
pnpm watch      # Development mode with hot reload
pnpm start      # Launch the application
```

Production builds:

```bash
cd electron
pnpm build:linux    # Linux packages
pnpm build:mac      # macOS packages
pnpm build:win      # Windows packages
```

### Mobile Builds

Mobile platform using Cordova:

```bash
cd mobile
# Mobile-specific build commands
```

### Release Process

The project follows semantic versioning:

- **v1.2.3** - Production releases
- **v1.2.3-BETA-1** - Pre-release testing
- **v1.2.3-rc-1** - Release candidates
- **v1.2.3-DEV-1** - Development builds

## Troubleshooting

### Common Issues

**TypeScript Errors**:

```bash
# Check for type issues
npx tsc --noEmit

# Common fix: Update interface definitions
# in interfaces.ts or component-specific types
```

**Build Failures**:

```bash
# Clean and rebuild
rm -rf node_modules electron/node_modules
pnpm install
cd electron && pnpm install
```

**Vue Component Issues**:

- Ensure proper TypeScript decorators
- Check template syntax for typos
- Verify prop types match interface definitions

### Debugging Tips

1. **Use Vue DevTools** for component inspection
2. **Enable TypeScript strict mode** catches more errors
3. **Check browser console** for runtime errors
4. **Use breakpoints** in VS Code with Electron debugging

---

This guide should help AI assistants understand the F-Chat Horizon codebase structure, conventions, and development patterns for more effective code generation and assistance.
