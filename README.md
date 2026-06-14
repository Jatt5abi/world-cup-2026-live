# World Cup 2026 Live Dashboard

Real-time World Cup 2026 tournament dashboard with live scores, group standings, team formations, and match results.

## Features

- **Live Scores**: Real-time match updates and live game display
- **Group Stage View**: All 12 groups (A-L) with 48 teams in sidebar
- **Interactive Team Selection**: Click any team to see:
  - Tactical formation on a soccer pitch
  - Player positions dynamically laid out
  - Recent match results
  - Group standings with points and W-D-L record
- **Tournament Stats**: Live match counts, goals, completed matches
- **Ticker Feed**: Real-time tournament updates scrolling ticker

## Tech Stack

- **Frontend**: HTML5, CSS3, Vanilla JavaScript
- **API**: Zafronix World Cup API (https://api.zafronix.com/fifa/worldcup/v1)
- **Hosting**: Vercel (or local HTTP server)
- **Data**: Real 2026 World Cup match data and standings

## Setup

### Local Development

```bash
cd ~/world-cup-2026-live
python3 -m http.server 8000
# Open http://localhost:8000
```

### Live Server

Deploy to Vercel or your hosting platform. Update API key in `index.html` line 306 if needed.

## API Configuration

The app uses Zafronix World Cup API. Token is in `index.html`:

```javascript
const API_KEY = 'zwc_free_4567732af16066e498ecb42b';
const API_BASE = 'https://api.zafronix.com/fifa/worldcup/v1';
```

**Endpoints:**
- `/tournaments/2026` — Tournament info and teams
- `/matches?year=2026` — All 2026 matches with live data

## Features Breakdown

### Live Match Display
- Shows currently playing match with large score
- Team flags and names
- Live minute counter
- Pulsing "LIVE" indicator

### Group Sidebar
- All 12 groups (A-L) organized vertically
- Click team to select and view details
- Team flags for quick identification
- Active team highlighting

### Pitch View
- SVG-style soccer field visualization
- Dynamic player positioning based on formation
- Common formations: 4-4-2, 4-3-3, 4-2-3-1, 4-1-4-1
- Player numbers 1-11 placed by position

### Results & Standings
- Recent completed matches with scores
- Team records (W-D-L)
- Points calculation (3 pts per win, 1 per draw)
- Goals for/against tracking
- Group-specific standings

### Ticker
- Scrolling match updates
- Goals, cards, tournament stats
- Real-time information feed

## Data Flow

1. **On Load**: Fetch tournament teams → organize by group
2. **Every 30s**: Poll matches API for updates
3. **On Team Click**: 
   - Fetch team's match history
   - Calculate standings from match data
   - Render formation based on team profile
4. **Live Match**: Auto-update every 5 seconds (simulated)

## Browser Compatibility

- Chrome/Edge (latest)
- Firefox (latest)
- Safari (latest)
- Mobile browsers (responsive design)

## Files

- `index.html` — Single-file app with HTML, CSS, JS
- `favicon.ico` — Soccer ball emoji favicon

## Customization

### Add Team Formations
Edit `formations` object in JavaScript:
```javascript
const formations = {
  'Team Name': { formation: '4-3-3', starters: [...] },
  // Add more teams
};
```

### Change Colors
Update CSS variables in `:root`:
```css
:root {
  --accent: #ff6b35;      /* Orange highlights */
  --accent-alt: #004e89;  /* Blue accents */
  --live: #ff1744;        /* Live indicator red */
  --gold: #ffd700;        /* Score gold */
}
```

### Update API
Replace `API_KEY` and `API_BASE` with your credentials if needed.

## Performance

- Single HTML file (no build step)
- ~14KB gzipped
- Lazy loads match data on demand
- 30-second polling interval (configurable)
- Caches team/group data in memory

## Known Limitations

- Polling-based updates (webhook alternative available)
- 4000 character limit on Telegram messages
- Formations are templates (real lineups from API would require squad data)
- Some teams may not have custom formations (uses default 4-4-2)

## Future Enhancements

- Historical matches and replays
- Player statistics and heat maps
- Penalty shootout tracking
- Knockout stage bracket visualization
- Push notifications for goals
- Dark/light theme toggle

## Session Notes (2026-06-13)

**What Was Built:**
1. Complete UI redesign from generic layout to professional sports dashboard
2. Groups sidebar showing all 48 teams organized by group A-L
3. Live scores window with large score display and team flags
4. Interactive pitch view with dynamic player formations
5. Past results and group standings panel
6. Fixed API endpoint (was /tournaments/2026, changed to /matches?year=2026)
7. Country flags for all 48 teams
8. Responsive layout with sidebar + main content

**Technical Changes:**
- Changed from deprecated meta tag to mobile-web-app-capable
- Added SVG pitch visualization with proper field markings
- Implemented dynamic formation positioning (4-4-2, 4-3-3, etc.)
- Team selection creates interactive clickable experience
- Real-time standings calculated from match data

**Current Live Match:**
- Brazil vs Morocco (from API live data)
- Proper team data fetching and display
- Live status updates every 30 seconds

**Commands for Phone/Telegram:**
- Test: Send any bash command via Telegram
- Example: `ls ~` to list home directory
- Example: `date` for current time
- Example: `git -C ~/world-cup-2026-live log --oneline` to check commits

## Support

For issues or questions, check the git log for recent changes or review this README.
