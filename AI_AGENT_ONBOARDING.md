# AI Agent Onboarding Guide - Headshots AI Starter

## 📋 Project Overview

**Headshots AI** is a Next.js-based SaaS application that generates professional AI headshots using Astria.ai's fine-tuning capabilities. The project serves as a complete starter template for building AI-powered image generation applications.

### Core Functionality

- Users upload 4+ sample images
- System trains a custom AI model using Astria's API
- Generates professional headshots in multiple styles
- Credit-based billing system with Stripe integration
- Real-time status updates via webhooks

## 🏗️ Architecture Overview

### Tech Stack

- **Frontend**: Next.js 14 (App Router), React 18, TypeScript
- **Styling**: Tailwind CSS, Shadcn/ui components, Radix UI primitives
- **Database**: Supabase (PostgreSQL) with Row Level Security
- **Authentication**: Supabase Auth with magic link login
- **AI Service**: Astria.ai for model training and inference
- **Payments**: Stripe (optional, credit-based system)
- **Email**: Resend (optional notifications)
- **Storage**: Vercel Blob for image uploads
- **Deployment**: Optimized for Vercel

### Key Architecture Patterns

- **Server Components**: Extensive use of React Server Components
- **API Routes**: Next.js 13+ app router with route handlers
- **Real-time Updates**: Client-side components with Supabase realtime
- **Webhook-driven**: Status updates via Astria webhooks
- **Credit System**: Optional Stripe integration for billing

## 📁 Project Structure

```
├── app/                          # Next.js app router
│   ├── astria/                   # Astria API integration routes
│   │   ├── train-model/route.ts  # Model training endpoint
│   │   ├── train-webhook/route.ts# Training status webhooks
│   │   └── prompt-webhook/route.ts# Generation completion webhooks
│   ├── auth/                     # Authentication flows
│   ├── overview/                 # Main dashboard (authenticated)
│   ├── login/                    # Login/auth pages
│   └── stripe/                   # Payment webhooks
├── components/                   # React components
│   ├── ui/                       # Shadcn/ui base components
│   ├── homepage/                 # Landing page sections
│   └── realtime/                 # Real-time data components
├── lib/                          # Utility libraries
│   ├── config.ts                 # Environment configuration
│   ├── utils.ts                  # General utilities
│   └── imageInspection.ts        # Image analysis utilities
├── types/                        # TypeScript definitions
│   ├── supabase.ts              # Auto-generated DB types
│   └── leap.ts                  # Astria API types
└── supabase/                    # Database schema and migrations
```

## 🗄️ Database Schema

### Core Tables

- **models**: User's AI models (training status, metadata)
- **images**: Generated headshot results
- **samples**: Training images uploaded by users
- **credits**: User credit balances (if Stripe enabled)

### Key Relationships

- User → Models (one-to-many)
- Models → Images (one-to-many, generated results)
- Models → Samples (one-to-many, training data)
- User → Credits (one-to-one, billing)

## 🔐 Authentication Flow

1. **Magic Link**: Users enter email, receive magic link
2. **Supabase Auth**: Handles authentication state
3. **Middleware**: Validates sessions on protected routes
4. **RLS Policies**: Row-level security ensures data isolation

## 🎯 Core User Journey

1. **Landing Page**: Marketing site with examples and pricing
2. **Authentication**: Magic link login
3. **Model Training**: Upload 4+ sample images, choose pack/style
4. **Processing**: Real-time status updates via webhooks
5. **Results**: View and download generated headshots
6. **Billing**: Credit-based system (optional)

## 🔌 Key Integrations

### Astria.ai API

- **Training**: `/tunes` or `/p/{pack}/tunes` endpoints
- **Two Modes**: Traditional prompts vs. Pack-based generation
- **Webhooks**: Real-time training and generation status updates
- **Test Mode**: Available for development without charges

### Environment Modes

- **Packs Mode** (`NEXT_PUBLIC_TUNE_TYPE=packs`): Use predefined prompt collections
- **Tune Mode** (`NEXT_PUBLIC_TUNE_TYPE=tune`): Custom prompt definitions

## ⚙️ Environment Configuration

### Required Variables

#### Core Astria integration

ASTRIA_API_KEY=your-api-key
APP_WEBHOOK_SECRET=random-string
DEPLOYMENT_URL=your-domain.com

#### Supabase (auto-configured via Vercel integration)

NEXT_PUBLIC_SUPABASE_URL=your-supabase-url
NEXT_PUBLIC_SUPABASE_ANON_KEY=your-anon-key
SUPABASE_SERVICE_ROLE_KEY=your-service-role-key

# Image storage

BLOB_READ_WRITE_TOKEN=vercel-blob-token

```txt
### Optional Integrations
```bash
# Stripe billing
NEXT_PUBLIC_STRIPE_IS_ENABLED=true
STRIPE_SECRET_KEY=sk_...
STRIPE_PRICE_ID_ONE_CREDIT=price_...

# Email notifications

RESEND_API_KEY=re_...

# Feature flags

ASTRIA_TEST_MODE=true

NEXT_PUBLIC_ANNOUNCEMENT_ENABLED=true
```

## 🛠️ Development Workflow

### Local Development Setup

```bash
npm install
cp .env.local.example .env.local
# Configure environment variables
npm run dev
```

### Key Development Commands

- `npm run dev`: Start development server
- `npm run build`: Production build
- `npm run update-types`: Sync Supabase types

### Database Management

- Use Supabase dashboard for schema changes
- Run `npm run update-types` after DB changes
- Migrations in `/supabase/migrations/`

## 🔍 Code Patterns & Conventions

### Component Patterns

- **Server Components**: Default for data fetching
- **Client Components**: Interactive UI, real-time updates
- **Shadcn/ui**: Consistent design system usage
- **Form Handling**: React Hook Form + Zod validation

### API Route Patterns

- **Authentication**: Check user session in all protected routes
- **Error Handling**: Consistent error responses with status codes
- **Webhook Security**: Validate webhook secrets
- **Database Operations**: Use Supabase client with RLS

### File Organization

- **Colocation**: Components near their usage
- **Separation**: UI components in `/components/ui/`
- **Types**: Centralized in `/types/`
- **Utils**: Shared utilities in `/lib/`

## 🚨 Common Issues & Solutions

### Webhook Issues

- **Preview URLs**: Cannot use Vercel preview URLs for webhooks
- **HTTPS Required**: All webhook URLs must be HTTPS
- **Secret Validation**: Always validate webhook secrets

### Image Upload Issues

- **Size Limits**: Respect Vercel Blob limits
- **Format Support**: JPEG/PNG recommended
- **Face Detection**: Consider implementing face validation

### Database Issues

- **RLS Policies**: Ensure proper row-level security
- **Type Sync**: Regenerate types after schema changes
- **Migration Order**: Apply migrations in correct sequence

## 📝 Key Files for AI Agents

### Critical Files to Understand

1. **`/app/astria/train-model/route.ts`** - Core model training logic
2. **`/types/supabase.ts`** - Database type definitions
3. **`/lib/config.ts`** - Environment configuration validation
4. **`/middleware.ts`** - Authentication middleware
5. **`/app/layout.tsx`** - Root application layout

### Configuration Files

1. **`/next.config.js`** - Next.js configuration
2. **`/tailwind.config.ts`** - Styling configuration
3. **`/.env.local.example`** - Environment template
4. **`/supabase/config.toml`** - Supabase configuration

## 🎛️ Feature Flags & Toggles

- **Stripe Integration**: `NEXT_PUBLIC_STRIPE_IS_ENABLED`
- **AI Mode**: `NEXT_PUBLIC_TUNE_TYPE` (packs vs tune)
- **Test Mode**: `ASTRIA_TEST_MODE`
- **Announcements**: `NEXT_PUBLIC_ANNOUNCEMENT_ENABLED`

## 🔒 Security Considerations

### Authentication

- Magic link authentication via Supabase
- Row-level security policies on all tables
- Webhook secret validation

### Data Protection

- User data isolation via RLS
- Secure environment variable handling
- No client-side API key exposure

### API Security

- Rate limiting (recommended to implement)
- Input validation on all endpoints
- Proper error handling without info leakage

## 🚀 Deployment Notes

### Vercel Deployment

- One-click deployment with template
- Automatic Supabase integration
- Environment variables auto-configured
- Edge function support

### Production Considerations

- Configure proper domain for webhooks
- Set up monitoring for webhook delivery
- Implement proper error tracking
- Configure backup strategies

---

This codebase is production-ready and well-structured for AI agents to understand and extend. The modular architecture, clear separation of concerns, and comprehensive documentation make it an excellent starting point for AI-powered SaaS applications.