# Real Estate AI Platform - Technical Documentation

## 📋 Table of Contents
1. [System Overview](#system-overview)
2. [Architecture Design](#architecture-design)
3. [API Reference](#api-reference)
4. [Database Schema](#database-schema)
5. [Frontend Components](#frontend-components)
6. [Integration Guide](#integration-guide)
7. [Security Implementation](#security-implementation)
8. [Performance Optimization](#performance-optimization)

## 🏗 System Overview

The Real Estate AI Platform is an enterprise-grade SaaS solution that enables real estate professionals to create AI-powered chatbots for property marketing. The platform features multi-tenant architecture, advanced AI training capabilities, and comprehensive white-label customization options.

### Core Value Proposition
- **AI-Powered Property Marketing**: Automated chatbots for property inquiries
- **Multi-Modal Training**: Process websites, documents, images, and audio
- **QR Code Integration**: Seamless property marketing campaigns
- **White-Label Platform**: Complete customization and branding
- **Enterprise Analytics**: Comprehensive reporting and insights

### Target Market
- Real estate agencies and brokerages
- Property management companies
- Individual real estate agents
- PropTech companies seeking white-label solutions

## 🏛 Architecture Design

### High-Level Architecture
```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Frontend      │    │    Backend      │    │   External      │
│   (React SPA)   │◄──►│   (Flask API)   │◄──►│   Services      │
│                 │    │                 │    │                 │
│ • Dashboard     │    │ • Authentication│    │ • OpenAI API    │
│ • Bot Builder   │    │ • Chatbot Logic │    │ • Stripe API    │
│ • Analytics     │    │ • QR Generation │    │ • MLS Data      │
│ • Settings      │    │ • Credit System │    │ • Zillow API    │
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │                       │                       │
         └───────────────────────┼───────────────────────┘
                                 │
                    ┌─────────────────┐
                    │    Database     │
                    │   (SQLite)      │
                    │                 │
                    │ • Users/Orgs    │
                    │ • Chatbots      │
                    │ • Properties    │
                    │ • Transactions  │
                    └─────────────────┘
```

### Technology Stack

#### Backend Technologies
- **Python 3.11**: Core programming language
- **Flask 3.1.1**: Web framework
- **SQLAlchemy**: ORM for database operations
- **Flask-JWT-Extended**: JWT authentication
- **Gunicorn**: WSGI HTTP server
- **Stripe SDK**: Payment processing
- **OpenAI SDK**: AI model integration
- **BeautifulSoup**: Web scraping
- **Pillow**: Image processing

#### Frontend Technologies
- **React 19**: UI framework
- **Vite 6**: Build tool and dev server
- **Tailwind CSS**: Utility-first CSS framework
- **shadcn/ui**: Component library
- **React Router**: Client-side routing
- **Axios**: HTTP client
- **Recharts**: Data visualization
- **Lucide Icons**: Icon library

#### Infrastructure
- **SQLite**: Database (production-ready for MVP)
- **Gunicorn**: Production WSGI server
- **CORS**: Cross-origin resource sharing
- **JWT**: Stateless authentication
- **Stripe**: Payment processing infrastructure

### Multi-Tenant Architecture

The platform implements a comprehensive multi-tenant system:

```python
# Organization-based tenancy
class Organization(db.Model):
    org_id = db.Column(db.String(36), primary_key=True)
    org_name = db.Column(db.String(100), nullable=False)
    branding_config = db.Column(db.JSON)
    current_credits = db.Column(db.Integer, default=0)
    
    # Relationships
    users = db.relationship('User', backref='organization')
    chatbots = db.relationship('Chatbot', backref='organization')
    properties = db.relationship('Property', backref='organization')
```

### Security Architecture

#### Authentication Flow
1. User registration creates organization
2. JWT tokens issued upon login
3. Tokens include user_id and organization_id
4. All API requests validate JWT and tenant isolation
5. Automatic token refresh mechanism

#### Data Isolation
- All models include organization_id foreign key
- Database queries automatically filter by organization
- API endpoints enforce tenant boundaries
- File uploads isolated by organization

## 📡 API Reference

### Authentication Endpoints

#### POST /api/auth/register
Register a new user and organization.

**Request Body:**
```json
{
  "first_name": "John",
  "last_name": "Doe",
  "email": "john@example.com",
  "password": "securepassword",
  "org_name": "Doe Real Estate"
}
```

**Response:**
```json
{
  "message": "User registered successfully",
  "user_id": "uuid-string",
  "organization_id": "uuid-string"
}
```

#### POST /api/auth/login
Authenticate user and receive JWT token.

**Request Body:**
```json
{
  "email": "john@example.com",
  "password": "securepassword"
}
```

**Response:**
```json
{
  "access_token": "jwt-token-string",
  "user_id": "uuid-string",
  "organization_id": "uuid-string",
  "role": "admin"
}
```

### Chatbot Management

#### POST /api/chatbots
Create a new chatbot (costs 10 credits).

**Headers:**
```
Authorization: Bearer <jwt-token>
```

**Request Body:**
```json
{
  "name": "Property Assistant",
  "description": "AI assistant for luxury properties",
  "personality": "professional",
  "knowledge_base": "Real estate expertise",
  "response_style": "helpful"
}
```

**Response:**
```json
{
  "message": "Chatbot created successfully",
  "chatbot": {
    "bot_id": "uuid-string",
    "name": "Property Assistant",
    "status": "active",
    "created_at": "2025-08-14T15:30:00Z"
  },
  "credits_used": 10
}
```

#### GET /api/chatbots
List all chatbots for the organization.

**Response:**
```json
[
  {
    "bot_id": "uuid-string",
    "name": "Property Assistant",
    "description": "AI assistant for luxury properties",
    "status": "active",
    "created_at": "2025-08-14T15:30:00Z",
    "conversation_count": 127,
    "last_active": "2025-08-14T18:45:00Z"
  }
]
```

### Property Management

#### POST /api/properties/search
Search properties across multiple data sources.

**Request Body:**
```json
{
  "address": "123 Main St",
  "city": "San Francisco",
  "state": "CA",
  "zip_code": "94102"
}
```

**Response:**
```json
{
  "search_results": {
    "basic_info": {
      "listing_id": "MLS123456",
      "price": 450000,
      "bedrooms": 3,
      "bathrooms": 2.5,
      "square_feet": 2100
    },
    "financial_info": {
      "zestimate": 465000,
      "rent_estimate": 2800,
      "price_history": [...]
    },
    "location_info": {
      "walk_score": 75,
      "schools": [...]
    }
  },
  "sources_searched": ["MLS", "Zillow", "Rentals"]
}
```

### Advanced Training

#### POST /api/training/url-content
Process content from a URL for training (costs 5 credits).

**Request Body:**
```json
{
  "url": "https://example-property-website.com"
}
```

**Response:**
```json
{
  "message": "URL content processed successfully",
  "processed_content": {
    "title": "Luxury Downtown Condo",
    "paragraphs": ["..."],
    "images": ["..."],
    "features": ["..."]
  },
  "credits_used": 5
}
```

#### POST /api/training/upload-document
Upload and process documents (supports PDF, Word, images, audio).

**Request:**
- Multipart form data with file upload
- Supported formats: PDF, DOCX, TXT, JPG, PNG, MP4, MP3, etc.

**Response:**
```json
{
  "message": "Document processed successfully",
  "processed_content": {
    "file_type": "pdf",
    "text": "Extracted text content...",
    "analysis": "AI analysis of content...",
    "features": ["..."]
  },
  "credits_used": 5
}
```

### Credit Management

#### GET /api/credits/balance
Get current credit balance.

**Response:**
```json
{
  "current_credits": 1000,
  "subscription_plan": "starter",
  "usage_this_month": 150,
  "credits_remaining": 850
}
```

#### POST /api/credits/purchase
Purchase credits with Stripe.

**Request Body:**
```json
{
  "amount": 1000,
  "payment_method_id": "pm_stripe_id"
}
```

**Response:**
```json
{
  "message": "Credits purchased successfully",
  "credits_added": 1000,
  "new_balance": 2000,
  "transaction_id": "txn_uuid"
}
```

### White-Label Features

#### POST /api/organizations/me/branding
Update organization branding.

**Request Body:**
```json
{
  "company_name": "Custom Real Estate",
  "logo_url": "https://example.com/logo.png",
  "primary_color": "#2563EB",
  "custom_domain": "chat.customrealestate.com"
}
```

## 🗄 Database Schema

### Core Models

#### Users Table
```sql
CREATE TABLE users (
    user_id VARCHAR(36) PRIMARY KEY,
    organization_id VARCHAR(36) NOT NULL,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    role VARCHAR(20) DEFAULT 'user',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (organization_id) REFERENCES organizations(org_id)
);
```

#### Organizations Table
```sql
CREATE TABLE organizations (
    org_id VARCHAR(36) PRIMARY KEY,
    org_name VARCHAR(100) NOT NULL,
    contact_email VARCHAR(100),
    current_credits INTEGER DEFAULT 0,
    subscription_plan VARCHAR(50),
    branding_config JSON,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

#### Chatbots Table
```sql
CREATE TABLE chatbots (
    bot_id VARCHAR(36) PRIMARY KEY,
    organization_id VARCHAR(36) NOT NULL,
    name VARCHAR(100) NOT NULL,
    description TEXT,
    personality VARCHAR(50),
    knowledge_base TEXT,
    response_style VARCHAR(50),
    status VARCHAR(20) DEFAULT 'active',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (organization_id) REFERENCES organizations(org_id)
);
```

#### Properties Table
```sql
CREATE TABLE properties (
    property_id VARCHAR(36) PRIMARY KEY,
    organization_id VARCHAR(36) NOT NULL,
    address VARCHAR(200) NOT NULL,
    city VARCHAR(100) NOT NULL,
    state VARCHAR(50) NOT NULL,
    zip_code VARCHAR(20) NOT NULL,
    property_type VARCHAR(50),
    price DECIMAL(12,2),
    bedrooms INTEGER,
    bathrooms DECIMAL(3,1),
    square_feet INTEGER,
    description TEXT,
    features JSON,
    data_sources JSON,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (organization_id) REFERENCES organizations(org_id)
);
```

#### QR Codes Table
```sql
CREATE TABLE qr_codes (
    qr_id VARCHAR(36) PRIMARY KEY,
    organization_id VARCHAR(36) NOT NULL,
    bot_id VARCHAR(36) NOT NULL,
    property_id VARCHAR(36),
    qr_data TEXT NOT NULL,
    scan_count INTEGER DEFAULT 0,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (organization_id) REFERENCES organizations(org_id),
    FOREIGN KEY (bot_id) REFERENCES chatbots(bot_id),
    FOREIGN KEY (property_id) REFERENCES properties(property_id)
);
```

#### Credit Transactions Table
```sql
CREATE TABLE credit_transactions (
    transaction_id VARCHAR(36) PRIMARY KEY,
    organization_id VARCHAR(36) NOT NULL,
    amount INTEGER NOT NULL,
    type VARCHAR(50) NOT NULL,
    related_entity_id VARCHAR(36),
    description TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (organization_id) REFERENCES organizations(org_id)
);
```

### Relationships and Constraints

#### Foreign Key Relationships
- Users belong to Organizations (1:N)
- Chatbots belong to Organizations (1:N)
- Properties belong to Organizations (1:N)
- QR Codes belong to Organizations and Chatbots (N:1:1)
- Credit Transactions belong to Organizations (1:N)

#### Indexes for Performance
```sql
CREATE INDEX idx_users_organization ON users(organization_id);
CREATE INDEX idx_chatbots_organization ON chatbots(organization_id);
CREATE INDEX idx_properties_organization ON properties(organization_id);
CREATE INDEX idx_qr_codes_organization ON qr_codes(organization_id);
CREATE INDEX idx_transactions_organization ON credit_transactions(organization_id);
CREATE INDEX idx_qr_codes_scan_count ON qr_codes(scan_count);
```

## 🎨 Frontend Components

### Component Architecture

The frontend follows a modular component architecture with clear separation of concerns:

```
src/
├── components/
│   ├── ui/                 # shadcn/ui components
│   ├── Layout.jsx          # Main layout wrapper
│   └── ...
├── pages/
│   ├── Login.jsx           # Authentication page
│   ├── Dashboard.jsx       # Main dashboard
│   └── ...
├── lib/
│   ├── api.js              # API client
│   ├── auth.jsx            # Authentication context
│   └── stripe.js           # Stripe utilities
└── hooks/                  # Custom React hooks
```

### Key Components

#### Authentication Context
```jsx
// lib/auth.jsx
export const AuthProvider = ({ children }) => {
  const [user, setUser] = useState(null);
  const [isAuthenticated, setIsAuthenticated] = useState(false);
  
  const login = async (email, password) => {
    const response = await authAPI.login({ email, password });
    // Handle authentication logic
  };
  
  return (
    <AuthContext.Provider value={{ user, login, logout }}>
      {children}
    </AuthContext.Provider>
  );
};
```

#### API Client
```javascript
// lib/api.js
const api = axios.create({
  baseURL: 'https://api-url/api',
  headers: { 'Content-Type': 'application/json' }
});

// Request interceptor for auth tokens
api.interceptors.request.use((config) => {
  const token = localStorage.getItem('access_token');
  if (token) {
    config.headers.Authorization = `Bearer ${token}`;
  }
  return config;
});
```

#### Dashboard Component
```jsx
// pages/Dashboard.jsx
const Dashboard = () => {
  const { user } = useAuth();
  const [stats, setStats] = useState({});
  
  useEffect(() => {
    loadDashboardData();
  }, []);
  
  return (
    <Layout>
      <StatsCards stats={stats} />
      <UsageChart data={usageData} />
      <QuickActions />
      <RecentActivity />
    </Layout>
  );
};
```

### Responsive Design

The frontend implements mobile-first responsive design:

```css
/* Tailwind CSS classes used throughout */
.container {
  @apply mx-auto px-4 sm:px-6 lg:px-8;
}

.grid-responsive {
  @apply grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6;
}

.sidebar {
  @apply fixed inset-y-0 left-0 z-50 w-64 transform transition-transform duration-300 ease-in-out lg:translate-x-0 lg:static;
}
```

### State Management

The application uses React Context for global state management:

- **AuthContext**: User authentication and session management
- **Local State**: Component-specific state with useState
- **API State**: Server state management with custom hooks

## 🔗 Integration Guide

### OpenAI Integration

The platform integrates with OpenAI for AI capabilities:

```python
# services/advanced_training_service.py
class AdvancedTrainingService:
    def __init__(self):
        self.openai_client = openai.OpenAI()
    
    def process_image_file(self, file_path, organization_id):
        response = self.openai_client.chat.completions.create(
            model="gpt-4-vision-preview",
            messages=[{
                "role": "user",
                "content": [{
                    "type": "text",
                    "text": "Analyze this real estate image..."
                }, {
                    "type": "image_url",
                    "image_url": {"url": f"data:image/jpeg;base64,{image_data}"}
                }]
            }],
            max_tokens=500
        )
        return response.choices[0].message.content
```

### Stripe Integration

Payment processing is handled through Stripe:

```python
# services/stripe_service.py
class StripeService:
    def __init__(self):
        stripe.api_key = os.environ.get('STRIPE_SECRET_KEY')
    
    def create_payment_intent(self, amount, customer_id):
        return stripe.PaymentIntent.create(
            amount=amount * 100,  # Convert to cents
            currency='usd',
            customer=customer_id,
            metadata={'organization_id': org_id}
        )
```

### External Data Sources

Property data integration supports multiple sources:

```python
# services/property_integration_service.py
class PropertyIntegrationService:
    def search_properties_by_address(self, address, city, state, zip_code):
        results = {
            'mls_data': self._search_mls(address, city, state, zip_code),
            'zillow_data': self._search_zillow(address, city, state, zip_code),
            'rental_data': self._search_rentals(address, city, state, zip_code)
        }
        return self._merge_property_data(results)
```

## 🔐 Security Implementation

### Authentication Security

#### JWT Token Management
```python
# JWT configuration
app.config['JWT_SECRET_KEY'] = os.environ.get('JWT_SECRET_KEY')
app.config['JWT_ACCESS_TOKEN_EXPIRES'] = timedelta(hours=24)

@jwt.token_in_blocklist_loader
def check_if_token_revoked(jwt_header, jwt_payload):
    # Token revocation logic
    return False
```

#### Password Security
```python
# Password hashing
from werkzeug.security import generate_password_hash, check_password_hash

def hash_password(password):
    return generate_password_hash(password, method='pbkdf2:sha256')

def verify_password(password, hash):
    return check_password_hash(hash, password)
```

### API Security

#### Request Validation
```python
# Input validation for all endpoints
@chatbot_bp.route('/chatbots', methods=['POST'])
@jwt_required()
def create_chatbot():
    data = request.json
    
    # Validate required fields
    required_fields = ['name', 'description']
    for field in required_fields:
        if field not in data:
            return jsonify({'error': f'Missing required field: {field}'}), 400
    
    # Sanitize input
    name = data['name'].strip()[:100]
    description = data['description'].strip()[:500]
```

#### CORS Configuration
```python
# CORS setup for cross-origin requests
from flask_cors import CORS

CORS(app, origins=[
    'http://localhost:3000',  # Development
    'https://your-domain.com'  # Production
])
```

### Data Protection

#### Multi-Tenant Data Isolation
```python
# Automatic tenant filtering
@jwt_required()
def get_user_chatbots():
    user_id = get_jwt_identity()
    user = User.query.get(user_id)
    
    # Only return chatbots for user's organization
    chatbots = Chatbot.query.filter_by(
        organization_id=user.organization_id
    ).all()
    
    return jsonify([bot.to_dict() for bot in chatbots])
```

#### API Key Encryption
```python
# Encrypt sensitive API keys
from cryptography.fernet import Fernet

def encrypt_api_key(api_key):
    key = os.environ.get('ENCRYPTION_KEY').encode()
    f = Fernet(key)
    return f.encrypt(api_key.encode()).decode()

def decrypt_api_key(encrypted_key):
    key = os.environ.get('ENCRYPTION_KEY').encode()
    f = Fernet(key)
    return f.decrypt(encrypted_key.encode()).decode()
```

## ⚡ Performance Optimization

### Backend Optimization

#### Database Query Optimization
```python
# Efficient queries with proper indexing
def get_organization_stats(org_id):
    # Single query with joins instead of multiple queries
    stats = db.session.query(
        func.count(Chatbot.bot_id).label('chatbot_count'),
        func.count(Property.property_id).label('property_count'),
        func.count(QRCode.qr_id).label('qr_count')
    ).select_from(Organization)\
     .outerjoin(Chatbot)\
     .outerjoin(Property)\
     .outerjoin(QRCode)\
     .filter(Organization.org_id == org_id)\
     .first()
    
    return stats
```

#### Caching Strategy
```python
# Redis caching for frequently accessed data
from flask_caching import Cache

cache = Cache(app, config={'CACHE_TYPE': 'redis'})

@cache.memoize(timeout=300)  # 5 minute cache
def get_property_market_data(address):
    # Expensive market data calculation
    return calculate_market_analysis(address)
```

#### Async Processing
```python
# Background task processing for heavy operations
from celery import Celery

celery = Celery('real_estate_ai')

@celery.task
def process_large_document(file_path, organization_id):
    # Process document in background
    service = AdvancedTrainingService()
    return service.process_document_upload(file_path, organization_id)
```

### Frontend Optimization

#### Code Splitting
```jsx
// Lazy loading for route components
import { lazy, Suspense } from 'react';

const Dashboard = lazy(() => import('./pages/Dashboard'));
const Analytics = lazy(() => import('./pages/Analytics'));

function App() {
  return (
    <Suspense fallback={<LoadingSpinner />}>
      <Routes>
        <Route path="/dashboard" element={<Dashboard />} />
        <Route path="/analytics" element={<Analytics />} />
      </Routes>
    </Suspense>
  );
}
```

#### API Request Optimization
```javascript
// Request deduplication and caching
const apiCache = new Map();

const cachedApiCall = async (endpoint, options = {}) => {
  const cacheKey = `${endpoint}-${JSON.stringify(options)}`;
  
  if (apiCache.has(cacheKey)) {
    return apiCache.get(cacheKey);
  }
  
  const response = await api.get(endpoint, options);
  apiCache.set(cacheKey, response.data);
  
  // Clear cache after 5 minutes
  setTimeout(() => apiCache.delete(cacheKey), 300000);
  
  return response.data;
};
```

#### Bundle Optimization
```javascript
// Vite configuration for optimal bundling
export default defineConfig({
  build: {
    rollupOptions: {
      output: {
        manualChunks: {
          vendor: ['react', 'react-dom'],
          ui: ['@radix-ui/react-dialog', '@radix-ui/react-dropdown-menu'],
          charts: ['recharts']
        }
      }
    },
    chunkSizeWarningLimit: 1000
  }
});
```

### Monitoring and Analytics

#### Performance Monitoring
```python
# Request timing middleware
import time
from flask import g

@app.before_request
def before_request():
    g.start_time = time.time()

@app.after_request
def after_request(response):
    duration = time.time() - g.start_time
    
    # Log slow requests
    if duration > 1.0:  # Log requests over 1 second
        app.logger.warning(f'Slow request: {request.endpoint} took {duration:.2f}s')
    
    return response
```

#### Error Tracking
```python
# Comprehensive error handling
import logging
from flask import jsonify

@app.errorhandler(Exception)
def handle_exception(e):
    # Log the error
    app.logger.error(f'Unhandled exception: {str(e)}', exc_info=True)
    
    # Return user-friendly error
    return jsonify({
        'error': 'An internal error occurred',
        'message': 'Please try again later'
    }), 500
```

---

This technical documentation provides comprehensive coverage of the Real Estate AI Platform's architecture, implementation details, and operational procedures. The platform is designed for enterprise-scale deployment with robust security, performance optimization, and maintainability features.

