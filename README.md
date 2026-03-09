# YouTube Channel Query & Filter

Query YouTube channels for videos and filter them by predefined keywords (science, medicine, AI, climate, space, technology, etc.).

## Features

- 🔍 Fetch videos from a YouTube channel
- 📊 Extract metadata (title, description, tags, view count, engagement stats)
- 🏷️ Filter videos by 29+ predefined keywords across 6 categories
- 📈 Safe API rate limiting (100-500 video limit per run)
- 💾 Export results to CSV with Title Case column names (Title, Matched Keyword, Category Name, View Count, Like Count, Tags)

## Setup

### 1. Get YouTube API Key

1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Create a new project
3. Enable **YouTube Data API v3** (APIs & Services → Library → search for YouTube)
4. Create an API Key (Credentials → Create Credentials → API Key)
5. Copy the API key

### 2. Configure Environment

```bash
# Copy the example file
cp .env.example .env

# Edit .env and add your API key
nano .env
```

```
YOUTUBE_API_KEY=your_api_key_here
```

### 3. Install Dependencies

Using pip:
```bash
pip install -r requirements.txt
```

Or using uv (if available):
```bash
uv sync
```

### 4. Configure Python Interpreter (PyCharm)

- Preferences → Python Interpreter → Add → Existing Environment
- Select `.venv/bin/python`

## Usage

Open `ytchq.ipynb` in Jupyter or PyCharm and:

1. **Update Channel ID** - Replace `UCxxxxxxxxxxxxxxxx` with your YouTube channel ID
2. **Set Video Limit** - Adjust `MAX_VIDEOS` (default 100, range 50-500 recommended)
3. **Run cells** - Execute cells in order (Setup → Get Videos → Extract Metadata → Filter)

### Example Output

```
Fetching up to 100 videos from channel UCdXXXXXXXXXXXXXXX...
✓ Found 98 videos
Extracting metadata...
  Processed 50/98 videos
  Processed 98/98 videos
✓ Extracted metadata for 98 videos
Matched 34 out of 98 videos
```

Results saved to `filtered_videos_YYYY-MM-DD_HHMMSS.csv` in the root directory

## Keywords Included

### Categories

- **AI & Machine Learning**: AI, artificial intelligence, machine learning
- **Medicine & Health**: medicine, medical, health, vaccine, vaccination, disease, pandemic, virus
- **Earth & Climate**: earth, climate, environment, global warming, climate change, renewable energy
- **Space & Astronomy**: space, NASA, astronomy
- **Science & Research**: science, research, study, scientist, biology, physics, chemistry
- **Technology & Innovation**: technology, tech, innovation

**Total: 29 keywords**

## API Rate Limiting

⚠️ **Important**: YouTube free tier allows **10,000 quota units/day**

- Each video query costs ~2 units (1 for playlist + 1 for metadata)
- 100 videos = ~200 units ✅ Safe
- 500 videos = ~1000 units ✅ Safe
- 5000+ videos = May exceed daily limit ⚠️

**Recommendation**: Keep `MAX_VIDEOS` at 100-500 per session

## File Structure

```
youtube-ch-query/
├── ytchq.ipynb           # Main notebook
├── .env                  # Your API key (gitignored)
├── .env.example          # Template
├── requirements.txt      # Python dependencies
├── pyproject.toml        # Project config
├── filtered_videos_*.csv # Output CSVs (generated with timestamp)
└── README.md            # This file
```

## Customizing Keywords

Edit the `FILTER_KEYWORDS` dictionary in the notebook:

```python
FILTER_KEYWORDS = {
    'Your Category': ['keyword1', 'keyword2', 'keyword3'],
    # ... more categories
}
```

## Troubleshooting

### "YOUTUBE_API_KEY not found in .env file"
- Verify `.env` file exists in project root
- Check API key is set correctly (no extra spaces)

### "Channel not found"
- Verify channel ID format (starts with `UC`)
- Channel must be public

### API quota exceeded
- Wait until next day (quota resets at midnight UTC)
- Reduce `MAX_VIDEOS` limit
- Consider paid API tier for higher limits

## Notes

- Free tier API key is read-only (no video uploads/modifications)
- Channel must be public to query
- Tags are optional; some videos may have no tags
- Processing time: ~1-2 seconds per 50 videos
