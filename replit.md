# Overview

Artist Pro is a professional live performance management application for musicians, enabling them to manage multiple setlists, track songs during live shows, and display lyrics with chords using a teleprompter interface. It features a shared, style-organized music library and is optimized for quick navigation and readability in dark mode for stage use.

The application includes five fully functional modules: SETLISTS (live performance management), CHECKLIST (pre/post-show task management), AGENDA (event scheduling), COMPOSITIONS (original music management), and FINANCIAL REPORTS (revenue and expense tracking).

**IMPORTANT**: The application now requires user authentication via Replit Auth and a paid subscription via Stripe to access all features. All data is stored in PostgreSQL and scoped to individual users.

# User Preferences

Preferred communication style: Simple, everyday language (Portuguese).

# System Architecture

## Navigation Flow
The application follows a multi-tier authentication and access control flow:

### For Non-Authenticated Users:
1. **Landing Page** (`/`) - Marketing page with login button and feature showcase

### For Authenticated But Unpaid Users:
1. **Subscribe Page** (`/subscribe`) - Stripe checkout for monthly subscription (R$ 19,90/mês)

### For Authenticated and Paid Users:
1. **Dashboard** (`/`) - Main hub for all five modules
2. **Setlists Management** (`/setlists`) - List view of setlists with statistics
3. **Individual Setlist** (`/setlist/:id`) - Detailed setlist management, including songs, library access, and teleprompter
4. **Checklist** (`/checklist`) - Pre/post-show task management
5. **Agenda** (`/agenda`) - Event scheduling and management
6. **Compositions** (`/compositions`) - Original music ideas management with list view
7. **Individual Composition** (`/composition/:id`) - Detailed composition view with audio player and chord detection
8. **Financial** (`/financial`) - Revenue and expense tracking

### Authentication Routes:
- `/api/login` - Initiates Replit Auth login flow (supports Google, GitHub, email)
- `/api/logout` - Logs user out and redirects to landing
- `/api/callback` - OAuth callback handler

## Frontend Architecture
**Technology Stack**: React with TypeScript, using Vite.
**UI Framework**: shadcn/ui components built on Radix UI primitives with Tailwind CSS for styling.
**Routing**: Client-side routing with Wouter.
**State Management**: React hooks with TanStack Query for server state management. Authentication state managed via `useAuth` hook that queries `/api/auth/user`.
**Design System**: Custom theme with dark/light mode, glass morphism effects, purple/blue gradient, and specific status indicators (green for unplayed, pink/rose for played). Teleprompter uses a pure black background with white lyrics and purple chords.

## Backend Architecture
**Server Framework**: Express.js on Node.js with TypeScript.
**API Design**: RESTful API under `/api`.
**Storage Layer**: PostgreSQL database via Drizzle ORM with type-safe queries and Zod validation. All data is user-scoped via `userId` foreign keys.
**Authentication**: Replit Auth (OpenID Connect) with session management via `express-session` and `connect-pg-simple` for PostgreSQL session storage.
**Payment Processing**: Stripe subscriptions with webhook verification for automatic subscription status updates.

## Data Models
Core data models include:
-   **User**: Authentication and subscription data (id, email, firstName, lastName, profileImageUrl, stripeCustomerId, stripeSubscriptionId, isPaid, subscriptionStatus)
-   **Session**: Express session storage for authentication
-   **Setlist**: Manages performance events (user-scoped via userId foreign key)
-   **Song**: Stored in a shared library, reusable across setlists (user-scoped via userId foreign key)
-   **SetlistSong**: Many-to-many relationship tracking song order and played status within a setlist
-   **ChecklistItem**: For pre/post-show task management (user-scoped via userId foreign key)
-   **AgendaEvent**: For scheduling and tracking events (user-scoped via userId foreign key)
-   **Composition**: For managing original musical works (user-scoped via userId foreign key)
-   **FinancialRecord**: For tracking event-based revenue and expenses (user-scoped via userId foreign key)
Zod schemas ensure type safety and runtime validation.

## Key Features

### Authentication & Payment
-   **User Authentication**: Replit Auth with support for Google, GitHub, and email/password login
-   **Subscription Management**: Monthly subscription (R$ 19,90) via Stripe with automatic renewal
-   **Webhook Processing**: Secure webhook signature verification for real-time subscription status updates
-   **Access Control**: Multi-tier protection (unauthenticated → authenticated → paid subscriber)
-   **User Profile**: Avatar, name, email display in header with logout functionality

### Core Features (Paid Subscribers Only)
-   **Multi-Setlist Management**: Create, organize, and track statistics for various performance occasions, with independent song playback status per setlist
-   **Shared Music Library**: Centralized repository for songs, filterable by musical style, allowing easy addition to setlists
-   **Song Management**: Songs can be added individually, in bulk, or from the shared library. Editing focuses on lyrics, notes, and style
-   **Chord Recognition System**: Professional inline chord detection (300+ valid chords) displaying chords and lyrics on the same line with specific color coding and a monospace font for readability
-   **Teleprompter Mode**: Full-screen, pure black background display of lyrics with inline chords, monospace font, keyboard navigation, and auto-advance
-   **Checklist Management**: Separate pre/post-show task lists with quick add, visual status, reordering, and persistent storage
-   **Agenda Management**: Comprehensive event scheduling with CRUD operations, detailed event information, and chronological sorting
-   **Compositions Management**: Store original music ideas with integrated audio player, lyrics with inline chord detection (purple-highlighted chords), notes section, and Cifra Club-style presentation
-   **Financial Reports**: Track event-based revenue and expenses, providing summary dashboards, profit calculation, and BRL currency formatting
-   **User Data Isolation**: All data is private and scoped to individual users
-   **Responsive Design**: Mobile-first approach for adaptable layouts

# External Dependencies

**UI Component Libraries**:
-   @radix-ui/* (accessible UI primitives)
-   Tailwind CSS (utility-first CSS framework)
-   shadcn/ui (component patterns)

**State & Data Management**:
-   @tanstack/react-query (server state management)
-   react-hook-form & @hookform/resolvers (form validation)
-   Zod (schema validation)
-   drizzle-zod (Drizzle ORM integration with Zod)

**Database**:
-   @neondatabase/serverless (PostgreSQL driver)
-   drizzle-orm (TypeScript ORM)
-   Drizzle Kit (migrations)

**Authentication & Payments**:
-   openid-client (OpenID Connect for Replit Auth)
-   passport & passport-local (authentication middleware)
-   express-session & connect-pg-simple (session management)
-   stripe & @stripe/stripe-js & @stripe/react-stripe-js (payment processing)

**Build & Development**:
-   Vite (build tool and dev server)
-   TypeScript (type safety)
-   @vitejs/plugin-react (React integration)

**Routing & Navigation**:
-   wouter (lightweight client-side routing)

**Styling Utilities**:
-   class-variance-authority (component variant management)
-   clsx & tailwind-merge (conditional class composition)

**Icons**:
-   lucide-react (icon library)

**Date Handling**:
-   date-fns (date utility library)

# Environment Variables & Secrets

The application requires the following environment variables configured in Replit Secrets:

### Database (Auto-configured by Replit)
- `DATABASE_URL` - PostgreSQL connection string
- `PGHOST`, `PGPORT`, `PGUSER`, `PGPASSWORD`, `PGDATABASE` - Individual PostgreSQL credentials

### Authentication (Auto-configured by Replit)
- `SESSION_SECRET` - Express session encryption key
- `REPL_ID` - Replit application ID for OAuth
- `ISSUER_URL` - Replit Auth OIDC issuer URL (defaults to https://replit.com/oidc)

### Stripe Payment Processing (User-configured)
- `STRIPE_SECRET_KEY` - Stripe secret API key (starts with `sk_test_...` for test mode)
- `VITE_STRIPE_PUBLIC_KEY` - Stripe publishable key (starts with `pk_test_...` for test mode)
- `STRIPE_WEBHOOK_SECRET` - Stripe webhook signing secret (starts with `whsec_...`)

## Stripe Setup Instructions

1. Create a Stripe account at https://stripe.com
2. Get API keys from https://dashboard.stripe.com/test/apikeys
3. Set up webhook endpoint:
   - Go to https://dashboard.stripe.com/test/webhooks
   - Click "Add endpoint"
   - URL: `https://YOUR-REPLIT-URL.replit.app/api/stripe/webhook`
   - Events to listen: `invoice.payment_succeeded`, `invoice.payment_failed`, `customer.subscription.updated`, `customer.subscription.deleted`
   - Copy the "Signing secret" (whsec_...)
4. Add all three keys to Replit Secrets

# Architecture Decisions

## Multi-User Support via PostgreSQL
- Migrated from localStorage to PostgreSQL for multi-user support
- All user data is isolated via userId foreign keys on all tables
- Cascade deletes ensure data integrity when users are removed

## Three-Tier Access Control
1. **Unauthenticated**: Can only view Landing page
2. **Authenticated but Unpaid**: Redirected to Subscribe page
3. **Authenticated and Paid**: Full access to all application features

## Webhook-Driven Subscription Status
- Stripe webhooks automatically update user.isPaid and user.subscriptionStatus
- Webhook signature verification ensures security
- Supports subscription lifecycle: active, past_due, canceled

## Session-Based Authentication
- Express sessions stored in PostgreSQL for persistence across server restarts
- 7-day session TTL with automatic refresh via OAuth refresh tokens
- Secure cookies with httpOnly and secure flags

## Known Technical Debt
- **SetlistDetailPage localStorage Migration**: `SetlistDetailPage.tsx` still uses localStorage (`storage.getSongsLibrary()`) instead of PostgreSQL API. This creates inconsistency with other modules (BibliotecaMusicas, MusicLibraryDialog) that correctly use the API. Future work should migrate SetlistDetailPage to use API endpoints for songs.
- **Recent Changes (2025-01-05)**:
  * BibliotecaMusicas: Added "Adicionar Várias" button with BulkAddDialog integration for bulk song creation via API
  * MusicLibraryDialog: Migrated from localStorage to API (`/api/songs`), added multi-select deletion with mutation-based API calls
  * All biblioteca-related CRUD now uses PostgreSQL via API with proper user scoping