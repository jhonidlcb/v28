# SoftwarePar - Full-Stack Software Development Platform

## Overview

SoftwarePar is a comprehensive software development platform tailored for the Argentine market, specializing in custom software development services. It offers a complete business solution for managing software projects, from client interaction and development to multi-stage payment processing via MercadoPago and continuous support. Key features include client management, a partner referral program, project management with progress tracking, support ticketing, and WhatsApp notifications. The platform supports three distinct user roles: administrators, partners, and clients. The business vision is to streamline software project delivery and management, leveraging robust financial and communication tools for enhanced efficiency and client satisfaction in the local market.

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### UI/UX Decisions
The frontend leverages React 18 with TypeScript, using shadcn/ui components, Radix UI primitives, and Tailwind CSS for a modern, responsive design. Wouter handles client-side routing, and Framer Motion provides smooth animations. The architecture emphasizes a modular component structure and real-time user feedback via WebSockets.

### Technical Implementations
The frontend uses TanStack Query for server state management. The backend is built with Express.js and TypeScript, adhering to a RESTful API design. It implements JWT-based authentication with role-based access control (RBAC) and bcryptjs for password hashing. API routes are organized by feature domains, incorporating middleware for authentication, authorization, and validation using Zod schemas. WebSockets are integrated for real-time notifications.

### Feature Specifications
- **Project Management**: Includes project creation, status tracking (pending, negotiating, in_progress, completed, cancelled), progress updates (0-100%), and dynamic currency handling (PYG/USD).
- **Payment System**: Multi-stage payment processing integrated with MercadoPago. It supports automatic payment link generation, webhook handling for status updates, commission calculation for partners, and a manual payment verification process for local methods where clients upload proof of payment for admin approval.
- **Communication**: Email (Gmail SMTP), WhatsApp (Twilio API), and real-time WebSocket notifications for critical updates and system alerts.
- **Partner Program**: A referral system with unique referral codes, configurable commission rates (25% default), and tracking of earnings and conversions.
- **Budget Negotiation**: A structured negotiation flow allowing admins and clients to propose, accept, reject, or counter project price proposals.
- **Invoicing**: Automatic generation of legal Paraguayan RESIMPLE invoices (boleta RESIMPLE) for approved payment stages, including unique invoice numbering, fixed exchange rates, and PDF generation with client and company billing details.
- **Electronic Invoicing (SIFEN)**: Comprehensive implementation for Paraguay's SIFEN system (v150 standard). This includes generating SIFEN-compliant XML, digital signing with PFX certificates (RSA-SHA256 XAdES), sending invoices to SIFEN via SOAP Web Services, generating QR codes for public validation, and storing CDC, XML, and authorization protocols. It operates independently of third-party SIFEN services, supporting both test and production environments with or without a PFX certificate.
- **FacturaSend Integration**: Alternative electronic invoicing via FacturaSend.com.py API. Automatically sends invoices to FacturaSend upon payment approval, with dynamic geographic catalog fetching for accurate SIFEN codes. **STATUS: ✅ CONFIGURED, TESTED AND WORKING** - API Key configured in Replit Secrets. Authentication uses `Authorization: Bearer api_key_<API_KEY>` format. Base URL: https://api.facturasend.com.py/jhonifabianbenitezdelacruz. Successfully tested with invoice generation (CDC: 01042200580001001000000112025101410437431888). Correct item format: ivaTipo=1, ivaBase=100, iva=10 for 10% IVA. Date format: ISO without milliseconds. Test endpoints: GET /api/test-facturasend (admin only)

### System Design Choices
PostgreSQL is the primary database, accessed via Drizzle ORM for type-safe operations. The database schema includes tables for users, partners, projects, payment stages, tickets, notifications, and more. Database migrations are managed with Drizzle Kit, and Neon's serverless PostgreSQL driver is used for connections. Authentication relies on JWT tokens stored in localStorage, enforcing RBAC across admin, partner, and client roles.

## External Dependencies

-   **Database**: PostgreSQL (via Neon serverless)
-   **ORM**: Drizzle ORM
-   **Payment Gateway**: MercadoPago
-   **Email Service**: Gmail SMTP
-   **SMS/Messaging API**: Twilio (for WhatsApp)
-   **Frontend Framework**: React
-   **Build Tool**: Vite
-   **UI Library**: shadcn/ui, Radix UI
-   **Styling**: Tailwind CSS
-   **State Management**: TanStack Query
-   **Animations**: Framer Motion
-   **Routing**: Wouter
-   **Backend Framework**: Express.js
-   **Validation**: Zod
-   **Authentication**: JWT, bcryptjs

## Replit Setup

### Environment Configuration
The project is configured to run in the Replit environment with the following setup:

- **Development Server**: Runs on port 5000 (frontend with Vite HMR)
- **Backend API**: Proxied through Vite dev server (configured in vite.config.ts)
- **Database**: PostgreSQL (Neon) - Schema managed with Drizzle ORM
- **Workflow**: `npm run dev` - Starts the development server with TypeScript execution via tsx

### Important Setup Steps
1. **Database Schema**: Run `npm run db:push` (or `npm run db:push --force` for schema updates with data changes) to sync the database schema
2. **Environment Variables**: Required variables are DATABASE_URL (auto-configured), GMAIL_USER, and GMAIL_PASS
3. **Optional Variables**: JWT_SECRET, MercadoPago credentials, Twilio credentials, FacturaSend API key
4. **Vite Configuration**: The dev server is configured with `allowedHosts: true` to work with Replit's proxy system

### Deployment Configuration
- **Target**: Autoscale (stateless web application)
- **Build Command**: `npm run build` (builds both frontend and backend)
- **Run Command**: `npm start` (runs production server)
- **Port**: 5000 (required by Replit)

### Recent Changes (December 2025)
- ✅ Initialized project from GitHub import
- ✅ Installed all npm dependencies
- ✅ Pushed database schema to PostgreSQL
- ✅ Configured workflow for development server
- ✅ Set up deployment configuration for autoscale
- ✅ Created .gitignore for Node.js project