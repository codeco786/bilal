# Urdu Calligraphy E-commerce Website

## Overview

This is a full-stack e-commerce web application dedicated to selling Urdu calligraphy art pieces. The application serves as a digital marketplace where customers can browse, purchase, and customize beautiful Urdu calligraphy artwork. The platform combines modern web technologies with cultural heritage, offering both English and Urdu language support to serve a global audience interested in Islamic and Urdu art.

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### Frontend Architecture
- **Framework**: React 18 with TypeScript
- **Routing**: Wouter for lightweight client-side routing
- **Styling**: Tailwind CSS with custom design system
- **UI Components**: Radix UI primitives with shadcn/ui component library
- **State Management**: React Query (TanStack Query) for server state, React Context for client state
- **Build Tool**: Vite for fast development and optimized builds

### Backend Architecture
- **Runtime**: Node.js with Express.js server
- **Language**: TypeScript throughout the stack
- **API Design**: RESTful API with JSON responses
- **Development**: Hot module replacement via Vite middleware integration

### Data Storage Solutions
- **Database**: PostgreSQL with Drizzle ORM (Active - January 16, 2025)
- **Database Provider**: Neon serverless PostgreSQL
- **Schema Management**: Drizzle Kit for migrations and schema management
- **Session Storage**: Database-backed cart sessions with automatic seeding
- **Migration**: Switched from in-memory MemStorage to DatabaseStorage implementation

## Key Components

### Core Entities
1. **Products**: Calligraphy artwork with multilingual support (English/Urdu)
2. **Blog Posts**: Educational content about calligraphy art and culture
3. **Custom Orders**: Personalized calligraphy commission system
4. **Shopping Cart**: Session-based cart management with product variants

### Frontend Components
- **Responsive Design**: Mobile-first approach with desktop optimization
- **Internationalization**: Built-in language switching between English and Urdu
- **Product Catalog**: Filterable grid with categories, frame styles, and sizes
- **Shopping Experience**: Cart management, product customization, and checkout flow
- **Content Management**: Blog system for storytelling and cultural education

### Backend Services
- **Product Management**: CRUD operations for artwork catalog
- **Order Processing**: Custom order quote system with status tracking
- **Content Delivery**: Blog post management with published/draft states
- **Cart Operations**: Session-based shopping cart with quantity management

## Data Flow

### Client-Server Communication
1. **API Layer**: RESTful endpoints under `/api/*` prefix
2. **Data Fetching**: React Query for caching and synchronization
3. **State Updates**: Optimistic updates with server reconciliation
4. **Error Handling**: Centralized error boundary with user-friendly messages

### Database Operations
1. **Schema Definition**: Shared TypeScript schemas between client and server
2. **Query Layer**: Drizzle ORM with type-safe database operations
3. **Data Validation**: Zod schemas for request/response validation
4. **Migration Management**: Automated schema migrations via Drizzle Kit

## External Dependencies

### Core Technologies
- **React Ecosystem**: React, React DOM, React Query for state management
- **Database Stack**: Drizzle ORM, Neon PostgreSQL, connection pooling
- **UI Framework**: Radix UI primitives, Tailwind CSS, Lucide icons
- **Development Tools**: Vite, TypeScript, ESBuild for production builds

### Third-party Services
- **Database Hosting**: Neon serverless PostgreSQL
- **Image Storage**: External image URLs (Unsplash for demo content)
- **Communication**: WhatsApp integration for customer support
- **Payment Processing**: Ready for integration (not implemented in MVP)

## Deployment Strategy

### Build Process
1. **Frontend Build**: Vite compiles React app to static assets
2. **Backend Build**: ESBuild bundles server code to single executable
3. **Database Setup**: Drizzle migrations run automatically on deployment
4. **Asset Optimization**: Tailwind CSS purging and image optimization

### Environment Configuration
- **Development**: Local development with Vite dev server and hot reload
- **Production**: Express server serving built assets with API routes
- **Database**: Environment-based connection string configuration
- **Replit Integration**: Special handling for Replit development environment

### Scalability Considerations
- **Database**: Serverless PostgreSQL scales automatically with usage
- **Frontend**: Static asset delivery optimized for CDN deployment
- **API**: Stateless design allows for horizontal scaling
- **Session Management**: Designed for migration to Redis for production scale

## Recent Changes

### January 16, 2025
- **Database Integration**: Successfully migrated from in-memory storage to PostgreSQL database
- **Data Persistence**: All products, blog posts, custom orders, and cart items now persist in database
- **Automatic Seeding**: Database automatically seeds with sample calligraphy products and blog content
- **Error Fixes**: Resolved SelectItem empty string errors that were preventing shop page functionality
- **Type Safety**: Enhanced null/undefined handling for better TypeScript compliance

The application follows modern web development best practices with a focus on performance, accessibility, and cultural sensitivity to serve the Urdu calligraphy art community effectively.