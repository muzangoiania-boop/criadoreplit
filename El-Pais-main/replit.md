# Protocolo Gelatina Reductora - Advertorial

## Overview
This is a React-based advertorial/quiz flow application for a weight loss product called "Protocolo Gelatina Reductora". The app features an interactive quiz that collects user information and presents a sales pitch for the product. Includes a comprehensive analytics dashboard.

## Technology Stack
- **Frontend Framework**: React 19.2.0
- **Build Tool**: Vite 6.2.0
- **Language**: TypeScript 5.8.2
- **Styling**: Tailwind CSS (via CDN)
- **Icons**: Lucide React
- **Charts**: Recharts
- **Database**: Supabase (PostgreSQL)
- **Fonts**: Merriweather (serif), Inter (sans-serif)
- **Deployment**: Vercel with Serverless Functions

## Project Structure
```
.
├── api/
│   └── analytics.ts           # Vercel serverless function for analytics
├── components/
│   ├── dashboard/
│   │   └── Dashboard.tsx      # Analytics dashboard component
│   ├── Header.tsx             # Top navigation bar
│   ├── NewsFeed.tsx           # Bottom section with news/social proof
│   └── QuizFlow.tsx           # Main quiz component with multi-step flow
├── lib/
│   ├── analytics.ts           # Analytics tracking service
│   ├── analyticsData.ts       # Dashboard data processing
│   └── supabase.ts            # Supabase client and functions
├── App.tsx                    # Main app with routing
├── index.tsx                  # Entry point
├── index.html                 # HTML template
├── vite.config.ts             # Vite configuration
├── vercel.json                # Vercel deployment configuration
├── tsconfig.json              # TypeScript configuration
├── supabase_setup.sql         # SQL to create quiz_events table in Supabase
└── package.json               # Dependencies
```

## Features
- Interactive multi-step quiz flow
- Testimonials from verified customers
- Dynamic BMI visualization
- Loading animations with progress bar
- Responsive mobile-first design
- Spanish language interface
- **Analytics Dashboard** with:
  - Session tracking (start, complete, abandon)
  - Answer tracking per question
  - Time spent per step
  - Funnel visualization
  - Answer distribution charts
  - Date range filters
  - Password protection

## Analytics Dashboard

### Access
Navigate to `/dashboard` or `/analytics` to access the dashboard.

**Default Password**: `admin123`

### Metrics Available
- Total sessions
- Completion rate
- Abandonment rate by step
- Average completion time
- Answer distribution per question
- Conversion funnel visualization
- Time series data (sessions per day)
- Recent events log

### How It Works
1. **Tracking**: The quiz automatically tracks all user interactions
2. **Storage**: Events are stored in localStorage AND sent to Supabase for persistent storage
3. **Dashboard**: Visualizes all collected data with charts and tables from both local and Supabase sources

### Supabase Configuration
- **Project URL**: https://swnwftcfvlmdviknfdou.supabase.co
- **Table**: `quiz_events`
- **Setup**: Run the SQL in `supabase_setup.sql` in your Supabase SQL Editor to create the required table

## Recent Changes (December 01, 2025)
- ✅ **Reverted Tracking Parameter Implementation**: Removed custom trackingParams.ts to allow GTM to handle parameter propagation natively
  - GTM is responsible for passing xcod, utm_*, sck parameters to checkout URL
  - CTA uses static Hotmart URL; GTM rewrites it with tracking parameters on click
- ✅ **GitHub Integration**: Project successfully pushed to GitHub repository

## Previous Changes (November 30, 2025)
- ✅ Added complete analytics tracking system
- ✅ Created professional dashboard with charts
- ✅ Implemented Vercel serverless API for events
- ✅ Added password protection for dashboard
- ✅ Tracking: quiz start, step views, answers, completions, abandonments
- ✅ Time tracking per step
- ✅ Funnel visualization
- ✅ Answer distribution charts
- ✅ Date range filters (24h, 7d, 30d, 90d, all time)
- ✅ **Supabase Integration**: Analytics data now persists in Supabase PostgreSQL database
- ✅ Dashboard shows data source indicator (Local, Supabase, or Mixed)
- ✅ **VTurb CTA Tracking for Meta Ads**: Fixed InitiateCheckout event tracking
- ✅ **Wistia Video Player**: Migrated from VTurb to Wistia for video hosting

## VTurb/Meta Ads Integration

### How It Works
The application uses VTurb smartplayer to display the sales video. When VTurb's internal CTA button becomes visible (configured in VTurb dashboard), our custom CTA button also appears below the video. When clicked, it sends an `initiate_checkout` event to the dataLayer for Google Tag Manager and Meta Ads.

### Video Player
- **Platform**: VTurb (ConverteAI)
- **Player ID**: vid-692e4d6a3dbab420e9909b10
- **Script**: `scripts.converteai.net/8a115e75-6120-44f3-a213-a7424af5f137/players/692e4d6a3dbab420e9909b10/v4/player.js`

### Implementation Details (QuizFlow.tsx)
1. **VTurb Smartplayer**: Uses VTurb's custom web component `<vturb-smartplayer>`
2. **CTA Sync**: Monitors when VTurb's internal CTA button becomes visible
3. **Custom CTA Button**: Yellow "ACCEDER A MI PROTOCOLO PERSONALIZADO AHORA" link appears below the video
4. **GTM Link Click Detection**: Uses `<a>` tag (not `<button>`) so GTM can detect link clicks to pay.hotmart.com
5. **InitiateCheckout Event**: When link is clicked, pushes event to dataLayer for additional tracking
6. **Duplicate Prevention**: Uses `ctaTrackedRef` to ensure the event only fires once per session

### IMPORTANT: GTM Link Click Detection
The GTM script is configured to detect clicks on links (`<a>` tags) that point to `pay.hotmart.com`. 
The CTA must use an `<a>` element (not `<button>`) with `href="https://pay.hotmart.com/..."` for GTM to:
- Automatically detect the link click event (`gtm.linkClick`)
- Apply UTM parameters and tracking cookies to the checkout URL
- Fire the `begin_checkout` and `InitiateCheckout` tags

### DataLayer Event Format
```javascript
{
  'event': 'initiate_checkout',
  'event_category': 'ecommerce',
  'event_label': 'cta_link_click',
  'cta_source': 'custom_cta_link'
}
```

### GTM Configuration Required
The GTM container automatically detects link clicks to `pay.hotmart.com` and fires:
- Google Analytics `begin_checkout` event
- Meta Pixel `InitiateCheckout` standard event

### Debug Logs
The implementation includes console logs prefixed with `[VTurb Tracker]` and `[CTA Link]` for debugging:
- `[VTurb Tracker] Starting VTurb smartplayer tracking`
- `[VTurb Tracker] VTurb player found`
- `[VTurb Tracker] VTurb CTA triggered - SHOWING CTA BUTTON`
- `[CTA Link] Link clicked - pushing InitiateCheckout to dataLayer`

### VTurb CTA Configuration
The CTA timing is configured directly in the VTurb dashboard. When the VTurb player shows its internal CTA button, the app detects this and shows the custom yellow CTA button below the video.

## Development

### Local Development
```bash
npm install
npm run dev
```
The app will run on http://localhost:5000

### Build for Production
```bash
npm run build
```
Output will be in the `dist/` directory.

### Preview Production Build
```bash
npm run preview
```

## Deployment (GitHub + Vercel)

### Configuration
1. Connect your GitHub repository to Vercel
2. Build command: `npm run build`
3. Output directory: `dist`
4. The `vercel.json` file is already configured for:
   - API routes (`/api/*`)
   - SPA routing

### Environment Variables (Optional)
- `DASHBOARD_PASSWORD`: Custom password for dashboard (default: quiz2024)

## Quiz Flow Steps
1. Introduction page with call-to-action
2. Weight loss goal selection
3. Body classification
4. Target area for fat reduction
5. Name input
6. Appearance satisfaction
7. Weight loss obstacles
8. Impact on daily life
9. Desired benefits
10-11. Customer testimonials (Gomita, Fernanda)
12. Height slider
13. Goal weight slider
14. Current weight slider
15. Water intake selection
16. Loading/analysis screen
17. Results page with BMI visualization
18. Sales/Video page

## Notes
- Analytics data persists in localStorage for development
- In production (Vercel), data is also sent to serverless API
- Dashboard requires password authentication
- Tailwind CSS is loaded via CDN (consider installing as dependency for production optimization)
