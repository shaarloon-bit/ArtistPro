# Design Guidelines - Artist Pro (Aplicativo de Shows ao Vivo)

## Design Approach
**Utility-Focused Application** - This is a professional musician's tool requiring exceptional readability, quick navigation during live performances, and clear visual states. Design prioritizes function and legibility above all else.

## Color System

### Dark Mode (Primary Theme)
- Background: Deep purple-dark (#1a0b2e, #0f0520)
- Accents: Deep blue (#1e3a8a, #1e40af)
- Primary actions: Vibrant purple (#8b5cf6, #a78bfa)
- Success/Unplayed: Bright green (#10b981, #34d399)
- Warning/Played: Pink/Rose (#ec4899, #f43f5e)

### Light Mode
- Background: Light gray (#f3f4f6, #e5e7eb)
- Accents: Soft blue (#60a5fa, #3b82f6)
- Container: White with subtle purple tint
- Text: Dark gray (#1f2937)

### Teleprompter Specific
- Background: Pure black (#000000)
- Lyrics: Pure white (#ffffff) in bold
- Chords: Soft pink (#ffc0cb, #ffb3d9)
- Navigation hints: Subtle gray (#4b5563)

## Typography System

### Main Application
- Headlines: 2xl to 4xl (32-48px), semi-bold to bold
- Song titles: xl to 2xl (24-32px), medium weight
- Body text: base to lg (16-18px), regular
- Metadata: sm to base (14-16px), regular

### Teleprompter Mode
- Lyrics: 4xl to 6xl (48-72px), **bold weight** - maximum readability
- Chords: 2xl to 3xl (28-36px), medium weight, pink color
- Song navigation: lg (18px), regular weight
- Spacing: Generous line-height (1.8-2.0) for clarity

Use system fonts or Google Fonts with excellent legibility: Inter, Roboto, or Noto Sans.

## Layout System

### Spacing Scale
Use Tailwind units: **2, 3, 4, 6, 8, 12, 16** for consistent spacing
- Tight spacing: p-2, m-3 (cards, buttons)
- Standard spacing: p-4, gap-6 (lists, forms)
- Section spacing: py-8, py-12 (page sections)
- Large spacing: py-16 (teleprompter margins)

### Main Application Layout
- Container: max-w-6xl centered
- Song list: Single column with generous padding (p-6)
- Forms: max-w-2xl, ample field spacing (space-y-4)

### Teleprompter Layout
- Full viewport (100vh, 100vw)
- Centered content area: max-w-5xl
- Lyrics: px-12 for reading comfort
- Navigation: Fixed bottom positioning with backdrop blur

## Component Library

### Song Cards (Main List)
- Card design with rounded corners (rounded-lg)
- Clear visual states:
  - **Unplayed**: Left border accent in green (border-l-4 border-green-500)
  - **Played**: Left border and background tint in pink (border-l-4 border-pink-500, bg-pink-50 dark mode)
- Content hierarchy: Song title (bold, large) > Artist (medium) > Metadata (small, muted)
- Hover state: Subtle elevation (shadow-md)
- Click action: Opens in teleprompter

### Action Buttons
- Primary CTA ("COMEÇAR O SHOW"): Large (py-4 px-8), vibrant purple, bold text, rounded-lg
- Secondary actions: Medium size (py-2 px-4), outlined or ghost style
- Icon buttons: Square (w-10 h-10), rounded-full, hover background
- Teleprompter controls: Semi-transparent with backdrop blur, rounded-full

### Forms (Add Song)
- Label above input, bold, mb-2
- Inputs: Large touch targets (py-3 px-4), rounded-md, clear focus states
- Textarea for lyrics: Monospace font hint, min-h-96 for full songs
- Helper text: Small, muted color below inputs
- Submit: Prominent purple button

### Teleprompter Components
- **Lyrics Display**: 
  - Auto-detect chord lines (contains chord patterns from provided list)
  - Chord lines: Pink color, positioned above lyrics
  - Lyric lines: White bold text, clear spacing below chords
  - Verse separation: Extra spacing (my-6)
  
- **Navigation Footer**:
  - Fixed bottom, full width
  - Backdrop blur (backdrop-blur-md)
  - Left corner: "◀ Previous: [Song Name]" in subtle gray
  - Center: Progress indicator "3/15 Songs"
  - Right corner: "Next: [Song Name] ▶" in subtle gray

### Status Indicators
- Song counter badge: Circle or pill shape, purple background
- Progress bar: Linear indicator showing songs played/total
- Played/unplayed dots: Color-coded indicators in list view

## Chord Detection System
Parse each line for presence of chords from the comprehensive list provided (C, Am, G7, CM7, Dsus4, etc.). Any line containing 2+ recognized chords is styled as chord line (pink), with the following line as lyrics (white bold).

## Navigation & Interaction

### Keyboard Controls
- Arrow Left (←): Previous song
- Arrow Right (→): Next song
- Spacebar: Mark as played/toggle
- Escape (ESC): Exit teleprompter
- Display keyboard shortcuts hint on first teleprompter load

### Touch/Click Interactions
- Swipe left/right on teleprompter for navigation (mobile)
- Tap zones: Left 30% = previous, Right 30% = next, Center = controls
- Long-press song card for quick actions

## Pre-loaded Content
Initialize app with metadata for 80 songs provided (Sina - Djavan, Yellow - Coldplay, etc.). Display as empty placeholders with "Add lyrics" prompt. User fills lyrics manually from legal sources.

## Accessibility
- High contrast ratios (WCAG AAA for teleprompter)
- Large touch targets (minimum 44x44px)
- Clear focus indicators
- Keyboard navigation throughout

## Performance Optimizations
- Local storage for all song data
- Minimal animations (only smooth transitions)
- Optimized font loading
- Lazy load song lyrics in teleprompter mode

## Images
No hero images needed. This is a utility application focused on text display and management. Any imagery should be subtle icons or illustrations supporting the music theme (musical notes, stage lights as decorative accents only).