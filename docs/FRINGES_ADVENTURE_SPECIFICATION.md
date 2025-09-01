# Fringes Adventure - Complete Application Specification

*Created for Project Recreation with Modern Technologies*

## 🎯 Project Overview

**Fringes Adventure** is a text-based interactive fiction game engine with web interface, supporting multiple games, user authentication, inventory management, and progressive storytelling.

## 🏗️ Core Architecture

### Current Technology Stack
- **Backend**: Flask (Python web framework)
- **Database**: SQLite with SQLAlchemy ORM
- **Authentication**: Flask-Login with session management
- **Frontend**: Bootstrap 5 + jQuery + AJAX
- **Templates**: Jinja2 server-side rendering
- **Security**: CSRF protection with Flask-WTF

### Target Technology Stack (Recreation)
- **Backend**: FastAPI (modern async Python framework)
- **Database**: PostgreSQL with SQLAlchemy 2.0 + Alembic migrations
- **Authentication**: JWT tokens with refresh mechanism
- **Frontend**: React/Next.js or Streamlit for rapid development
- **API**: RESTful API with OpenAPI documentation
- **Security**: Pydantic validation + modern security practices

## 📊 Data Models

### Core Entities

#### 1. User Model
```python
class User:
    id: int (Primary Key)
    username: str (Unique)
    password_hash: str
    is_admin: bool
    audio_enabled: bool
    session_token: str (Optional) - For single-session enforcement
    session_token_issued_at: datetime (Optional) - Session token timestamp
    active_game_id: int (Foreign Key to Game)
```

#### 2. Game Model
```python
class Game:
    id: int (Primary Key)
    abbreviation: str (Unique, e.g., "phoenix-gem")
    title: str
    description: str
    version: str
    is_active: bool
    starting_scene: str
```

#### 3. Scene Model
```python
class Scene:
    id: int (Primary Key)
    name: str (Unique)
    visited_by: List[User] (Many-to-Many)
```

#### 4. Item Model
```python
class Item:
    id: int (Primary Key)
    name: str (Unique)
    description: str
    points: int (Default: 10)
    is_challenge_reward: bool
    useless_notification: str (Optional)
    inventory_increase: int (Default: 0)
```

#### 5. UserGame Model (Normalized)
```python
class UserGame:
    id: int (Primary Key)
    user_id: int (Foreign Key)
    game_id: int (Foreign Key)
    points: int
    inventory_capacity: int (Default: 3)
    last_scene: str
```

#### 6. UserProgress Model
```python
class UserProgress:
    id: int (Primary Key)
    user_id: int (Foreign Key)
    game_id: int (Foreign Key)
    progress_type: ProgressType (Enum)
    scene: str (Optional)
    item: str (Optional)
    option: str (Optional)
    timestamp: datetime
```

### Progress Types Enum
```python
class ProgressType(Enum):
    TAKEN = "taken"
    USED = "used"
    REVEALED = "revealed"
    OPTION = "option"
    HIDDEN = "hidden"
```

## 🎮 Game Features

### 1. User Authentication & Management
- **Registration**: Username/password registration
- **Login**: Session-based authentication
- **Admin System**: Admin user privileges
- **Session Management**: Single-session enforcement with tokens
- **Audio Preferences**: User-configurable audio settings
- **Session Token Validation**: Automatic session invalidation on new login

### 2. Multi-Game Support
- **Game Selection**: Users can have active games
- **Game Metadata**: Title, description, version tracking
- **Game Isolation**: Separate progress per game
- **Game Assets**: Images, audio, scenes per game

### 3. Scene Navigation System
- **JSON-Driven Scenes**: Scene definitions in JSON files
- **Directional Movement**: North, South, East, West navigation
- **Exit Requirements**: Items required to access certain exits
- **Scene History**: Track visited scenes per user
- **Scene Assets**: Background images and audio per scene
- **Scene Options**: Multiple navigation choices with points
- **Success Messages**: Custom messages with delays for navigation

### 4. Inventory Management
- **Item Collection**: Pick up items from scenes
- **Item Usage**: Use items to unlock content
- **Inventory Capacity**: Limited inventory slots (default: 3)
- **Item Requirements**: Items needed to access certain content
- **Inventory Persistence**: Items persist across sessions
- **Item Drop**: Remove items from inventory
- **Inventory Increase**: Items that expand capacity

### 5. Progressive Game State
- **Points System**: Points awarded for actions
- **Progress Tracking**: Comprehensive progress logging
- **Item Reveals**: Hidden items that can be revealed
- **Challenge Rewards**: Special items with higher point values
- **State Persistence**: All progress saved to database
- **Generic Reveal System**: Hide/show items and options based on progress

### 6. Interactive Elements
- **AJAX Interactions**: All game actions use AJAX
- **Real-time Updates**: Inventory and points update without page reload
- **Error Handling**: Graceful error responses
- **Loading States**: Visual feedback during actions
- **Notifications**: Toast messages for user feedback
- **Background Audio**: Scene-specific audio with user controls
- **Background Images**: Scene-specific visual backgrounds

### 7. Admin & Development Features
- **Builder Mode**: Admin-only overlay showing hidden content
- **Scene Debugging**: View all scene data including hidden elements
- **User Management**: Admin interface for user administration
- **Game Reset**: Reset user progress to beginning
- **Maintenance Mode**: System maintenance interface

### 8. Advanced Features
- **Audio Controls**: User toggle for background audio
- **Session Security**: Single-session enforcement
- **Browser History**: Back/forward button support
- **Responsive Design**: Mobile-friendly interface
- **Error Recovery**: Graceful handling of connection issues
- **Version Display**: Show application version in UI

## 🔧 Technical Features

### 1. Database Architecture
- **Environment Support**: Production, Development, Test environments
- **SQLite for Development**: In-memory database for testing
- **File-based Production**: Persistent SQLite for production
- **Migration Support**: Database schema versioning
- **Indexing**: Optimized queries with proper indexes

### 2. Security Features
- **CSRF Protection**: Form submission protection
- **Password Hashing**: Secure password storage
- **Session Security**: Secure session management
- **Input Validation**: Server-side validation
- **SQL Injection Prevention**: ORM-based queries
- **Session Token Validation**: Single-session enforcement

### 3. Development Tools
- **Hot Reload**: Development server with auto-restart
- **Debug Logging**: Environment-aware logging
- **Admin Utilities**: User management tools
- **Testing Framework**: Comprehensive test suite
- **Code Quality**: Type hints, linting, documentation

### 4. Asset Management
- **Static File Serving**: Images, audio, CSS, JS
- **Game-specific Assets**: Organized by game directory
- **Asset Optimization**: Efficient file serving
- **Caching**: Static asset caching

### 5. Frontend Architecture
- **Modular JavaScript**: ES6 modules for UI components
- **Event-Driven**: Custom events for component communication
- **Service Layer**: API abstraction layer
- **Template System**: Dynamic HTML generation
- **State Management**: Client-side state tracking

## 📁 File Structure

### Current Structure
```
fringes_adventure/
├── app/
│   ├── database/          # Database setup & management
│   ├── models/            # SQLAlchemy models
│   ├── routes/            # Flask blueprints & endpoints
│   ├── services/          # Business logic layer
│   ├── templates/         # Jinja2 templates
│   └── static/            # Frontend assets
│       ├── js/
│       │   ├── services/  # API service layer
│       │   ├── ui/        # UI components
│       │   └── templates/ # HTML templates
│       ├── css/           # Stylesheets
│       └── images/        # Static images
├── games/                 # Game content
│   ├── phoenix-gem/       # Game-specific assets
│   └── stellar/           # Game-specific assets
├── docs/                  # Documentation
├── utility/               # Development tools
├── data/                  # Database storage
└── system/                # Production configs
```

### Target Structure (Recreation)
```
fringes_adventure_v2/
├── backend/               # FastAPI application
│   ├── app/
│   │   ├── models/        # Pydantic models
│   │   ├── api/           # API routes
│   │   ├── services/      # Business logic
│   │   ├── database/      # Database setup
│   │   └── auth/          # Authentication
│   ├── alembic/           # Database migrations
│   └── tests/             # Backend tests
├── frontend/              # React/Next.js or Streamlit
│   ├── components/        # UI components
│   ├── pages/             # Application pages
│   ├── services/          # API clients
│   └── styles/            # CSS/styling
├── games/                 # Game content (unchanged)
├── docs/                  # Documentation
└── docker/                # Containerization
```

## 🔄 API Endpoints

### Authentication
- `POST /auth/register` - User registration
- `POST /auth/login` - User login
- `POST /auth/logout` - User logout
- `POST /auth/refresh` - Refresh JWT token

### Game Management
- `GET /games` - List available games
- `GET /games/{game_id}` - Get game details
- `POST /games/{game_id}/activate` - Activate game for user

### Scene Navigation
- `GET /games/{game_id}/scenes/{scene_name}` - Get scene data
- `POST /games/{game_id}/scenes/{scene_name}/move` - Move to new scene
- `GET /games/{game_id}/scenes/{scene_name}/items` - Get scene items
- `POST /games/{game_id}/scenes/{scene_name}/options` - Select scene option
- `GET /games/{game_id}/scenes/{scene_name}/builder` - Get builder scene data (admin)

### Inventory Management
- `GET /games/{game_id}/inventory` - Get user inventory
- `POST /games/{game_id}/inventory/take/{item_name}` - Take item
- `POST /games/{game_id}/inventory/use/{item_name}` - Use item
- `DELETE /games/{game_id}/inventory/drop/{item_name}` - Drop item

### Progress Tracking
- `GET /games/{game_id}/progress` - Get user progress
- `POST /games/{game_id}/progress/reveal` - Reveal hidden item
- `GET /games/{game_id}/stats` - Get user statistics

### User Management
- `GET /api/v1/user/info` - Get user information
- `POST /api/v1/user/audio-toggle` - Toggle audio preference
- `POST /api/v1/user/reset-game` - Reset user game progress

### System
- `GET /api/v1/status` - System status and version
- `GET /api/v1/health` - Health check endpoint

## 🎨 User Interface Requirements

### 1. Game Interface
- **Scene Display**: Text description with background image
- **Navigation Controls**: Directional buttons (N/S/E/W)
- **Inventory Panel**: Current items with take/use/drop actions
- **Points Display**: Current score
- **Audio Controls**: Play/pause background audio
- **Responsive Design**: Mobile-friendly layout
- **Builder Mode Overlay**: Admin-only debug information

### 2. Authentication Interface
- **Login Form**: Username/password with validation
- **Registration Form**: New user registration
- **Error Handling**: Clear error messages
- **Success Feedback**: Confirmation messages

### 3. Admin Interface
- **User Management**: View/edit user accounts
- **Game Management**: Configure games and content
- **Statistics**: View game statistics
- **Maintenance**: System maintenance tools
- **Builder Mode**: Toggle for development debugging

### 4. Navigation & UX
- **Browser History**: Back/forward button support
- **Loading States**: Visual feedback during operations
- **Error Recovery**: Graceful handling of failures
- **Notifications**: Toast messages for user feedback
- **Confirmation Modals**: Confirm destructive actions

## 🔒 Security Requirements

### 1. Authentication Security
- **JWT Tokens**: Secure token-based authentication
- **Password Security**: Strong password requirements
- **Session Management**: Secure session handling
- **Rate Limiting**: Prevent brute force attacks
- **Single Session**: Enforce one active session per user

### 2. Data Security
- **Input Validation**: Server-side validation
- **SQL Injection Prevention**: Parameterized queries
- **XSS Prevention**: Output sanitization
- **CSRF Protection**: Cross-site request forgery protection

### 3. API Security
- **CORS Configuration**: Proper cross-origin settings
- **API Rate Limiting**: Prevent abuse
- **Request Validation**: Pydantic model validation
- **Error Handling**: Secure error responses

## 🧪 Testing Requirements

### 1. Unit Testing
- **Model Testing**: Database model validation
- **Service Testing**: Business logic testing
- **API Testing**: Endpoint functionality
- **Authentication Testing**: Security validation

### 2. Integration Testing
- **Database Integration**: Full database operations
- **API Integration**: Complete API workflows
- **Game Flow Testing**: End-to-end game scenarios
- **Authentication Flow**: Complete auth workflows

### 3. Performance Testing
- **Load Testing**: Multiple concurrent users
- **Database Performance**: Query optimization
- **Asset Loading**: Static file performance
- **Memory Usage**: Resource consumption

## 🚀 Deployment Requirements

### 1. Containerization
- **Docker Support**: Containerized application
- **Multi-stage Builds**: Optimized production images
- **Environment Configuration**: Environment-specific configs
- **Health Checks**: Application health monitoring

### 2. Database Migration
- **Alembic Migrations**: Database schema versioning
- **Migration Scripts**: Automated migration deployment
- **Rollback Support**: Migration rollback capability
- **Data Seeding**: Initial data population

### 3. Production Deployment
- **Reverse Proxy**: Nginx configuration
- **Process Management**: Systemd service files
- **Logging**: Structured logging
- **Monitoring**: Application monitoring
- **Backup Strategy**: Database backup procedures

## 📈 Performance Requirements

### 1. Response Times
- **Page Load**: < 2 seconds
- **API Responses**: < 500ms
- **Database Queries**: < 100ms
- **Asset Loading**: < 1 second

### 2. Scalability
- **Concurrent Users**: Support 100+ concurrent users
- **Database Connections**: Connection pooling
- **Static Assets**: CDN-ready asset serving
- **Caching**: Redis caching for frequently accessed data

### 3. Resource Usage
- **Memory**: < 512MB per application instance
- **CPU**: Efficient query processing
- **Storage**: Optimized database storage
- **Network**: Efficient API communication

## 🔄 Migration Strategy

### Phase 1: Backend Recreation
1. **FastAPI Setup**: Create new FastAPI application
2. **Database Migration**: Convert to PostgreSQL with Alembic
3. **API Development**: Recreate all API endpoints
4. **Authentication**: Implement JWT-based auth
5. **Testing**: Comprehensive test suite

### Phase 2: Frontend Recreation
1. **UI Framework**: Choose React/Next.js or Streamlit
2. **Component Development**: Recreate all UI components
3. **API Integration**: Connect frontend to new API
4. **User Experience**: Enhance UI/UX
5. **Testing**: Frontend testing

### Phase 3: Deployment & Migration
1. **Containerization**: Docker setup
2. **Production Deployment**: Deploy new application
3. **Data Migration**: Migrate existing user data
4. **Testing**: Production testing
5. **Go-live**: Switch to new application

## 📋 Success Criteria

### Functional Requirements
- ✅ All current features working
- ✅ Improved performance
- ✅ Enhanced security
- ✅ Better user experience
- ✅ Modern technology stack

### Technical Requirements
- ✅ Comprehensive test coverage
- ✅ API documentation
- ✅ Monitoring and logging
- ✅ Scalable architecture
- ✅ Production-ready deployment

### User Experience Requirements
- ✅ Faster response times
- ✅ Better mobile experience
- ✅ Enhanced accessibility
- ✅ Improved error handling
- ✅ Modern UI design

---

*This specification serves as the complete blueprint for recreating Fringes Adventure with modern technologies while preserving all existing functionality and improving the overall user experience.*
