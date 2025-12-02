# PPT Maker

AI-powered PowerPoint presentation generator with iOS app and Python backend.

## Project Overview

PPT Maker allows users to create professional PowerPoint presentations in seconds using AI. The app features a two-step workflow that gives users full control over their content while leveraging AI for creative generation.

### Key Features

- **Two-Step Workflow**: Generate AI outline → Edit content → Create presentation
- **15 Professional Templates**: Corporate, Creative, Academic, Minimal, Warm, Tech, Nature, Luxury, Vibrant, Monochrome, Sunset, Ocean, Dark, Pastel, Retro
- **Presentation History**: Browse, preview, and manage saved presentations
- **In-App Preview**: QuickLook integration for viewing presentations before sharing
- **Haptic Feedback**: Tactile response throughout the app for better UX
- **Onboarding Tutorial**: 4-page introduction for first-time users
- **Enhanced AI Prompts**: Creative, action-oriented presentation content

## Architecture

### Backend (Python/FastAPI)
- **Repository**: [PPTMaker-Server](https://github.com/kbartiquel/PPTMaker-Server)
- **Tech Stack**: FastAPI, python-pptx, OpenAI API
- **API Endpoints**:
  - `POST /generate-outline` - Generate editable presentation outline
  - `POST /generate-presentation` - Create .pptx from edited outline
  - `GET /templates` - List available templates
  - `GET /health` - Health check

### iOS App (Swift/SwiftUI)
- **Repository**: [PPTMaker-IosApp](https://github.com/kbartiquel/PPTMaker-IosApp)
- **Tech Stack**: SwiftUI, URLSession, QuickLook
- **Requirements**: iOS 16.0+, Xcode 15.0+

## Project Structure

```
PPTMaker/
├── backend/                    # Python FastAPI server
│   ├── templates.py           # 15 template definitions
│   ├── ai_service.py          # OpenAI integration
│   ├── presentation_generator.py  # PowerPoint creation
│   ├── main.py                # FastAPI application
│   └── requirements.txt
│
└── PPTMaker/                  # iOS SwiftUI app
    └── PPTMaker/
        ├── Models/            # Data models
        │   ├── Template.swift
        │   └── SlideModels.swift
        ├── Services/          # Business logic
        │   ├── APIService.swift
        │   ├── FileService.swift
        │   └── HapticManager.swift
        ├── ViewModels/        # MVVM layer
        │   └── PresentationViewModel.swift
        └── Views/             # SwiftUI views
            ├── ContentView.swift
            ├── OutlineEditorView.swift
            ├── TemplateSelectionView.swift
            ├── PresentationHistoryView.swift
            └── OnboardingView.swift
```

## Development Setup

### Backend Setup

```bash
cd backend
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
pip install -r requirements.txt

# Create .env file with your OpenAI API key
echo "OPENAI_API_KEY=your_key_here" > .env

# Run the server
python main.py
# Server runs on http://localhost:8000
```

### iOS App Setup

1. Open `PPTMaker.xcodeproj` in Xcode
2. Update `APIService.swift` with your backend URL (default: http://localhost:8000)
3. Configure Info.plist settings in Xcode:
   - UIFileSharingEnabled: YES
   - LSSupportsOpeningDocumentsInPlace: YES
   - NSAppTransportSecurity > NSAllowsLocalNetworking: YES
4. Build and run (⌘R)

## Key Design Decisions

### Two-Step Workflow
Originally planned as a single-step generation, we evolved to a two-step process to give users control over their content before final generation. This allows for editing slide titles, bullet points, and structure.

### AI Model Selection
Using `gpt-4o-mini` instead of `gpt-4` for cost efficiency and API access compatibility. The enhanced prompts ensure creative, engaging content despite the smaller model.

### 15 Templates
Expanded from the original 5 templates to cover diverse use cases:
- Business: Corporate, Professional Dark, Monochrome
- Creative: Creative Bold, Vibrant Energy, Retro Vintage
- Academic: Academic Classic
- Modern: Minimal Modern, Tech Startup
- Nature: Nature Eco, Ocean Blue
- Premium: Luxury Premium
- Warm: Warm & Friendly, Sunset Glow, Pastel Soft

### Template Color Matching
Both backend (RGB tuples) and iOS (SwiftUI Colors) use exact color matching to ensure WYSIWYG template previews.

## Deployment

### Backend Deployment (Planned: Render)
The backend is designed to run on Render or similar platforms. Environment variables required:
- `OPENAI_API_KEY`: Your OpenAI API key
- Optional: `ANTHROPIC_API_KEY` for Claude integration

### iOS App Distribution
- Bundle ID: `com.kimbytes.pptmaker`
- App Name: PPT Maker
- Distribution: TestFlight → App Store

## API Usage Example

### Step 1: Generate Outline
```bash
curl -X POST http://localhost:8000/generate-outline \
  -H "Content-Type: application/json" \
  -d '{
    "topic": "Climate Change Solutions",
    "num_slides": 8
  }'
```

### Step 2: Generate Presentation
```bash
curl -X POST http://localhost:8000/generate-presentation \
  -H "Content-Type: application/json" \
  -d '{
    "presentation_title": "Climate Change Solutions",
    "template": "nature",
    "slides": [...]
  }' \
  --output presentation.pptx
```

## Development History

- Initial backend with 5 templates
- Fixed OpenAI API compatibility issues (model access, library version)
- Enhanced AI prompts for creative content
- Expanded to 15 templates
- iOS app development with MVVM architecture
- Added presentation history with QuickLook preview
- Integrated haptic feedback throughout
- Created non-skippable onboarding tutorial
- Implemented in-app PPT preview

## License

Copyright © 2025 kimbytes. All rights reserved.

## Contact

For issues or questions, please open an issue in the respective repositories.
