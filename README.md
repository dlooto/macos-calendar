# macOS Calendar Skill

Access and manage macOS Calendar using AppleScript.

## Features

- List all calendars
- View today's events
- Create new appointments
- Check upcoming events
- Delete events (with caution)

## Installation

This skill is already included in your OpenClaw workspace. No additional installation required.

## Usage

### Basic Commands

```bash
# List all calendars
osascript -e 'tell application "Calendar" to get name of calendars'

# View today's events
osascript <<'EOF'
tell application "Calendar"
    set todayEvents to every event where start date > (current date) and start date < (current date) + 1 * days
    repeat with ev in todayEvents
        log time string of (start date of ev) & ": " & summary of ev
    end repeat
end tell
EOF
```

### Common Workflows

**Morning Check:**
```bash
echo "📅 Today's Schedule:"
osascript -e 'tell application "Calendar" to get summary of events where start date > (current date) and start date < (current date) + 1 * days'
```

**Add Quick Event:**
```bash
osascript -e 'tell application "Calendar" to make new event at calendar "Home" with properties {summary:"Quick Meeting", start date:(current date) + 30 * minutes, end date:(current date) + 1 * hours}'
```

## Security

This skill accesses your personal calendar data. Ensure:
1. Terminal has Calendar access in System Settings
2. You understand what data is being accessed
3. You review scripts before running

## Troubleshooting

### Permission Issues
1. Open System Settings
2. Go to Privacy & Security > Automation
3. Ensure your terminal app has Calendar access

### Calendar Not Found
```bash
# Check available calendars
osascript -e 'tell application "Calendar" to get name of calendars'
```

## License

MIT License - See LICENSE file for details.