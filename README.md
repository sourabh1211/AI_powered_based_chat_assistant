# AI-Powered Knowledge Base Chat Assistant (MERN + OpenAI)

A production-ready **MERN** starter that embeds a **React chat widget** on your website and answers visitor questions using your **MongoDB knowledge base** + **OpenAI**. Includes **conversation context tracking** and an **analytics dashboard** to understand common queries.

---

## ✨ Key Features

- **React Chat Widget (Tailwind CSS)**
  - Floating launcher + chat panel for website visitors
  - Responsive UI styled with **Tailwind CSS**
- **Knowledge Base Indexing (MongoDB + Text Search)**
  - Knowledge base articles stored in MongoDB
  - **Text index** for fast retrieval of relevant articles
- **OpenAI-Powered Answers**
  - Retrieves top-matching articles and sends them as context to OpenAI
  - Produces helpful, grounded answers based on indexed content
- **Conversation Context Tracking**
  - Maintains conversation state via `sessionId`
  - Stores message history in MongoDB (configurable)
- **Query Analytics**
  - Tracks common questions + frequency + last seen time
  - **Analytics dashboard** (React) to visualize query trends

---

## 🧱 Tech Stack

**Frontend**
- React.js (Vite)
- Tailwind CSS
- Charting (simple bar chart component)

**Backend**
- Node.js
- Express.js

**Database**
- MongoDB
- Mongoose
- MongoDB **text indexes** for KB retrieval

**AI**
- OpenAI API (server-side only)

---

## 📸 Screenshots

### 1) Chat Widget
![Chat Widget](Screenshot%202025-12-30%20004717.png)

### 2) Demo Chat Session
![Demo Chat Session](Screenshot%202025-12-30%20005050.png)

### 3) Query Analytics Dashboard
![Analytics Dashboard](Screenshot%202025-12-30%20005122.png)

---

## 📁 Project Structure

```
kb-chat-assistant/
  client/                 # React app (Tailwind CSS)
    src/
      components/
        ChatWidget.jsx
        AnalyticsDashboard.jsx
      pages/
      api/
  server/                 # Express API
    src/
      models/
        Article.js
        Conversation.js
        QueryStat.js
      routes/
        chat.js
        analytics.js
      services/
        retrieval.js
        openai.js
      seed/
        seed.js
  docs/
    analytics-dashboard.png
```

---

## ✅ Deliverables Checklist

- [x] GitHub-ready repo structure + setup instructions (this README)
- [x] Demo knowledge base seeding script + demo chat session flow
- [x] Query analytics tracking + dashboard + screenshot placeholder

---

## 🚀 Getting Started

### 1) Prerequisites
- Node.js **18+**
- MongoDB (local or Atlas)
- OpenAI API key

### 2) Install dependencies (root + client + server)

From the repo root:

```bash
npm install
npm run install:all
```

### 3) Configure environment variables

#### Server env
Copy example and edit:

```bash
cp server/.env.example server/.env
```

Edit `server/.env`:

```env
PORT=8080
MONGO_URI=add-mongo-url
CLIENT_ORIGIN=http://localhost:5173
GEMINI_API_KEY=gemini-api-key
GEMINI_MODEL=gemini-2.5-flash
GEMINI_EMBED_MODEL=add-embed-model
ADMIN_API_KEY=add-admin-key

```

#### Client env
```bash
cp client/.env.example client/.env
```

Edit `client/.env`:

```env
VITE_API_BASE_URL=http://localhost:8080
```

> **Important:** Keep `OPENAI_API_KEY` only on the server. Never expose it in the client.

### 4) Seed the demo knowledge base

```bash
npm run seed
```

This will insert demo articles into MongoDB (FAQ-style) and ensure indexes are created.

### 5) Run the app (client + server)

```bash
npm run dev
```

- React client: `http://localhost:5173`
- Express API: `http://localhost:8080`

---

## 🧠 How It Works

### 1) Retrieve relevant knowledge base articles
When a user asks a question, the backend:
1. Runs a MongoDB **text search** against KB articles.
2. Picks the top matches.
3. Builds a compact context block (title + snippet + link/source).

### 2) Ask OpenAI with grounded context
The retrieved context is injected into the prompt so the model answers using your KB content.

### 3) Persist conversation state
Each chat session is associated with a `sessionId`:
- Stores message history (user + assistant)
- Enables follow-up questions with context

### 4) Track analytics
Every user question is normalized and recorded:
- Count
- First seen time
- Last seen time
- (Optionally) sessionId, page path, etc.

---

## 🔌 API Endpoints

### Chat
- `POST /api/chat`
  - Body:
    ```json
    {
      "sessionId": "optional-client-generated-id",
      "message": "How do I reset my password?"
    }
    ```
  - Response:
    ```json
    {
      "sessionId": "abc123",
      "answer": "..."
    }
    ```

### Analytics
- `GET /api/analytics/top?limit=20`
  - Returns most common questions

---

## 🗃️ MongoDB Indexes

Knowledge base articles use a text index to support search.

Example (implemented in the seed/setup):
- `title` (text)
- `body` (text)
- `tags` (text)

If you add new searchable fields, update the schema/index accordingly in:
- `server/src/models/Article.js`

---

## 🎨 Tailwind CSS Styling

The chat widget UI is styled using **Tailwind CSS** inside the React app.  
To customize the appearance:
- `client/src/components/ChatWidget.jsx`
- `client/src/index.css` (Tailwind directives)

---

## 🧪 Demo Knowledge Base

The seed script inserts demo articles such as:
- Password reset
- Account setup
- Billing / plans
- Widget installation

You can replace demo content by editing:
- `server/src/seed/seed.js`

---

## ⚙️ NPM Scripts

From repo root:

- `npm run dev` – runs **server + client**
- `npm run install:all` – installs deps in root/client/server
- `npm run seed` – seeds demo KB into MongoDB
- `npm run build` – builds the client
- `npm run start` – runs server (production)

---

## 🔒 Security & Production Notes

- Store `OPENAI_API_KEY` in server env only (never in frontend).
- Configure `ALLOWED_ORIGINS` to your domain(s) in production.
- Consider adding:
  - Rate limiting (e.g., `express-rate-limit`)
  - Auth for admin analytics routes
  - Logging + monitoring
  - Caching retrieval results for repeated queries

---

## 🧩 Customization Ideas

- Add **citations** in answers (show which KB articles were used)
- Implement **streaming** responses (SSE/WebSocket) for smoother UX
- Add “**thumbs up/down**” feedback per answer
- Support multiple KB collections or multi-tenant orgs
- Add file upload + auto-ingest into KB

---

## 🛠 Troubleshooting

**MongoDB connection fails**
- Verify `MONGODB_URI` is correct
- Ensure MongoDB is running locally or Atlas IP access is configured

**CORS errors**
- Update `ALLOWED_ORIGINS` in `server/.env`

**OpenAI errors**
- Confirm `OPENAI_API_KEY` is valid
- Verify your model name in `OPENAI_MODEL`

---

## 📄 License

MIT (feel free to change this for your project).
