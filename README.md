# Buddha & Ambedkar Global News Portal

An automated, modern, responsive full-stack global news portal built with **React.js** and **ASP.NET Core Web API (.NET 9 / EF Core / Microsoft SQL Server)**.

The platform is solely dedicated to collecting, organizing, and delivering authentic global news related to:
1. **Gautam Buddha** (teachings, archaeological discoveries, sacred sites like Bodh Gaya, Lumbini, Sarnath, Kushinagar).
2. **Buddhism & Buddhist Culture** (monastic traditions, peace conferences, Zen/Theravada/Mahayana philosophy across India, Nepal, Sri Lanka, Thailand, Japan, China, Myanmar, and the West).
3. **Dr. B. R. Ambedkar** (legacy, constitutional jurisprudence, human rights, Ambedkar Jayanti, Mahaparinirvan Diwas).
4. **Ambedkar Movement Worldwide** (global organizations like AIC, AANA, FABO UK, AKSC, student scholarships, lectures).
5. **Buddhist Conversion & Dhamma** (Deekshabhoomi, 22 vows, Navayana Dhamma).
6. **Social Justice, Equality & Anti-Caste Morality** (equality, constitutional rights, emancipation).

> **Strict Editorial Policy:** General politics, cricket scores, Bollywood/Hollywood celebrity gossip, and sensationalist commercial news are strictly filtered out by the automated ingestion pipeline.

---

## 🌟 Key Features

### 1. Automated Global News Ingestion Pipeline
- **Background Service (`NewsBackgroundService`)**: Runs continuously in ASP.NET Core, automatically querying global RSS feeds and news feeds every 15 minutes.
- **Strict Whitelist & Blacklist Filtering**: Matches multi-language keywords (English, हिन्दी, मराठी) while aggressively eliminating entertainment and unrelated sports noise.
- **Cryptographic Anti-Duplication Engine**: Computes SHA-256 hashes of normalized URLs and titles to prevent duplicate articles from being saved to SQL Server.
- **Automatic Categorization & Geotagging**: Classifies news into the 5 core categories and detects country origins (India, Nepal, Sri Lanka, Thailand, Japan, China, Myanmar, USA, UK, Global).
- **Legitimate & Ethical Fair Use**: Aggregates titles, short contextual summaries, and prominently directs users to the original publisher with direct attribution links.

### 2. Modern, Responsive Portal UI
- **Color Palette**: Royal Blue (`#1D4ED8`), Navy (`#0B192C`), Pure White (`#FFFFFF`), with Golden accents (`#F59E0B` / `#D97706`).
- **Dark Mode & Light Mode**: Smooth transition with persistent `localStorage` preference.
- **Breaking News Ticker**: Animated marquee displaying real-time breaking headlines with live ingestion indicators.
- **Hero Featured Slider**: Dynamic auto-advancing carousel showcasing high-impact stories.
- **Trending Stories Widget**: Top 5 ranking stories based on reader interest.
- **Multi-lingual Support**: English, Hindi (हिन्दी), and Marathi (मराठी) article feeds.
- **Social Sharing Modal**: Instant 1-click sharing to WhatsApp, X (Twitter), Facebook, LinkedIn, and Clipboard copy.

### 3. Website Pages
1. **Home Page (`/`)**: Breaking ticker, hero slider, trending list, latest global news grid, category tabs, and dual Buddha & Ambedkar spotlights.
2. **Gautam Buddha News (`/buddha`)**: Buddha heritage, Buddhist archaeology, Bodh Gaya & Lumbini reports.
3. **Dr. Babasaheb Ambedkar News (`/ambedkar`)**: Ambedkarite news, Jayanti & Mahaparinirvan events, constitutional justice.
4. **Buddhism Worldwide (`/buddhism-worldwide`)**: Interactive country filter pills (India, Nepal, Sri Lanka, Thailand, Japan, China, Myanmar, USA, UK).
5. **Ambedkar Movement Worldwide (`/ambedkar-movement`)**: Global diaspora organizations, community campaigns, and human rights advocacy.
6. **Search Page (`/search`)**: Multi-faceted search by keyword, country, category, and language.
7. **About Us (`/about`)**: Objective, 6 core pillars, and strict editorial copyright charter.
8. **Contact Us (`/contact`)**: Public reader form with backend API persistence (`POST /api/contact`).
9. **Article Details View (`/news/:id/:slug`)**: SEO-friendly slug URL, full summary, publisher citation, Google NewsArticle JSON-LD structured data.

### 4. Secure Admin Dashboard (`/admin`)
- **JWT Authentication**: Secure admin login with hashed credentials and token validation.
- **Live Statistics Overview**: Total articles, articles ingested today, featured counts, pending reviews, active feeds, and category/country distribution charts.
- **⚡ "Fetch News Now" Button**: On-demand manual trigger to poll all RSS sources instantly.
- **News Article Moderation**: Add news manually, edit summaries, delete articles, toggle Approved / Featured / Trending status.
- **Category & Source Management**: Create and configure custom RSS feeds.
- **Reader Feedback Queue**: Review incoming contact messages and news tips.

---

## 🏗️ Architecture & Technology Stack

```
CMS/
├── backend/                                   # ASP.NET Core Web API (.NET 9)
│   ├── Controllers/
│   │   ├── NewsController.cs                  # REST endpoints for news queries & moderation
│   │   ├── CategoriesController.cs            # Categories CRUD
│   │   ├── SourcesController.cs               # RSS sources & manual fetch trigger
│   │   ├── AdminController.cs                 # JWT Login, statistics, user management
│   │   ├── ContactController.cs               # Public inquiry submission & admin inbox
│   │   └── SeoController.cs                   # Dynamic sitemap.xml & rss.xml generation
│   ├── Data/
│   │   ├── AppDbContext.cs                    # EF Core database context & index mappings
│   │   ├── DbSeeder.cs                        # Automatic database creation & seed data
│   │   └── schema.sql                         # Complete Microsoft SQL Server DDL script
│   ├── DTOs/
│   │   ├── NewsDtos.cs                        # Request/Response models for news
│   │   ├── AdminDtos.cs                       # Login, stats, category, source DTOs
│   │   └── ContactDto.cs                      # Contact submission schema
│   ├── Middleware/
│   │   └── AdminAuthorizeAttribute.cs         # Custom JWT token authorization filter
│   ├── Models/
│   │   ├── NewsArticle.cs                     # Article entity with SHA256 UrlHash
│   │   ├── Category.cs                        # Category schema
│   │   ├── NewsSource.cs                      # RSS source definition & sync metrics
│   │   ├── AdminUser.cs                       # Hashed admin credentials & salt
│   │   └── ContactSubmission.cs               # Reader messages
│   ├── Services/
│   │   ├── NewsAggregatorService.cs           # RSS parser, topic whitelist, anti-duplication
│   │   ├── NewsBackgroundService.cs           # Periodic background worker (15 min interval)
│   │   ├── PasswordHasher.cs                  # PBKDF2-SHA256 password hashing (100k rounds)
│   │   └── TokenService.cs                    # RFC 7519 HMAC-SHA256 JWT token engine
│   ├── wwwroot/
│   │   └── index.html                         # Unified Standalone React SPA Frontend
│   ├── appsettings.json                       # DB connection strings, JWT secret, timer
│   └── Program.cs                             # Dependency Injection, CORS, pipeline & routing
│
└── frontend/                                  # Modular React + Vite Development Workspace
    ├── src/
    │   ├── components/
    │   │   ├── Navbar.jsx                     # Top bar, Dhamma emblem logo, navigation
    │   │   ├── Footer.jsx                     # Mission statement, country links, copyright
    │   │   ├── NewsCard.jsx                   # Card with image, category badge, share button
    │   │   ├── FeaturedSlider.jsx             # Auto-advancing carousel with slide dots
    │   │   ├── BreakingTicker.jsx             # Real-time marquee news ticker
    │   │   └── ShareModal.jsx                 # WhatsApp, X, Facebook, LinkedIn sharing
    │   ├── pages/
    │   │   ├── Home.jsx                       # Full portal home page layout
    │   │   ├── BuddhaNews.jsx                 # Buddha & Buddhist heritage news
    │   │   ├── AmbedkarNews.jsx               # Dr. B. R. Ambedkar news
    │   │   ├── BuddhismWorldwide.jsx          # Country-filtered Buddhist feeds
    │   │   ├── AmbedkarMovement.jsx           # Global Ambedkarite coalition updates
    │   │   ├── SearchPage.jsx                 # Multi-filter search engine
    │   │   ├── AboutUs.jsx                    # Editorial policy & 6 pillars
    │   │   ├── ContactUs.jsx                  # Public inquiry submission form
    │   │   ├── ArticleDetail.jsx              # News detail view with JSON-LD SEO schema
    │   │   ├── AdminDashboard.jsx             # Admin moderation panel & RSS runner
    │   │   └── AdminLogin.jsx                 # JWT admin login screen
    │   ├── services/
    │   │   └── api.js                         # Axios client with JWT interceptor
    │   ├── styles/
    │   │   └── index.css                      # Tailwind styling & dark mode definitions
    │   ├── App.jsx                            # React Router configuration
    │   └── main.jsx                           # React DOM mount point
    ├── index.html
    ├── tailwind.config.js
    ├── vite.config.js
    └── package.json
```

---

## 🗄️ Database Structure (Microsoft SQL Server)

The database schema is defined in `backend/Data/schema.sql` and mapped via Entity Framework Core:

1. **`Categories`**:
   - `Id` (INT, Primary Key)
   - `Name` (NVARCHAR(100))
   - `Slug` (NVARCHAR(120), Unique Index)
   - `Description` (NVARCHAR(500))
   - `DisplayOrder` (INT)
   - `IsActive` (BIT)

2. **`Sources` (RSS Feeds)**:
   - `Id` (INT, Primary Key)
   - `Name` (NVARCHAR(150))
   - `RssUrl` (NVARCHAR(1000))
   - `DefaultCountry` (NVARCHAR(100))
   - `DefaultLanguage` (NVARCHAR(20))
   - `IsActive` (BIT)
   - `LastFetchedAt` (DATETIME2)
   - `ArticlesFetchedCount` (INT)
   - `LastError` (NVARCHAR(500))

3. **`Articles`**:
   - `Id` (INT, Primary Key)
   - `Title` (NVARCHAR(500))
   - `Slug` (NVARCHAR(550))
   - `Summary` (NVARCHAR(2000))
   - `Content` (NVARCHAR(MAX))
   - `OriginalUrl` (NVARCHAR(1000))
   - `Source` (NVARCHAR(150))
   - `CategoryId` (INT, Foreign Key to Categories)
   - `Country` (NVARCHAR(100))
   - `Language` (NVARCHAR(20))
   - `ImageUrl` (NVARCHAR(1000))
   - `PublishedAt` (DATETIME2, Indexed)
   - `IsFeatured` (BIT)
   - `IsTrending` (BIT)
   - `IsApproved` (BIT)
   - `IsManual` (BIT)
   - `ViewCount` (INT)
   - `UrlHash` (NVARCHAR(128), Unique Index to prevent duplicates)
   - `MatchedKeywords` (NVARCHAR(500))

4. **`Users` (Admin)**:
   - `Id` (INT, Primary Key)
   - `Username` (NVARCHAR(80), Unique Index)
   - `Email` (NVARCHAR(150), Unique Index)
   - `PasswordHash` (NVARCHAR(255))
   - `Salt` (NVARCHAR(128))
   - `Role` (NVARCHAR(50))

5. **`ContactSubmissions`**:
   - `Id` (INT, Primary Key)
   - `Name` (NVARCHAR(100))
   - `Email` (NVARCHAR(150))
   - `Subject` (NVARCHAR(200))
   - `Message` (NVARCHAR(3000))
   - `CreatedAt` (DATETIME2)
   - `IsRead` (BIT)

---

## 🚀 Step-by-Step Setup & Installation

### Prerequisites
- [.NET 8 or .NET 9 SDK](https://dotnet.microsoft.com/download)
- [Microsoft SQL Server](https://www.microsoft.com/sql-server/) or SQL Server Express / LocalDB
- [Node.js](https://nodejs.org/) (v18 or higher, required only for frontend development)

---

### Step 1: Clone the Repository
```powershell
cd c:\Users\Lenovo\OneDrive\Desktop\CMS
```

---

### Step 2: Configure SQL Server Database
Open `backend/appsettings.json` and adjust the connection string to match your SQL Server instance:

```json
"ConnectionStrings": {
  "DefaultConnection": "Server=localhost;Database=BuddhaAmbedkarNewsDb;Trusted_Connection=True;MultipleActiveResultSets=true;TrustServerCertificate=True;"
}
```

*Note:* If using SQL Server LocalDB, the default fallback `Server=(localdb)\mssqllocaldb;Database=BuddhaAmbedkarNewsDb;...` will be used automatically. You can also run the full SQL script `backend/Data/schema.sql` inside SQL Server Management Studio (SSMS).

---

### Step 3: Run the Full-Stack Application
The backend server directly serves both the REST API and the React SPA on **`http://localhost:5000`**:

```powershell
cd backend
dotnet run
```

When the application starts:
1. It automatically ensures the database tables are created.
2. It seeds the 5 core categories, active RSS sources, and initial high-quality news articles.
3. It seeds the default administrator account.
4. It launches the background news-fetching worker.

Open your browser at:
👉 **`http://localhost:5000`**

To explore and test all APIs interactively via Swagger:
👉 **`http://localhost:5000/swagger`**

---

### Step 4: Admin Dashboard Login
Navigate to:
👉 **`http://localhost:5000/#admin`**

Default Administrator Credentials:
- **Username:** `admin`
- **Password:** `Admin@BuddhaAmbedkar2026`

*(A one-click "Pre-fill Demo Credentials" button is provided on the login screen for convenience).*

---

### Step 5: (Optional) Running with Vite Dev Server
If you wish to develop the React frontend with Vite hot-module replacement:

```powershell
cd frontend
npm install
npm run dev
```

The frontend will run on `http://localhost:5173` and automatically proxy all `/api`, `/rss.xml`, and `/sitemap.xml` requests to the ASP.NET Core backend on `http://localhost:5000`.

To build the frontend for production:
```powershell
cd frontend
npm run build
```
This automatically outputs the optimized bundle into `backend/wwwroot/`.

---

## 📡 REST API Reference

| HTTP Method | Endpoint | Description | Auth |
|---|---|---|---|
| `GET` | `/api/news` | Paginated news list with filters (category, country, language, search) | Public |
| `GET` | `/api/news/latest` | Latest 10 global articles | Public |
| `GET` | `/api/news/featured` | Top featured carousel articles | Public |
| `GET` | `/api/news/trending` | Top 5 trending articles | Public |
| `GET` | `/api/news/buddha` | Gautam Buddha & Buddhist heritage news | Public |
| `GET` | `/api/news/ambedkar` | Dr. B. R. Ambedkar news & legacy | Public |
| `GET` | `/api/news/buddhism-worldwide` | Country-filtered global Buddhist news | Public |
| `GET` | `/api/news/ambedkar-movement` | Global Ambedkarite movement updates | Public |
| `GET` | `/api/news/search` | Multi-criteria search query endpoint | Public |
| `GET` | `/api/news/country/{country}` | Filter articles by specific country | Public |
| `GET` | `/api/news/{id}` | Single article details (increments view count) | Public |
| `POST` | `/api/news` | Manually publish a new news article | Admin JWT |
| `PUT` | `/api/news/{id}` | Update existing news article | Admin JWT |
| `DELETE` | `/api/news/{id}` | Delete news article | Admin JWT |
| `PUT` | `/api/news/{id}/approve` | Toggle moderation approval | Admin JWT |
| `PUT` | `/api/news/{id}/toggle-feature` | Toggle featured carousel status | Admin JWT |
| `PUT` | `/api/news/{id}/toggle-trending` | Toggle trending status | Admin JWT |
| `GET` | `/api/categories` | Retrieve all categories with article counts | Public |
| `POST` | `/api/categories` | Create new category | Admin JWT |
| `DELETE` | `/api/categories/{id}` | Delete category | Admin JWT |
| `GET` | `/api/sources` | List all registered RSS feeds | Public |
| `POST` | `/api/sources` | Register a new RSS news feed | Admin JWT |
| `POST` | `/api/sources/fetch-now` | **Trigger immediate RSS fetch cycle** | Admin JWT |
| `POST` | `/api/sources/{id}/fetch-now` | Trigger single source RSS fetch | Admin JWT |
| `POST` | `/api/admin/login` | Authenticate admin & receive JWT | Public |
| `GET` | `/api/admin/stats` | Portal analytics and breakdown metrics | Admin JWT |
| `POST` | `/api/contact` | Public inquiry / news tip submission | Public |
| `GET` | `/api/contact` | Review reader feedback | Admin JWT |
| `GET` | `/sitemap.xml` | Search engine indexing XML sitemap | Public |
| `GET` | `/rss.xml` | Public RSS 2.0 feed for news aggregators | Public |

---

## 🌐 Production Deployment Guide

### Option 1: Microsoft Azure App Service
1. **Database:** Deploy an Azure SQL Database. Copy the connection string to App Service Configuration.
2. **Publish Backend:**
   ```powershell
   cd backend
   dotnet publish -c Release -o ./publish
   ```
3. Deploy the `./publish` directory to Azure App Service using Azure CLI or GitHub Actions.

### Option 2: Linux VPS / Docker / Nginx
1. Create a `Dockerfile` for the ASP.NET Core API.
2. Configure Nginx as a reverse proxy:
   ```nginx
   server {
       listen 80;
       server_name news.buddhaambedkarnews.org;
       location / {
           proxy_pass http://localhost:5000;
           proxy_http_version 1.1;
           proxy_set_header Upgrade $http_upgrade;
           proxy_set_header Connection keep-alive;
           proxy_set_header Host $host;
           proxy_cache_bypass $http_upgrade;
       }
   }
   ```
3. Install `systemd` service to keep `dotnet BuddhaAmbedkarNews.dll` running continuously as a daemon.

---

## 📜 Ethical Journalistic Principles

This news portal is built to preserve and disseminate the noble truths of **Tathagata Gautam Buddha** and the egalitarian principles of **Bodhisattva Dr. B. R. Ambedkar**. All automated news aggregation adheres to fair use, linking back to original sources to ensure transparent publisher attribution.

*Bhavatu Sabba Mangalam • Jai Bhim • May All Beings Be Free From Suffering*
