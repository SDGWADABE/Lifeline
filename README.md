
# Kano Lab & Radiology Center - Single Web App - Vercel + Supabase

Single center KANO-01 | No Branch | No 2FA | Billing in Reception | Payment enforced before lab/scan | Clean homepage 0 seed | Duplicate X-printer receipt + A4 reports in reception

## Features Implemented
- Clean homepage - starts 0, no seed, empty state welcome
- Payment enforced: AWAITING_PAYMENT blocks Samples, Results, Radiology scheduling with PAYMENT REQUIRED modal, red banner pending, green paid
- Duplicate receipt X-printer 80mm: After payment, Customer Copy + Center Copy preview, print duplicate thermal 80mm, downloadable
- A4 Lab & Radiology reports in Reception Print Center: Lab report with reference ranges, flags, signatures, QR /verify, A4 format; Radiology A4 with findings/impression
- Single center Kano only, users linked timeline, workflow guide, real-time sync BroadcastChannel + Supabase Realtime
- Vercel single app + Supabase backend

## Deploy to Vercel in 2 minutes

1. Create Supabase project at https://supabase.com
2. Go to SQL Editor > Paste `supabase/schema.sql` > Run
3. Go to Project Settings > API > Copy URL and anon key
4. Push this folder to GitHub
5. Go to https://vercel.com > New Project > Import GitHub repo
6. Add Environment Variables:
   - VITE_SUPABASE_URL = https://xxx.supabase.co
   - VITE_SUPABASE_ANON_KEY = eyJxxx
7. Deploy - Done! Live at https://your-app.vercel.app

### Without Supabase (Local Mode)
- Just deploy to Vercel without env vars - app works in localStorage mode
- Data saved in browser localStorage, real-time sync via BroadcastChannel

### Vercel Drop (Simplest - No Git)
1. Run `npm run build` locally
2. Drag `dist` folder to https://vercel.com/new > Browse > Deploy

## Logins (mock, same for all)
- Reception: reception@labrad.ng / Recep@123 (can print A4 in reception)
- Finance: finance@labrad.ng / Fin@123 (can confirm payment + print receipt)
- Lab Scientist: scientist@labrad.ng / Lab@123
- Super Admin: super.admin@labrad.ng / Super@123

## Workflow
Register Patient (Reception) -> Create Order (AWAITING_PAYMENT) -> Pay in Billing & Payments -> PAID -> Receipt duplicate X-printer auto preview + X-Printer Duplicate button -> Lab collection unlocked -> Results entry -> Reception Print Center -> Print A4 Reception

## Printing
- X-Printer 80mm: `printThermal(receipt)` opens new window with 80mm HTML, 2 copies Customer + Center, cut line, then window.print()
- A4: `printA4(result)` opens new window with A4 210mm HTML, header, barcode, QR, signatures, then window.print()

## Supabase Tables
patients, orders, samples, results, radiology_orders, receipts, reports_a4 - all with RLS Allow all for anon demo (change to authenticated in production)

## Payment Gate Logic (strict)
```js
const canLab = (order) => order.paymentStatus === 'PAID'
if (!canLab(order)) alert('PAYMENT REQUIRED - Cannot collect sample before payment. Go to Billing')
```
Same for results, radiology scheduling, A4 printing.

Enjoy!
