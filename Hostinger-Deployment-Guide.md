# Hosting Hub - Hostinger Deployment Guide

## Prerequisites

1. **Node.js 20.x** or higher installed on Hostinger
2. **PostgreSQL database** provisioned on Hostinger
3. **Environment variables** configured in Hostinger panel

## Environment Variables Required

Set these in your Hostinger control panel or `.env` file:

```
NODE_ENV=production
PORT=3000
DATABASE_URL=postgresql://username:password@host:5432/hiring_hub
SESSION_SECRET=your-secure-random-string-here
```

Optional (for features):
```
GOOGLE_CLOUD_STORAGE_KEY=
STRIPE_SECRET_KEY=
SENDGRID_API_KEY=
OPENAI_API_KEY=
```

## Deployment Steps

### 1. Prepare for Production Build

```bash
npm install
npm run check  # Type checking
npm run build  # Builds frontend + backend
```

### 2. Upload to Hostinger

**Option A: Using Git (Recommended)**
- Connect your GitHub repo in Hostinger's Git Integration
- Set build command: `npm run build`
- Set start command: `npm start`
- Deploy automatically on push

**Option B: Manual Upload**
1. Run `npm run build` locally
2. Upload entire project folder via FTP/SFTP
3. Run `npm install --production` on server
4. Start with `npm start`

### 3. Database Setup

Run migrations on Hostinger:
```bash
npm run db:push
```

### 4. Verify Deployment

- Check application on: `https://your-hostinger-domain.com`
- Monitor logs in Hostinger control panel
- Test API endpoints

## Key Notes

- **Port**: Application listens on port 3000 (configure reverse proxy in Hostinger)
- **Static Files**: Frontend built to `dist/public/` and served by Express
- **Database**: Requires PostgreSQL with `connect-pg-simple` for session storage
- **Build Output**: Single `dist/index.cjs` file runs the full stack

## Troubleshooting

### Build Fails
- Ensure TypeScript compilation: `npm run check`
- Check Node version: `node --version`

### Database Connection Error
- Verify `DATABASE_URL` format: `postgresql://user:pass@host:port/dbname`
- Ensure PostgreSQL user has table creation permissions

### Static Files Not Loading
- Verify `dist/public/` directory exists after build
- Check reverse proxy headers in Hostinger

## Scaling Tips

- Use Hostinger's managed PostgreSQL
- Enable caching headers for static assets
- Monitor Node.js memory usage
- Consider load balancing for high traffic

## Support

For Hostinger-specific issues:
- Contact Hostinger support
- Check their Node.js deployment docs
- Verify firewall/security rules allow database connections
