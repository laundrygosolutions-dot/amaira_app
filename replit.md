# 123laundry - Laundry & Dry Cleaning Service

## Overview

This is a marketing website and booking platform for 123laundry, India's largest laundry and dry cleaning chain with 1400+ stores. The application allows customers to learn about services, view pricing, find store locations, and schedule pickup requests for laundry and dry cleaning services. Slogan: "clean. Quick. Reliable"

The project follows a full-stack TypeScript architecture with React frontend and Express backend, using Drizzle ORM for database operations.

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### Frontend Architecture
- **Framework**: React 18 with TypeScript
- **Routing**: Wouter (lightweight React router)
- **State Management**: TanStack React Query for server state
- **UI Components**: shadcn/ui component library built on Radix UI primitives
- **Styling**: Tailwind CSS with CSS variables for theming
- **Build Tool**: Vite with HMR support

The frontend is a single-page application with a landing page structure containing multiple sections: Hero, Features, Services, Delivery, Process, Stats, Pricing, Testimonials, and Locations. A modal handles service scheduling requests.

### Backend Architecture
- **Framework**: Express 5 running on Node.js
- **API Design**: RESTful JSON API with `/api` prefix
- **Validation**: Zod schemas for request validation (via drizzle-zod)
- **Development**: tsx for TypeScript execution, Vite middleware for HMR

The server handles schedule request submissions and serves the static frontend in production. Routes are registered in `server/routes.ts` with storage abstraction in `server/storage.ts`.

### Data Storage
- **ORM**: Drizzle ORM with PostgreSQL dialect
- **Schema Location**: `shared/schema.ts` (shared between frontend and backend)
- **Current Tables**:
  - `users`: Basic user authentication (id, username, password)
  - `schedule_requests`: Service booking requests (name, phone, email, service, date, timeSlot, address, status, createdAt)
- **Development Storage**: In-memory storage class (`MemStorage`) used as fallback
- **Database Migrations**: Managed via `drizzle-kit push`

### Shared Code
The `shared/` directory contains schema definitions and types used by both frontend and backend, ensuring type safety across the stack. Zod schemas generated from Drizzle tables are used for validation.

### Build Process
- **Development**: `npm run dev` runs tsx with Vite middleware
- **Production Build**: Custom build script (`script/build.ts`) that:
  - Builds frontend with Vite to `dist/public`
  - Bundles server with esbuild to `dist/index.cjs`
  - Bundles specific dependencies to reduce cold start times
- **Production Start**: `npm start` runs the bundled server

## External Dependencies

### Database
- **PostgreSQL**: Required for production (configured via `DATABASE_URL` environment variable)
- **connect-pg-simple**: Session storage for PostgreSQL

### UI Libraries
- **Radix UI**: Full suite of accessible UI primitives (dialog, dropdown, navigation, etc.)
- **Lucide React**: Icon library
- **react-icons**: Additional icons (WhatsApp, social media)
- **embla-carousel-react**: Carousel functionality
- **react-day-picker**: Date picker component
- **vaul**: Drawer component
- **cmdk**: Command palette component

### Form & Validation
- **react-hook-form**: Form state management
- **@hookform/resolvers**: Zod resolver for form validation
- **zod**: Schema validation
- **zod-validation-error**: Human-readable validation errors

### Styling
- **Tailwind CSS**: Utility-first CSS framework
- **class-variance-authority**: Component variant management
- **tailwind-merge**: Class merging utility

### Development Tools
- **Replit Plugins**: Runtime error overlay, cartographer, dev banner (development only)