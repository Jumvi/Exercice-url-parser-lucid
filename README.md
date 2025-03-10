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
