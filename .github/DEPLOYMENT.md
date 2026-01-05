# Deployment Guide

## Deploying to Vercel

### Prerequisites
1. A Vercel account
2. Supabase project fully set up
3. Stripe account configured
4. Database migrations run

### Steps

1. **Connect GitHub Repository**
   - Push your code to GitHub
   - Connect repository to Vercel
   - Vercel will auto-detect Next.js

2. **Configure Environment Variables**
   All environment variables are already in your project settings:
   - NEXT_PUBLIC_SUPABASE_URL
   - NEXT_PUBLIC_SUPABASE_ANON_KEY
   - SUPABASE_SERVICE_ROLE_KEY
   - STRIPE_SECRET_KEY
   - NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY
   - POSTGRES_URL

3. **Deploy**
   - Click "Deploy" in Vercel
   - Wait for build to complete
   - Your app will be live!

4. **Post-Deployment**
   - Test authentication flow
   - Verify Stripe checkout works
   - Create your first admin user
   - Upload sample content

### Domain Setup
1. Add custom domain in Vercel settings
2. Update DNS records
3. SSL certificate auto-generated
4. Update Supabase redirect URLs

### Stripe Webhook Setup
1. Add your production domain to Stripe webhooks
2. Set webhook endpoint: `https://yourdomain.com/api/stripe/webhook`
3. Select events: `checkout.session.completed`
4. Copy webhook secret to environment variables

## Database Migrations

Run these scripts in your Supabase SQL editor (or via v0):
1. `scripts/001_create_tables.sql`
2. `scripts/002_create_policies.sql`
3. `scripts/003_create_triggers.sql`
4. `scripts/004_seed_data.sql` (optional, for testing)

## Monitoring

### Vercel
- Check deployment logs
- Monitor function execution time
- View analytics dashboard

### Supabase
- Monitor database queries
- Check auth logs
- Review RLS policy hits

### Stripe
- Monitor successful payments
- Check for failed transactions
- Review webhook delivery logs

## Troubleshooting

### Common Issues

1. **Authentication fails**
   - Verify Supabase environment variables
   - Check email confirmation is enabled
   - Review proxy.ts middleware configuration

2. **Payment not working**
   - Test with Stripe test cards
   - Verify webhook is receiving events
   - Check Stripe API keys are correct

3. **Database errors**
   - Ensure all migrations are run
   - Verify RLS policies are set up
   - Check connection string is valid

4. **Build failures**
   - Review build logs in Vercel
   - Check TypeScript errors
   - Verify all dependencies are installed

## Performance Optimization

1. **Images**
   - Use Next.js Image component
   - Optimize video thumbnails
   - Compress PDF files

2. **Caching**
   - Enable Vercel Edge caching
   - Use React Server Components
   - Cache course data

3. **Database**
   - Add indexes for frequently queried fields
   - Use database connection pooling
   - Optimize complex queries

## Security Checklist

- [ ] All environment variables set in Vercel
- [ ] RLS policies enabled on all tables
- [ ] HTTPS enforced
- [ ] Stripe webhook signature verification enabled
- [ ] CORS configured properly
- [ ] Admin routes protected
- [ ] API routes authenticated
- [ ] Email verification required

## Backup Strategy

1. **Database Backups**
   - Supabase automatically backs up daily
   - Enable point-in-time recovery
   - Export data periodically

2. **Code Backups**
   - Code is in GitHub
   - Tag production releases
   - Keep deployment history

3. **Media Backups**
   - Back up uploaded videos/PDFs
   - Store in separate cloud storage
   - Regular backup verification

## Scaling Considerations

As your platform grows:
1. Upgrade Supabase plan for more database connections
2. Enable Vercel Pro for better performance
3. Consider CDN for video delivery
4. Add Redis caching layer
5. Implement rate limiting
6. Set up monitoring alerts
7. Add load balancing for high traffic

---

Your Aman Singh Education Hub is ready for production!
