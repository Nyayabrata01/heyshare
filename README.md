
# Heyshare 🚀
HeyShare is a login-free web app for instant file, image, and text sharing via secure room links also have real-time syncing across devices using Upstash Redis, with safe uploads , no sign-up, no installation, just seamless sharing.enables temporary sharing sessions where participants can join instantly via a link and collaborate in real time.

## ✨ Features

* 📤 Instant file, image, and text sharing
* 🔗 Secure, shareable room links for temporary sessions
* ⚡ Real‑time message syncing using **Upstash Redis**
* 🔐 Secure file uploads powered by **UploadThing**
* ❌ No login, sign‑up, or installation required
* 🌐 Works across all modern devices and browsers

## 🛠️ Tech Stack

* **Frontend**: Next.js / React
* **Realtime & Storage**: Upstash Redis or Vercel KV
* **File Uploads**: UploadThing
* **Deployment**: Vercel

## ⚙️ Environment Variables

Create a `.env.local` file in the root of the project and add **one** of the following options based on your setup:

### Option 1: Using Upstash Redis directly

```env
UPSTASH_REDIS_REST_URL=https://your-redis-url.upstash.io
UPSTASH_REDIS_REST_TOKEN=your-token-here
```

### Option 2: Using Vercel KV

```env
KV_REST_API_URL=https://your-kv-url.kv.vercel-storage.com
KV_REST_API_TOKEN=your-token-here
```

> ⚠️ Do not expose these keys publicly. Add `.env.local` to your `.gitignore`.

## 🚀 Getting Started

1. Clone the repository

```bash
git clone https://github.com/Nyayabrata01/heyshare.git
cd heydrop
```

2. Install dependencies

```bash
npm install
```

3. Add environment variables (see above)

4. Run the development server

```bash
npm run dev
```

5. Open `http://localhost:3000` in your browser

## 📌 Use Cases

* Quick file transfer between phone and laptop
* Classroom or meeting collaboration
* Temporary peer‑to‑peer sharing
* Privacy‑friendly real‑time communication

## 🔒 Security & Privacy

* No user accounts or stored personal data
* Temporary rooms with controlled access
* Secure upload handling via UploadThing


---

**HeyDrop** — Share instantly. No friction. No sign‑ups.
