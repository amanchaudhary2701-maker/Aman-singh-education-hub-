# Aman Singh Education Hub Platform

A comprehensive online education platform built with Next.js 16, Supabase, and Stripe. This platform provides a complete learning management system with courses, live classes, video lectures, PDF resources, tests, and payment processing.

## Features

### Student Features
- User authentication (email/password with Supabase Auth)
- Browse and enroll in courses
- Personalized student dashboard with progress tracking
- Video library with search and filtering
- Live classes (upcoming, live, and recorded sessions)
- Downloadable PDF study materials
- Interactive tests and quizzes with timer
- Test results and performance tracking
- Stripe payment integration for course purchases

### Admin Features
- Comprehensive admin dashboard with statistics
- User management (view all users, roles, and status)
- Course management (create, edit, publish courses)
- Video library management
- Live classes scheduling and management
- PDF resources management
- Tests and exams creation
- Role-based access control (admin, teacher, student)

### Technical Features
- Next.js 16 App Router with React Server Components
- Supabase for authentication and PostgreSQL database
- Row Level Security (RLS) for data protection
- Stripe embedded checkout for payments
- Real-time database updates
- Responsive design with Tailwind CSS
- TypeScript for type safety
- Middleware for protected routes

## Tech Stack

- **Framework**: Next.js 16
- **Language**: TypeScript
- **Database**: Supabase (PostgreSQL)
- **Authentication**: Supabase Auth
- **Payments**: Stripe
- **Styling**: Tailwind CSS v4
- **UI Components**: Radix UI + shadcn/ui
- **Icons**: Lucide React

## Database Schema

### Tables
- `profiles` - User profiles with roles (student, teacher, admin)
- `courses` - Course catalog with pricing and metadata
- `videos` - Video lectures library
- `pdfs` - PDF study materials
- `tests` - Tests and exams with questions
- `live_classes` - Scheduled and recorded live sessions
- `enrollments` - Course enrollments with progress tracking
- `test_results` - Student test scores and performance
- `announcements` - Course announcements

### Security
All tables are protected with Row Level Security (RLS) policies:
- Students can only access their own data
- Teachers can manage content for their courses
- Admins have full access to all resources

## Getting Started

### Prerequisites
- Node.js 18+ installed
- A Supabase account and project
- A Stripe account for payment processing

### Installation

1. Clone the repository
2. Install dependencies (already configured in package.json)
3. Set up environment variables (already configured in your project)
4. Run the database migration scripts in the `scripts/` folder
5. Start the development server

### Database Setup

Execute the SQL scripts in order:
1. `scripts/001_create_tables.sql` - Creates all database tables
2. `scripts/002_create_policies.sql` - Sets up Row Level Security policies
3. `scripts/003_create_triggers.sql` - Creates triggers for auto-profile creation
4. `scripts/004_seed_data.sql` - Adds sample data for testing

### First Admin User

To create your first admin user:
1. Sign up through the normal signup flow
2. After email verification, update your role in the database:
   ```sql
   UPDATE profiles SET role = 'admin' WHERE email = 'your-email@example.com';
   ```

## Project Structure

```
app/
├── (auth)/
│   ├── login/          - Login page
│   ├── signup/         - Registration page
│   └── signup-success/ - Email verification message
├── admin/              - Admin panel
│   ├── users/          - User management
│   ├── courses/        - Course management
│   ├── videos/         - Video management
│   ├── live-classes/   - Live classes management
│   ├── pdfs/           - PDF resources management
│   └── tests/          - Tests management
├── courses/            - Course pages
│   ├── [id]/           - Course detail page
│   └── [id]/checkout/  - Stripe checkout page
├── dashboard/          - Student dashboard
├── videos/             - Video library
│   └── [id]/           - Video player page
├── live-classes/       - Live classes schedule
├── pdfs/               - PDF resources library
├── tests/              - Tests listing
│   └── [id]/           - Test taking interface
└── api/
    ├── enroll/         - Course enrollment endpoint
    └── stripe/         - Stripe webhook handler

components/
├── ui/                 - shadcn/ui components
└── checkout.tsx        - Stripe checkout component

lib/
├── supabase/
│   ├── client.ts       - Client-side Supabase client
│   ├── server.ts       - Server-side Supabase client
│   └── proxy.ts        - Middleware helper
├── products.ts         - Stripe product definitions
├── stripe.ts           - Stripe configuration
└── types.ts            - TypeScript type definitions

scripts/
├── 001_create_tables.sql      - Database schema
├── 002_create_policies.sql    - RLS policies
├── 003_create_triggers.sql    - Database triggers
└── 004_seed_data.sql          - Sample data
```

## Key Features Implementation

### Authentication Flow
1. User signs up with email/password
2. Supabase sends verification email
3. Upon verification, a profile is auto-created via database trigger
4. User can log in and access the platform
5. Middleware protects authenticated routes

### Course Enrollment Flow
1. Student browses courses
2. Clicks "Enroll Now" on a paid course
3. Redirected to Stripe embedded checkout
4. After successful payment, enrollment is created
5. Student can access course content from dashboard

### Test Taking Flow
1. Student selects a test from the tests page
2. Timer starts (if configured)
3. Student answers multiple-choice questions
4. Submits test (or auto-submits when time expires)
5. Results are calculated and saved to database
6. Student sees score and pass/fail status

### Admin Content Management
1. Admin logs in and accesses admin panel
2. Can create courses, upload videos, schedule classes
3. Can manage users and change roles
4. All content is immediately available to students
5. RLS ensures only authorized users can modify content

## Environment Variables

All required environment variables are already configured in your project:
- `NEXT_PUBLIC_SUPABASE_URL` - Supabase project URL
- `NEXT_PUBLIC_SUPABASE_ANON_KEY` - Supabase anonymous key
- `SUPABASE_SERVICE_ROLE_KEY` - Supabase service role key (server-side)
- `STRIPE_SECRET_KEY` - Stripe secret key
- `NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY` - Stripe publishable key
- `POSTGRES_URL` - Database connection string

## Color Scheme

The platform uses a professional education-focused color palette:
- Primary: Blue (#3a86ff) - Trust and learning
- Secondary: Purple (#8338ec) - Creativity and knowledge
- Accent: Pink (#ff006e) - Energy and engagement
- Success: Green (#38b000) - Achievement
- Warning: Yellow (#ffbe0b) - Attention
- Danger: Red (#e63946) - Critical actions

## User Roles

### Student
- Browse and purchase courses
- Watch videos and download PDFs
- Take tests and track progress
- Join live classes
- View personal dashboard

### Teacher
- All student permissions
- Create and manage courses
- Upload videos and PDFs
- Schedule live classes
- Create tests
- Access admin panel (limited)

### Admin
- All teacher permissions
- Manage all users
- Access full admin panel
- View platform statistics
- Manage all content

## Payment Integration

The platform uses Stripe embedded checkout for secure payments:
1. Course prices are defined in `lib/products.ts`
2. Checkout sessions are created server-side in `app/actions/stripe.ts`
3. Stripe handles all payment processing
4. Webhook at `/api/stripe/webhook` handles post-payment events

## Security Best Practices

1. All database queries use RLS for authorization
2. Server components fetch data securely
3. API routes verify user authentication
4. Passwords are hashed by Supabase Auth
5. Environment variables protect sensitive keys
6. HTTPS enforced in production
7. Stripe handles PCI compliance

## Development Tips

1. Always read files before editing to understand context
2. Use the database seed data for testing
3. Create a test admin account for full access
4. Test payment flow in Stripe test mode
5. Monitor Supabase logs for debugging
6. Use browser dev tools to debug client components

## Next Steps

### Recommended Enhancements
1. Add OAuth providers (Google, GitHub)
2. Implement real-time video streaming
3. Add discussion forums for courses
4. Create mobile app with React Native
5. Add certificate generation on course completion
6. Implement email notifications
7. Add analytics dashboard with charts
8. Create bulk upload for videos/PDFs
9. Add course reviews and ratings
10. Implement gamification (badges, points)

## Support

For issues or questions:
1. Check the database setup scripts
2. Verify environment variables are set
3. Review Supabase logs for errors
4. Check browser console for client-side issues
5. Test with sample data from seed script

## License

This project is part of the Aman Singh Education Hub platform.

---

Built with Next.js 16, Supabase, and Stripe by v0.
