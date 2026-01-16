# StreamHub - IPTV Streaming Application

## Overview

StreamHub is a full-stack IPTV streaming application that connects to Xtream Codes API servers to provide live TV, movies, and series streaming. The application features a Netflix-style dark UI with category browsing, content search, favorites management, and integrated video playback. Users authenticate via Replit Auth and can configure multiple IPTV server connections.

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### Frontend Architecture
- **Framework**: React 18 with TypeScript
- **Routing**: Wouter (lightweight React router)
- **State Management**: TanStack React Query for server state, local React state for UI
- **Styling**: Tailwind CSS with custom dark theme (Netflix-inspired aesthetic)
- **UI Components**: shadcn/ui component library built on Radix UI primitives
- **Video Playback**: ReactPlayer for handling various streaming formats
- **Build Tool**: Vite with path aliases (@/, @shared/, @assets/)

### Backend Architecture
- **Runtime**: Node.js with Express
- **Language**: TypeScript with ES modules
- **API Pattern**: REST endpoints defined in shared/routes.ts with Zod validation
- **Authentication**: Replit Auth via OpenID Connect with Passport.js
- **Session Management**: PostgreSQL-backed sessions via connect-pg-simple

### Data Storage
- **Database**: PostgreSQL
- **ORM**: Drizzle ORM with drizzle-zod for schema-to-validation integration
- **Schema Location**: shared/schema.ts (tables: iptvConfigs, appSettings, favorites, users, sessions)
- **Migrations**: Drizzle Kit with push-based migrations (`npm run db:push`)

### API Structure
The API follows a typed contract pattern defined in shared/routes.ts:
- `/api/iptv/configs` - CRUD for IPTV server configurations (admin only)
- `/api/iptv/categories/:type` - Fetch categories (live, vod, series) from active Xtream server
- `/api/iptv/streams/:type` - Fetch streams by category from Xtream server
- `/api/favorites` - User favorites management
- `/api/auth/user` - Current authenticated user

### Key Design Patterns
- **Shared Types**: Schema and route definitions in /shared are used by both client and server
- **Xtream Codes Integration**: Backend proxies requests to configured IPTV servers, constructing player_api.php calls
- **Stream URL Construction**: Direct stream URLs built from config credentials (host/username/password/streamId)

## External Dependencies

### Third-Party Services
- **Replit Auth**: OpenID Connect authentication provided by Replit platform
- **Xtream Codes API**: External IPTV provider API for fetching categories, streams, and content metadata

### Database
- **PostgreSQL**: Required via DATABASE_URL environment variable
- **Session Store**: Uses PostgreSQL for persistent session storage

### Environment Variables Required
- `DATABASE_URL` - PostgreSQL connection string
- `SESSION_SECRET` - Secret for session encryption
- `ISSUER_URL` - Replit OIDC issuer (defaults to https://replit.com/oidc)
- `REPL_ID` - Replit environment identifier

### Key NPM Packages
- `drizzle-orm` / `drizzle-kit` - Database ORM and migrations
- `@tanstack/react-query` - Async state management
- `react-player` - Video streaming component
- `passport` / `openid-client` - Authentication
- `express-session` / `connect-pg-simple` - Session management
- `zod` / `drizzle-zod` - Schema validation