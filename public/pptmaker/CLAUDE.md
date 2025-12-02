# PPTMaker - Development Documentation

## Project Overview

**PPTMaker** is an iOS application with a Python FastAPI backend that generates AI-powered PowerPoint presentations. Users can generate presentation outlines using AI, edit them, select templates, and generate professional PPTX files.

**Owner:** KimBytes (com.kimbytes.pptmaker)
**Platform:** iOS (Swift/SwiftUI) + Python Backend (FastAPI)
**AI Provider:** OpenAI GPT or Anthropic Claude
**Production URL:** https://ppt-maker-server.onrender.com

---

## Architecture

### Backend (Python FastAPI)
- **Location:** `/Users/kbartiquel/Documents/PROJECTS/PPTMaker/backend/`
- **Port:** 8000 (local) / Render (production)
- **Framework:** FastAPI with Uvicorn

**Key Files:**
- `main.py` - API endpoints and request/response handling
- `ai_service.py` - AI integration (OpenAI/Anthropic)
- `presentation_generator.py` - PPTX generation using python-pptx
- `templates.py` - 15 template definitions and styling
- `settings_manager.py` - Paywall and settings configuration
- `admin.html` - Admin panel interface

### iOS App (Swift/SwiftUI)
- **Location:** `/Users/kbartiquel/Documents/PROJECTS/PPTMaker/iosApp/PPTMaker/`
- **Bundle ID:** com.kimbytes.pptmaker

**Views (9 files):**
- `ContentView.swift` - Main presentation creation flow
- `OutlineEditorView.swift` - Edit outline before generation
- `TemplateSelectionView.swift` - Browse and select templates
- `PresentationHistoryView.swift` - Saved presentations library
- `OnboardingView.swift` - First-time user experience
- `PaywallView.swift` - Default paywall display
- `CustomPaywallV1View.swift` - Custom paywall iteration 1
- `CustomPaywallV2View.swift` - Custom paywall iteration 2
- `CustomPaywallV3View.swift` - Custom paywall iteration 3

**Services (7 files):**
- `APIService.swift` - Backend communication
- `FileService.swift` - Local file management
- `HapticManager.swift` - Haptic feedback
- `RevenueCatService.swift` - Subscription management
- `LimitTrackingService.swift` - Usage limit tracking
- `AnalyticsService.swift` - Event tracking
- `PaywallSettingsService.swift` - Paywall configuration

**Models (2 files):**
- `SlideModels.swift` - Data structures for slides and outlines
- `Template.swift` - Template definitions (15 templates)

**Utilities:**
- `ColorScheme.swift` - Brand and semantic colors

---

## Core Features

### Implemented Features

1. **AI Outline Generation with Dynamic Slide Types**
   - User enters topic and number of slides (5-15)
   - Two-step AI analysis: Topic analysis → Intelligent slide type selection
   - **Dynamic vs Custom mode** - AI chooses types or user restricts
   - **12 presentation tones** + custom option:
     - Professional, Casual, Formal, Enthusiastic, Inspirational
     - Humorous, Technical, Creative, Persuasive, Educational
     - Friendly, Serious, Custom (user-defined)
   - Supports 5 slide types:
     - **Title** - Opening slide with main title and subtitle
     - **Content** - Traditional bullet point slides
     - **Section** - Large divider slides between major parts
     - **Quote** - Inspirational quotes with author attribution
     - **Two-Column** - Side-by-side comparisons (before/after, pros/cons)
   - Supports both OpenAI GPT and Anthropic Claude

2. **15 Professional Templates**
   - Corporate Professional (navy blue, classic style)
   - Creative Bold (purple/pink, geometric style)
   - Academic Classic (dark blue/gold, classic style)
   - Minimal Modern (black/green, minimal style)
   - Warm & Friendly (orange/green, modern style)
   - Tech Startup (indigo/purple, gradient style)
   - Nature Eco (forest green/lime, modern style)
   - Luxury Premium (dark gray/gold, classic style)
   - Vibrant Energy (hot pink/orange, geometric style)
   - Monochrome Elegant (dark/gray, minimal style)
   - Sunset Glow (red/orange/yellow, gradient style)
   - Ocean Blue (cyan/sky blue, gradient style)
   - Professional Dark (slate/gray, modern style)
   - Pastel Soft (purple/pink/lavender, modern style)
   - Retro Vintage (hot pink/purple/teal, geometric style)

3. **5 Design Styles**
   - Gradient - Color gradient backgrounds with decorative circles
   - Geometric - Contemporary shapes and angular designs
   - Minimal - Clean, minimal aesthetic
   - Classic - Traditional professional design
   - Modern - Bold, contemporary approach

4. **Editable Outline**
   - Add/remove slides
   - Reorder slides (drag & drop)
   - Edit titles, subtitles, and bullet points
   - Edit quote text and authors
   - Edit two-column content
   - Slide type badges in editor

5. **PPTX Generation**
   - Backend generates .pptx files using python-pptx
   - Returns file to iOS app
   - Saved to iOS documents directory

6. **Presentation History**
   - View all saved presentations
   - QuickLook preview
   - Delete presentations with confirmation
   - Share presentations (system share sheet)
   - Refresh presentation list

7. **Edit & Regenerate**
   - Saves outline JSON alongside PPTX
   - Load saved outline to edit
   - Regenerate with same or different template

8. **Paywall & Monetization**
   - RevenueCat SDK integration
   - Free tier limits:
     - 2 outline generations
     - 10 presentation generations
   - Premium tier (unlimited)
   - Multiple custom paywall UIs (V1, V2, V3)
   - Admin-configurable settings via backend
   - Weekly/monthly subscription options

9. **Admin Panel**
   - Web-based admin interface
   - Configure paywall settings
   - Adjust usage limits
   - Password-protected access

10. **Other Features**
    - Dark/Light mode support
    - Haptic feedback
    - Analytics tracking
    - Onboarding flow

### Removed Features

- **PDF Export** - Removed due to file corruption issues on iOS

---

## API Endpoints

| Method | Endpoint | Purpose |
|--------|----------|---------|
| GET | `/` | Health check and API info |
| GET | `/templates` | List all 15 templates |
| GET | `/settings` | Get paywall configuration |
| GET | `/health` | Detailed health check with AI status |
| POST | `/generate-outline` | Generate AI presentation outline |
| POST | `/generate-presentation` | Generate PPTX from outline |
| GET | `/admin` | Admin panel HTML page |
| POST | `/admin/auth` | Admin authentication |
| GET | `/admin/settings` | Get current settings |
| POST | `/admin/settings` | Update paywall settings |

### POST `/generate-outline`

**Request:**
```json
{
  "topic": "Climate Change",
  "num_slides": 8,
  "tone": "professional",
  "allowed_slide_types": ["content", "section", "quote", "two-column"]
}
```

**Response:**
```json
{
  "status": "success",
  "outline": {
    "presentation_title": "Climate Change: A Global Challenge",
    "slides": [...]
  },
  "metadata": {
    "topic": "Climate Change",
    "requested_slides": 8
  }
}
```

### POST `/generate-presentation`

**Request:**
```json
{
  "presentation_title": "Climate Change",
  "template": "corporate",
  "slides": [...]
}
```

**Response:** PPTX file (binary)

---

## User Flow

1. **Launch App** → Onboarding (first time) or main screen
2. **Enter Topic** → e.g., "Climate Change"
3. **Configure Options**:
   - Select number of slides (5-15)
   - Choose slide type mode (Dynamic or Custom)
   - Select presentation tone
4. **Generate Outline** → AI creates structured outline
5. **Edit Outline** → Add/remove/reorder slides, edit content
6. **Select Template** → Choose from 15 templates
7. **Generate** → Backend creates PPTX
8. **Save** → File saved to iOS documents directory
9. **View/Share** → Access from history, preview, share

---

## File Structure

```
PPTMaker/
├── backend/
│   ├── main.py                    # API endpoints
│   ├── ai_service.py              # AI integration
│   ├── presentation_generator.py  # PPTX generation
│   ├── templates.py               # 15 template definitions
│   ├── settings_manager.py        # Paywall settings
│   ├── admin.html                 # Admin panel
│   ├── requirements.txt           # Python dependencies
│   ├── render.yaml                # Render deployment config
│   ├── runtime.txt                # Python version
│   ├── .env                       # Environment variables
│   └── README.md
├── iosApp/
│   └── PPTMaker/
│       ├── Views/
│       │   ├── ContentView.swift
│       │   ├── OutlineEditorView.swift
│       │   ├── TemplateSelectionView.swift
│       │   ├── PresentationHistoryView.swift
│       │   ├── OnboardingView.swift
│       │   ├── PaywallView.swift
│       │   ├── CustomPaywallV1View.swift
│       │   ├── CustomPaywallV2View.swift
│       │   └── CustomPaywallV3View.swift
│       ├── ViewModels/
│       │   └── PresentationViewModel.swift
│       ├── Models/
│       │   ├── SlideModels.swift
│       │   └── Template.swift
│       ├── Services/
│       │   ├── APIService.swift
│       │   ├── FileService.swift
│       │   ├── HapticManager.swift
│       │   ├── RevenueCatService.swift
│       │   ├── LimitTrackingService.swift
│       │   ├── AnalyticsService.swift
│       │   └── PaywallSettingsService.swift
│       ├── Utilities/
│       │   └── ColorScheme.swift
│       └── Assets.xcassets/
└── CLAUDE.md (this file)
```

---

## Technical Details

### Backend Dependencies
```
fastapi==0.115.0
uvicorn[standard]==0.32.0
python-pptx==0.6.23
openai==1.54.0
anthropic==0.39.0
python-dotenv==1.0.1
pydantic==2.10.0
python-multipart==0.0.17
Pillow>=10.4.0
```

### iOS Requirements
- iOS 15.0+
- Swift 5
- SwiftUI
- QuickLook framework
- RevenueCat SDK

### Environment Variables
```env
# AI Configuration
AI_SERVICE=openai  # or anthropic
OPENAI_API_KEY=your_key_here
OPENAI_MODEL=gpt-3.5-turbo  # optional, default
ANTHROPIC_API_KEY=your_key_here
ANTHROPIC_MODEL=claude-sonnet-4-20250514  # optional, default

# Admin
ADMIN_PASSWORD=your_admin_password
```

### Paywall Settings (settings.json)
```json
{
  "presentationLimit": 10,
  "outlineLimit": 2,
  "hardPaywall": false,
  "customPaywall": true,
  "customPaywallVersion": 2,
  "paywallCloseButtonDelay": 1,
  "paywallCloseButtonDelayOnLimit": 35,
  "showPaywallOnStart": false,
  "custompaywallv2Monthly": false,
  "custompaywallv2Weekly": true
}
```

---

## Slide Data Models

### SlideData Structure
```swift
struct SlideData {
    var slideNumber: Int
    var type: String  // "title", "content", "section", "quote", "two-column"
    var title: String
    var subtitle: String?
    var bulletPoints: [String]?

    // Quote slides
    var quoteText: String?
    var quoteAuthor: String?

    // Two-column slides
    var columnLeftTitle: String?
    var columnLeftPoints: [String]?
    var columnRightTitle: String?
    var columnRightPoints: [String]?
}
```

### Example Slide Types

**Quote Slide:**
```json
{
  "slide_number": 3,
  "type": "quote",
  "title": "Expert Opinion",
  "quote_text": "The best way to predict the future is to invent it.",
  "quote_author": "Alan Kay"
}
```

**Two-Column Slide:**
```json
{
  "slide_number": 6,
  "type": "two-column",
  "title": "Before vs After",
  "column_left_title": "Before",
  "column_left_points": ["Manual process", "Time consuming", "Error prone"],
  "column_right_title": "After",
  "column_right_points": ["Automated", "Fast", "Accurate"]
}
```

**Section Slide:**
```json
{
  "slide_number": 4,
  "type": "section",
  "title": "Key Findings"
}
```

---

## Development

### Running Backend Locally
```bash
cd backend
source venv/bin/activate
python main.py
```
Visit: http://localhost:8000/docs (Swagger UI)

### Kill Existing Backend Process
```bash
lsof -ti:8000 | xargs kill -9
```

### iOS Development
Run in Xcode Simulator or physical device

---

## Deployment

### Render (Production)

**Configuration (render.yaml):**
```yaml
services:
  - type: web
    name: ppt-maker-server
    env: python
    buildCommand: pip install -r requirements.txt
    startCommand: uvicorn main:app --host 0.0.0.0 --port $PORT
```

**Environment Variables to Set:**
- `AI_SERVICE`
- `OPENAI_API_KEY` or `ANTHROPIC_API_KEY`
- `ADMIN_PASSWORD`

---

## Future Enhancement Ideas

### Potential New Features
- Image slides (with DALL-E integration)
- Chart/graph slides
- Table slides
- Timeline slides
- Speaker notes generation
- Cloud sync
- Multi-language support

### Technical Improvements
- Custom fonts support
- Animation/transition support
- Collaboration features
- Export to additional formats

---

## License

Copyright 2024-2025 KimBytes (com.kimbytes.pptmaker)
