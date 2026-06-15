# SplitEase

A full-stack web application for managing group expenses and splitting bills seamlessly. SplitEase allows users to create groups, track expenses, optimize debt settlements, and manage finances collaboratively.

---

## 📋 Table of Contents

- [Project Overview](#project-overview)
- [Tech Stack](#tech-stack)
- [Features](#features)
- [Project Structure](#project-structure)
- [Backend](#backend)
- [Frontend](#frontend)
- [Installation & Setup](#installation--setup)
- [Running the Application](#running-the-application)
- [Environment Variables](#environment-variables)
- [API Endpoints](#api-endpoints)
- [Key Components](#key-components)

---

## 🎯 Project Overview

SplitEase is a group expense management application that helps friends, roommates, and colleagues:
- Track shared expenses across groups
- Automatically calculate debts between members
- Optimize and simplify payment settlements
- Manage group membership with invite codes
- View activity logs and payment history
- Get AI-powered assistance with the SplitBot chatbot

---

## 🛠️ Tech Stack

### Backend
- **Framework**: Django 6.0.3
- **API**: Django REST Framework 3.16.1
- **Database**: PostgreSQL
- **Authentication**: JWT (Simple JWT 5.5.1)
- **CORS**: django-cors-headers 4.9.0
- **AI Integration**: Google Generative AI 0.8.6
- **Server**: Gunicorn 25.1.0

### Frontend
- **Framework**: React 19.2.4
- **Routing**: React Router DOM 7.13.1
- **HTTP Client**: Axios 1.13.6
- **Authentication**: Google OAuth (@react-oauth/google 0.13.4)
- **Build Tool**: React Scripts 5.0.1
- **Animations**: React Transition Group 4.4.5

---

## ✨ Features

### Group Management
- Create expense groups with custom names
- Add/remove group members
- Join groups using unique invite codes
- Track group creation and membership

### Expense Tracking
- Create expenses with multiple borrowers and lenders
- Support for multi-payer transactions
- Detailed expense descriptions and comments
- Edit and delete expenses
- Activity logging for all transactions

### Debt Management
- Automatic debt calculation between group members
- View individual user debts
- Track optimized debt settlements (debt simplification)
- Support for both single and multi-payer debt scenarios

### User Features
- User registration and authentication (JWT-based)
- Google OAuth integration
- Token refresh mechanism
- User profile management
- Email and name information

### AI Assistant
- **SplitBot**: AI-powered chatbot for assistance
- Powered by Google Generative AI
- Contextual help and recommendations

### Activity & Analytics
- Real-time activity feed
- User action logging (creation, modification, deletion)
- Bubble chart visualization of expenses
- Group summary statistics

---

## 📁 Project Structure

```
SplitEase/
├── backend/                 # Django REST API
│   ├── api/                 # Main app with models, views, serializers
│   │   ├── models.py        # Database models (Group, Expense, Debt, etc.)
│   │   ├── views.py         # API endpoints
│   │   ├── serializers.py   # Data serialization
│   │   ├── helpers.py       # Debt calculation helpers
│   │   ├── ai_utils.py      # AI/Bot utilities
│   │   ├── urls.py          # API routes
│   │   ├── admin.py         # Django admin configuration
│   │   └── migrations/      # Database migrations
│   ├── backend/             # Django settings
│   │   ├── settings.py      # Configuration
│   │   ├── urls.py          # Main URL routing
│   │   ├── asgi.py          # ASGI configuration
│   │   └── wsgi.py          # WSGI configuration
│   ├── manage.py            # Django CLI
│   ├── requirements.txt     # Python dependencies
│   ├── .env.example         # Environment template
│   └── build.sh             # Build script
│
├── frontend/                # React Frontend
│   ├── src/
│   │   ├── components/      # React components
│   │   │   ├── App.jsx              # Main app component
│   │   │   ├── AuthPage.jsx         # Login/Signup
│   │   │   ├── LandingPage.jsx      # Landing page
│   │   │   ├── GroupsDashboard.jsx  # Groups overview
│   │   │   ├── GroupView.jsx        # Group details
│   │   │   ├── GroupExpenses.jsx    # Expenses list
│   │   │   ├── AddExpense.jsx       # Create expense
│   │   │   ├── ActivityFeed.jsx     # Activity log
│   │   │   ├── BubbleChart.jsx      # Data visualization
│   │   │   ├── InvitePage.jsx       # Join group
│   │   │   ├── SplitBotModal.jsx    # AI assistant
│   │   │   └── ...other components
│   │   ├── styles/          # CSS stylesheets
│   │   ├── index.js         # Entry point
│   │   └── setupProxy.js    # Proxy configuration
│   ├── public/              # Static files
│   ├── package.json         # NPM dependencies
│   ├── package-lock.json    # Locked dependencies
│   └── .gitignore
│
└── README.md                # This file
```

---

## 🔧 Backend

### Database Models

**User** (Django's built-in)
- Django's default user model with authentication

**Group**
- `name`: Group identifier
- `created_by`: User who created the group
- `members`: Many-to-many relationship with users
- `created_at`: Timestamp
- `invite_code`: Unique 12-character code for joining

**Expense**
- `group`: Foreign key to Group
- `description`: Expense description
- `amount`: Total expense amount
- `created_by`: User who created the expense
- `created_at`: Timestamp

**ExpenseBorrower**
- `expense`: Foreign key to Expense
- `username`: Who owes money
- `amount`: Amount owed

**ExpenseLender**
- `expense`: Foreign key to Expense
- `username`: Who paid money
- `amount`: Amount paid

**Debt**
- `group`: Foreign key to Group
- `from_user`: Who owes
- `to_user`: Who is owed
- `amount`: Debt amount

**OptimisedDebt**
- Simplified debt representation after optimization

**UserDebt**
- `group`: Foreign key to Group
- `username`: User identifier
- `net_debt`: Aggregate debt (positive = owes, negative = owed)

**ActivityLog**
- `group`: Associated group
- `user`: User who performed action
- `action`: Type of action (create, update, delete)
- `description`: Details of the action
- `created_at`: When action occurred

**ExpenseComment**
- `expense`: Foreign key to Expense
- `author`: User who commented
- `text`: Comment content
- `created_at`: Timestamp

### Key Views (API Endpoints)

**Authentication**
- `POST /auth/signup/` - Register new user
- `POST /auth/login/` - User login
- `POST /auth/logout/` - User logout
- `POST /auth/token/refresh/` - Refresh JWT token
- `POST /auth/google/` - Google OAuth login

**Groups**
- `GET /groups/` - List user's groups
- `POST /groups/` - Create group
- `GET /groups/{id}/` - Get group details
- `PUT /groups/{id}/` - Update group
- `POST /groups/join/` - Join group by invite code
- `POST /groups/{id}/add-member/` - Add member to group

**Expenses**
- `GET /expenses/` - List expenses
- `POST /expenses/` - Create expense
- `GET /expenses/{id}/` - Get expense details
- `PUT /expenses/{id}/` - Update expense
- `DELETE /expenses/{id}/` - Delete expense

**Debts**
- `GET /debts/` - List debts
- `GET /debts/optimised/` - Get optimized debts

**Activity**
- `GET /activity/` - Activity feed

**AI Assistant**
- `POST /splitbot/` - Get bot response

### Helpers & Utilities

**helpers.py**
- `process_new_debt()`: Add new debt relationship
- `reverse_debt()`: Remove debt relationship
- `process_multi_payer_debt()`: Handle multi-payer scenarios
- `reverse_multi_payer_debt()`: Reverse multi-payer transactions
- `simplify_debts()`: Optimize and minimize debt transactions

**ai_utils.py**
- `get_bot_response()`: Generate AI responses using Google Generative AI

---

## ⚛️ Frontend

### Main Components

**App.jsx** - Root component
- Manages authentication state
- Sets up JWT token refresh interceptor
- Handles user login/logout
- Routes management

**AuthPage.jsx** - Authentication
- User signup with credentials
- User login
- Google OAuth integration
- Form validation

**LandingPage.jsx** - Welcome page
- Introduction to SplitEase
- Call-to-action buttons
- Navigation to signup/login

**GroupsDashboard.jsx** - Groups overview
- List all user's groups
- Create new group
- Access group details

**GroupView.jsx** - Group details
- View group members
- Add members
- Access expenses and debts
- Manage group settings

**GroupExpenses.jsx** - Expenses listing
- Display all group expenses
- Filter by date/member
- View expense details

**AddExpense.jsx** - Expense creation
- Multi-borrower support
- Multi-payer support
- Amount splitting
- Add comments

**ActivityFeed.jsx** - Activity log
- Recent transactions
- User actions
- Group activities

**BubbleChart.jsx** - Data visualization
- Visual representation of expenses
- Interactive bubble chart

**SplitBotModal.jsx** - AI Assistant
- Chat interface with SplitBot
- Context-aware responses

**InvitePage.jsx** - Group joining
- Join group by invite code
- Verify group membership

---

## 📦 Installation & Setup

### Prerequisites
- Python 3.9+
- Node.js 16+
- PostgreSQL 12+
- Git

### Backend Setup

1. **Navigate to backend directory**
   ```bash
   cd backend
   ```

2. **Create virtual environment**
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

4. **Setup environment variables**
   ```bash
   cp .env.example .env
   # Edit .env with your configuration
   ```

5. **Configure PostgreSQL**
   - Ensure PostgreSQL is running
   - Update database credentials in `.env`

6. **Run migrations**
   ```bash
   python manage.py migrate
   ```

7. **Create superuser (optional)**
   ```bash
   python manage.py createsuperuser
   ```

### Frontend Setup

1. **Navigate to frontend directory**
   ```bash
   cd frontend
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Create environment file**
   ```bash
   touch .env.local
   ```

4. **Add required environment variables**
   ```
   REACT_APP_GOOGLE_CLIENT_ID=your_google_client_id
   REACT_APP_API_URL=http://localhost:8000
   ```

---

## 🚀 Running the Application

### Start Backend Server

```bash
cd backend
python manage.py runserver 8001
```

The backend will be available at `http://localhost:8001`

Django admin available at: `http://localhost:8001/admin`

### Start Frontend Server

```bash
cd frontend
npm start
```

The frontend will open at `http://localhost:3000`

### Production Build

Frontend production build:
```bash
cd frontend
npm run build
```

Backend production (using Gunicorn):
```bash
cd backend
gunicorn backend.wsgi --bind 0.0.0.0:8001
```

---

## 🔐 Environment Variables

### Backend (.env)

```
# Django Settings
SECRET_KEY=your-secret-key-here
DEBUG=True
ALLOWED_HOSTS=*

# Database (PostgreSQL)
DATABASE_ENGINE=django.db.backends.postgresql
DATABASE_NAME=splitease
DATABASE_USER=postgres
DATABASE_PASSWORD=your_password_here
DATABASE_HOST=localhost
DATABASE_PORT=5432

# CORS (comma-separated for multiple origins)
CORS_ALLOWED_ORIGINS=http://localhost:3000

# Google API (for AI features)
GOOGLE_API_KEY=your_google_api_key
```

### Frontend (.env.local)

```
REACT_APP_GOOGLE_CLIENT_ID=your_google_client_id
REACT_APP_API_URL=http://localhost:8000
```

---

## 📡 API Endpoints Summary

### Authentication
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/auth/signup/` | User registration |
| POST | `/auth/login/` | User login |
| POST | `/auth/logout/` | User logout |
| POST | `/auth/token/refresh/` | Refresh JWT token |
| POST | `/auth/google/` | Google OAuth login |

### Groups
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/groups/` | List user's groups |
| POST | `/groups/` | Create new group |
| GET | `/groups/{id}/` | Get group details |
| PUT | `/groups/{id}/` | Update group |
| POST | `/groups/join/` | Join by invite code |
| POST | `/groups/{id}/add-member/` | Add member |

### Expenses
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/expenses/` | List expenses |
| POST | `/expenses/` | Create expense |
| GET | `/expenses/{id}/` | Get expense details |
| PUT | `/expenses/{id}/` | Update expense |
| DELETE | `/expenses/{id}/` | Delete expense |

### Debts
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/debts/` | List debts |
| GET | `/debts/optimised/` | Get optimized debts |

### Activity
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/activity/` | Get activity log |

### AI Assistant
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/splitbot/` | Chat with AI assistant |

---

## 🎨 Key Components & Features Explained

### Expense Tracking
Expenses can have multiple borrowers (who owe money) and lenders (who paid). The system automatically calculates debts based on expense distribution.

### Debt Optimization
The application includes a debt simplification algorithm that minimizes the number of transactions needed to settle all debts. Instead of multiple circular transactions, it finds the most efficient payment path.

### Group Invites
Groups can be shared via unique invite codes, allowing easy onboarding without requiring email invitations.

### JWT Authentication
The backend uses JWT tokens with refresh capability for stateless authentication, allowing for scalable API design.

### Google OAuth
Users can quickly authenticate using their Google account, streamlining the signup process.

### AI Assistant (SplitBot)
An AI-powered chatbot powered by Google Generative AI that can help with expense management queries and provide recommendations.

---

## 📝 Notes

- The backend defaults to port 8001 to avoid conflicts
- PostgreSQL must be running and configured before starting the backend
- Frontend proxy in `package.json` is set to `http://localhost:8000` for API requests
- All API requests require JWT authentication (except login/signup endpoints)
- Token refresh is automatic and handled via Axios interceptor

---

## 🤝 Contributing

To contribute to SplitEase:
1. Create a feature branch
2. Make your changes
3. Test thoroughly
4. Create a pull request

---

## 📄 License

This project is open source. See LICENSE file for details.

---

## 📧 Support

For issues, questions, or suggestions, please open an issue in the repository.

---

**Made with ❤️ for splitting expenses easily**
