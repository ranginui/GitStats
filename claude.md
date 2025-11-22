# GitStats - Project Guide for Claude

## Project Overview
GitStats is a statistics generator for git repositories, designed to help developers analyze development patterns and project metrics. It generates HTML reports with tables and graphs showing comprehensive repository statistics.

## Purpose
GitStats analyzes git repositories and produces visual statistics including:
- General statistics (total files, lines, commits, authors)
- Activity metrics (commits by time, day, week, month, year)
- Author statistics (contribution rankings, timelines, age of involvement)
- File statistics (count by date, extensions)
- Lines of Code metrics over time

## Architecture

### Main Components

#### `git-stats` (Main Executable)
- Python 2.x script (Python 3 not supported)
- Entry point for the application
- Contains the core data collection and HTML generation logic
- Key classes:
  - `DataCollector`: Manages data extraction from git repositories
  - Uses caching mechanism (pickle + zlib compression) for performance

#### Supporting Files
- `gitstats.css`: Stylesheet for HTML output
- `sortable.js`: JavaScript for interactive table sorting
- `arrow-*.gif`: UI elements for sorted tables
- `Makefile`: Build/install instructions
- `doc/`: Documentation directory

### Technology Stack
- **Language**: Python 3 (>= 3.6 recommended)
- **VCS**: Git (>= 1.5.2.4)
- **Graphing**: Gnuplot (>= 4.0.0)
- **Output**: HTML/CSS/JavaScript

## Key Functionality

### Data Collection
- Executes git commands via `subprocess` to extract repository data
- Implements caching to avoid re-processing unchanged data
- Uses `getpipeoutput()` function to chain git commands

### Configuration
The `conf` dictionary contains customizable settings:
- `max_domains`: Maximum domains to display
- `max_ext_length`: Maximum extension length
- `style`: CSS stylesheet filename
- `max_authors`: Maximum number of authors to display

### Image Generation
- Uses Gnuplot to create statistical graphs
- Supports SVG (default) and PNG formats
- Image type controlled by `IMAGE_TYPE` variable

### HTML Output Features
- **Responsive Design**: Mobile-friendly layout with flexible tables and navigation
- **Dark Mode**: Toggle between light and dark themes with localStorage persistence
- **Modern CSS**: Uses CSS variables for easy theming and customization
- **Accessibility**: Supports reduced motion preferences and high contrast mode
- **Interactive Tables**: Sortable columns using sortable.js
- **Progressive Enhancement**: Works with and without JavaScript enabled

## Working with the Code

### Running GitStats
```bash
./git-stats <git-repo-path> <output-path>
```

### Platform Considerations
- Primarily designed for Linux (`ON_LINUX` flag)
- Windows 7 support available with PATH configuration
- Different terminal output behavior based on platform

### Code Conventions
- Python 3 syntax (print functions, unicode strings by default)
- Global variables for execution time tracking
- Heavy use of subprocess for git command execution
- Cache files use pickle with zlib compression
- Bytes/string handling with UTF-8 encoding for subprocess output

## Important Notes

### Limitations
- Requires Python 3 (Python 2 no longer supported)
- Requires git repository (bare clones work)
- Memory-intensive for large repositories
- Assumes local file structure (CSS/JS files in same directory)

### Performance
- Uses caching to improve subsequent runs
- Tracks execution time (internal vs external commands)
- Can be slow on large repositories

### Security Considerations
- Uses `shell=True` in subprocess calls - be cautious with untrusted input
- No input sanitization on repository paths
- Executes arbitrary git commands

## Development Guidelines

When modifying this codebase:
1. Maintain Python 3 compatibility (3.6+)
2. Test with various repository sizes
3. Ensure cache invalidation works correctly
4. Validate HTML output in multiple browsers
5. Test graph generation with different gnuplot versions
6. Be mindful of memory usage on large repos
7. Handle subprocess output as bytes and decode properly
8. Use context managers (with statements) for file operations

## File Structure
```
GitStats/
├── git-stats           # Main executable script
├── gitstats.css        # HTML report styling
├── sortable.js         # Table sorting functionality
├── arrow-*.gif         # UI assets
├── Makefile           # Build configuration
├── doc/               # Documentation
└── README.markdown    # User documentation
```

## Version Information
- Uses git short hash as version identifier
- Retrieved via `git rev-parse --short HEAD`
- Cached in VERSION global variable
