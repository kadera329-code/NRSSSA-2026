# Automatic Volleyball Standings Generator

This project provides an automatic standings table generator for volleyball tournaments with per-set result tracking.

## Features

✅ **Per-Set Result Tracking** - Record individual set scores for each match  
✅ **Automatic Standings Calculation** - Standings update automatically when matches are added  
✅ **Detailed Statistics** - Track sets for/against and points for/against  
✅ **Web Interface** - User-friendly web app to manage results and view standings  
✅ **JSON Data Storage** - Results stored as JSON for easy export and analysis  
✅ **Real-time Updates** - Standings refresh automatically  

## Standings Table Includes

| Column | Description |
|--------|-------------|
| Position | Team ranking |
| Team | Team name |
| P | Matches played |
| W | Matches won |
| L | Matches lost |
| SF | Sets for (won) |
| SA | Sets against (lost) |
| PF | Points for (total points scored) |
| PA | Points against (total points conceded) |
| Ratio | Points for / Points against ratio |

## Setup

### 1. Clone and install dependencies
```bash
git clone https://github.com/kadera329-code/NRSSSA-2026.git
cd NRSSSA-2026
pip install -r requirements.txt
```

### 2. Run the web application
```bash
python app.py
```

Open your browser and go to `http://localhost:5000`

## Usage

### Adding Match Results

1. Navigate to the pool you want to update
2. Fill in the match details:
   - Match number (1-6)
   - Team 1 and Team 2
   - Date and time (optional)
   - Venue (optional)
3. Enter the set results (points per set)
4. Click "Save Match Result"

### Example Match Entry

**Match 1: Tom Mboya vs Udira Kamrembo**
- Set 1: 25-20 (Tom Mboya wins)
- Set 2: 25-18 (Tom Mboya wins)
- Result: Tom Mboya wins 2-0

### Using the Python Script

```python
from scripts.standings_generator import VolleyballStandingsGenerator

# Create a generator for Boys Pool A
pool = VolleyballStandingsGenerator(
    "Boys Pool A",
    ["Tom Mboya", "Masara", "Gesiaga", "Udira Kamrembo"]
)

# Add match results (per set)
pool.add_match(
    1,  # Match number
    "Tom Mboya",  # Team 1
    "Udira Kamrembo",  # Team 2
    [25, 25],  # Team 1 scores per set
    [20, 18]   # Team 2 scores per set
)

# Generate standings markdown
standings = pool.generate_standings()
print(standings)

# Get detailed match report
matches = pool.get_detailed_matches()
print(matches)
```

## Data Structure

Results are stored in JSON format:

```json
{
  "match": 1,
  "team1": "Tom Mboya",
  "team2": "Udira Kamrembo",
  "team1_sets": [25, 25],
  "team2_sets": [20, 18],
  "date": "2026-06-28",
  "time": "14:00",
  "venue": "Main Gym"
}
```

## API Endpoints

### Get Standings
```
GET /api/pool/{pool_id}/standings
```

Returns array of standings with team statistics.

### Get Results
```
GET /api/pool/{pool_id}/results
```

Returns array of all match results for the pool.

### Add Match Result
```
POST /api/pool/{pool_id}/match
```

Request body:
```json
{
  "match_num": 1,
  "team1": "Tom Mboya",
  "team2": "Udira Kamrembo",
  "team1_sets": [25, 25],
  "team2_sets": [20, 18],
  "date": "2026-06-28",
  "time": "14:00",
  "venue": "Main Gym"
}
```

## Pool IDs

- `boys_a` - Boys Pool A
- `boys_b` - Boys Pool B
- `girls_a` - Girls Pool A
- `girls_b` - Girls Pool B

## Tournament Structure

Each pool has 4 teams with 6 matches following this schedule:
1. 1 vs 4
2. 2 vs 3
3. 4 vs 3
4. 1 vs 2
5. 2 vs 4
6. 3 vs 1

## File Structure

```
NRSSSA-2026/
├── app.py                 # Flask web application
├── requirements.txt       # Python dependencies
├── scripts/
│   └── standings_generator.py  # Standalone standings generator
├── templates/
│   ├── index.html        # Home page
│   └── pool.html         # Pool detail page
├── results/              # Match results (JSON files)
└── volleyball/
    ├── boys/
    │   ├── pool-a.md
    │   └── pool-b.md
    ├── girls/
    │   ├── pool-a.md
    │   └── pool-b.md
```

## Statistics Calculation

### Sets For/Against
Sets are counted by comparing set scores. A team wins a set if their score is higher than opponent's score.

### Points For/Against
Total of all points scored across all sets played.

### Win/Loss
A team wins a match if they win more sets than their opponent (best of 3 sets).

## Example Standing Table

| Pos | Team | P | W | L | SF | SA | PF | PA | Ratio |
|-----|------|---|---|---|----|----|-----|-----|-------|
| 1 | Tom Mboya | 1 | 1 | 0 | 2 | 0 | 50 | 38 | 1.32 |
| 2 | Masara | 1 | 0 | 1 | 1 | 2 | 47 | 50 | 0.94 |
| 3 | Gesiaga | 1 | 0 | 1 | 0 | 2 | 38 | 50 | 0.76 |
| 4 | Udira Kamrembo | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0.00 |

## Contributing

To contribute improvements or fixes, please create a pull request.

## License

Open source - feel free to use and modify for your tournament needs.
