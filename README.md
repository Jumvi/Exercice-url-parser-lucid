# URL Shortener with QR Code Generator

A full-stack URL shortener application built with AdonisJS that creates short URLs with associated QR codes.

## Prerequisites

- Node.js (v20 or higher)
- PostgreSQL
- npm or yarn

## Getting Started

### 1. Clone & Install Dependencies

```bash
# Clone the repository
git clone <repository-url>
cd <project-folder>

# Install dependencies
npm install
```

### 2. Environment Setup

```bash
# Copy environment file
cp .env.example .env
```

Edit `.env` file with your configuration:

```env
PORT=3333
HOST=0.0.0.0
NODE_ENV=development
APP_KEY=<will-be-generated>
SESSION_DRIVER=cookie

# Database settings
DB_HOST=127.0.0.1
DB_PORT=5432
DB_USER=your_db_user
DB_PASSWORD=your_db_password
DB_DATABASE=url_shortener_db
```

Generate application key:

```bash
node ace generate:key
```

Copy the generated key to `APP_KEY` in `.env`

### 3. Install Required Packages

```bash
# Install QR code package
npm install qrcode
```

### 4. Database Setup

```bash
# Create your postgresql database with postgres user
psql -U postgres
CREATE DATABASE url_shortener_db;

# Run migrations after creating them
node ace migration:run
```

### 5. Create and Configure Components

1. Generate URL Migration:
```bash
node ace make:migration urls
```
- Add the required table columns in the migration file
- Include: id, full_url, short_url, qr_code, timestamps

2. Generate URL Model:
```bash
node ace make:model Url
```
- Define the fillable properties
- Add the necessary column decorators

3. Create URL Controller:
```bash
node ace make:controller Url
```
- Implement CRUD operations
- Handle QR code generation
- Manage URL shortening logic

4. Configure Routes:
- Add routes in `start/routes.ts`
- Include routes for all CRUD operations

### 6. Running the Application

```bash
# Start the development server
node ace serve --watch
```

Visit `http://localhost:3333` in your browser.

## Features to Implement

1. URL Management:
   - Create new short URLs
   - Generate QR codes automatically
   - List all URLs
   - Update existing URLs
   - Delete URLs

2. User Interface:
   - Form for URL submission
   - Display QR codes
   - Copy shortened URLs
   - Show success/error messages


```
### 5. Application Pages Implementation

#### 5.1 Home Page
As a user, I want to:
- Enter my full URL in the "Entrez URL" field
- Enter my desired short URL in the "Entrer le shorturl au choix" field
- Click "Envoyer l'URL" button to:
  - Create a new short URL linked to my full URL
  - Be redirected to the goToUrl page

#### 5.2 goToUrl Page
The page already provides:
- A search form with a "phrase à encoder" input field
- A "Send Keyword" button
- A sidebar layout

As a user, I need to:

1. URL Redirection
   - Enter a short URL in the "phrase à encoder" field
   - Click "Send Keyword" to be redirected to the corresponding full URL

2. Sidebar URL Listing
   - View a list of all existing short URLs in the sidebar
   - For each short URL entry:
     - Click to reveal:
       - QR code display
       - Delete option
       - Update form for short URL and full URL
     - Scan QR code to view and access the full URL

3. URL Management
   - Update existing URLs:
     - Modify short URL
     - Modify full URL
   - Delete URLs
   - QR code functionality:
     - Display QR code for each URL
     - Scan to access full URL
     - Automatic redirection when scanned

Note: The basic template with form and display code is already provided. Students need to implement the functionality described above.


# Deploying Your AdonisJS Application on Render

This step-by-step guide will help you deploy your AdonisJS application on Render's cloud platform.

## Prerequisites

- A [Render account](https://render.com/signup)
- Your AdonisJS project on [GitHub](https://github.com)
- PostgreSQL database (Render provides a free tier)

## 1. Project Preparation 🔧

### 1.1 Environment Variables

Create or update your `.env.example` file:

```env
PORT=3333
HOST=0.0.0.0
NODE_ENV=production
APP_KEY=your-app-key
DRIVE_DISK=local
DB_CONNECTION=pg
POSTGRES_HOST=your-db-host
POSTGRES_PORT=5432
POSTGRES_USER=your-db-user
POSTGRES_PASSWORD=your-db-password
POSTGRES_DB_NAME=your-db-name
```

### 1.2 Update package.json

Add these deployment scripts:

```json
{
  "scripts": {
    "build": "node ace build --production",
    "start": "node server.js",
    "install-dependencies": "npm install"
  }
}
```

### 1.3 Database Configuration

Update `config/database.ts`:

```typescript
import Env from '@ioc:Adonis/Core/Env'
import { DatabaseConfig } from '@ioc:Adonis/Lucid/Database'

const databaseConfig: DatabaseConfig = {
  connection: Env.get('DB_CONNECTION'),
  connections: {
    pg: {
      client: 'pg',
      connection: {
        host: Env.get('POSTGRES_HOST'),
        port: Env.get('POSTGRES_PORT'),
        user: Env.get('POSTGRES_USER'),
        password: Env.get('POSTGRES_PASSWORD'),
        database: Env.get('POSTGRES_DB_NAME'),
        ssl: {
          rejectUnauthorized: false
        }
      }
    }
  }
}

export default databaseConfig
```

## 2. Setting Up Database on Render 💾

1. Log in to [Render Dashboard](https://dashboard.render.com)
2. Click "New +" → "PostgreSQL"
3. Configure your database:
   - Give it a name
   - Choose region (closest to users)
   - Select free plan
4. **Save these credentials**:
   - `Internal Database URL`
   - `External Database URL`
   - `Database Name`
   - `User`
   - `Password`
   - `Host`
   - `Port`

## 3. Deploying Your Application 🚀

### 3.1 Create Web Service

1. Click "New +" → "Web Service"
2. Connect your GitHub repo
3. Configure service:
   - **Name**: Your app name
   - **Environment**: Node
   - **Region**: Choose closest
   - **Branch**: your branch
   - **Build Command**: `npm run install-dependencies && npm run build`
   - **Start Command**: `npm run start`

### 3.2 Environment Variables Setup

Add these in your Web Service's Environment section:

```env
PORT=3333
HOST=0.0.0.0
NODE_ENV=production
APP_KEY=<your-app-key>
DRIVE_DISK=local
DB_CONNECTION=pg
POSTGRES_HOST=<from-database-credentials>
POSTGRES_PORT=5432
POSTGRES_USER=<from-database-credentials>
POSTGRES_PASSWORD=<from-database-credentials>
POSTGRES_DB_NAME=<from-database-credentials>
```

## 4. Post-Deployment Steps ✅

1. Run migrations:
   - Go to Web Service → Shell
   - Run: `node ace migration:run`

2. Verify deployment:
   - Wait for deployment to complete
   - Click generated URL
   - Check application status

## Troubleshooting Guide 🔍

### Database Connection Issues
- ✓ Verify environment variables
- ✓ Check database credentials
- ✓ Confirm SSL configuration

### Migration Failures
- ✓ Check Render logs
- ✓ Verify migration files
- ✓ Test database access

### Application Start Issues
- ✓ Review build logs
- ✓ Check dependencies
- ✓ Verify start command

## Useful Resources 📚

- [Render Documentation](https://render.com/docs)
- [AdonisJS Deployment Guide](https://docs.adonisjs.com/guides/deployment)
- [PostgreSQL on Render](https://render.com/docs/databases)
- [Node.js on Render](https://render.com/docs/deploy-node-express-app)
