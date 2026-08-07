# DriftCall — Video Storefront

One-tap video calls from your phone to a physical clothing store. Customer scans a QR, enters their phone, taps "Start Video Call" — instantly connected to you behind the counter.

## Stack

- **Netlify** — static hosting + serverless JWT function
- **Supabase** — stores customer phone numbers
- **LiveKit Cloud** (Build tier, free) — video transport

## Quick Start

### 1. Supabase

1. Create a new project at https://supabase.com
2. Run this SQL in the SQL editor:

```sql
create table call_sessions (
  id uuid default gen_random_uuid() primary key,
  phone text not null,
  created_at timestamptz default now()
);
```

3. Get your project URL and anon key from Settings → API

### 2. LiveKit Cloud

1. Sign up at https://cloud.livekit.io (Build tier — free, no card)
2. Create a new project
3. Copy your **API Key**, **API Secret**, and **WebSocket URL**

### 3. Netlify

1. Push this repo to GitHub
2. Connect at https://app.netlify.com → "Add new site" → "Import an existing project"
3. In **Site settings → Environment variables**, add:

```
LIVEKIT_API_KEY=your_livekit_api_key
LIVEKIT_API_SECRET=your_livekit_api_secret
LIVEKIT_URL=wss://your_instance.livekit.cloud
SUPABASE_URL=https://your_project.supabase.co
SUPABASE_ANON_KEY=your_supabase_anon_key
```

4. Deploy.

### 4. Update placeholders

In `index.html` and `owner.html`, replace:
- `YOUR_PROJECT_ID` with your Supabase project ID
- `YOUR_ANON_KEY` with your Supabase anon key
- `YOUR_LIVEKIT_INSTANCE` with your LiveKit instance name

### 5. Generate QR code

```bash
npm install qrcode
node -e "const QR = require('qrcode'); QR.toFileSync('https://your-site.netlify.app/', 'qr.png', { width: 800 }); console.log('QR saved to qr.png');"
```

Print and stick it on your window or counter.

## How it works

```
Customer scans QR → index.html → enters phone → taps "Start"
  → POST to Supabase (save phone)
  → POST to Netlify function (get LiveKit JWT)
  → connect to LiveKit room "storefront"
  → camera + mic enabled automatically

Owner opens owner.html → taps "Go Live" → connects to same room
  → stays connected all day
  → customers appear instantly
```

## Pages

- **/** — customer page (phone input + start button + video)
- **/owner** — owner page (go-live button + stays connected)

## Local dev

```bash
npm install
npm run dev        # starts Netlify Dev on localhost:8888
```

Set env vars in a `.env` file:

```
LIVEKIT_API_KEY=xxx
LIVEKIT_API_SECRET=xxx
LIVEKIT_URL=wss://xxx.livekit.cloud
SUPABASE_URL=https://xxx.supabase.co
SUPABASE_ANON_KEY=xxx
```

## License

MIT
