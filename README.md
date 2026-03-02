# macOS Calendar Skill for OpenClaw

[![OpenClaw Skill](https://img.shields.io/badge/OpenClaw-Skill-blue)](https://openclaw.ai)
[![macOS Only](https://img.shields.io/badge/platform-macOS-lightgrey)](https://www.apple.com/macos/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

An OpenClaw skill that lets you access, create, update, and manage events in MacOS Calendar.app using natural language.
This skill enables your AI assistant to interact with your calendar - checking events, creating appointments, and managing your schedule.

## Features

- 📅 **List all calendars** - View available calendars on your Mac
- 👁️ **View today's events** - See what's scheduled for today
- ➕ **Create new appointments** - Add events to your calendar
- 🔍 **Check upcoming events** - See what's coming up
- 🗑️ **Delete events** - Remove events (with caution)
- ⏰ **Meeting reminders** - Check for upcoming meetings
- 📊 **Weekly overview** - Get a summary of your week


## Installation

### Prerequisites

Before installing this skill, ensure you have:

1. **OpenClaw installed** - If you haven't installed OpenClaw yet, visit [openclaw.ai](https://openclaw.ai) for installation instructions
2. **macOS operating system** - This skill only works on macOS
3. **Calendar app access** - Grant terminal access to Calendar in System Settings > Privacy & Security > Automation

### Installing in OpenClaw

There are several ways to install this skill in your OpenClaw workspace:

#### Method 1: Using ClawHub (Recommended)

If you have the `clawhub` CLI installed:

```bash
# Search for the skill
clawhub search macos-calendar

# Install the skill
clawhub install macos-calendar
```

#### Method 2: Manual Installation

1. **Clone or download** the skill files to your OpenClaw skills directory:

```bash
# Navigate to your OpenClaw skills directory
cd ~/.openclaw/workspace/skills/

# Clone the repository (replace with actual GitHub URL)
git clone https://github.com/dlooto/macos-calendar.git
```

2. **Verify the skill structure**:
   - Ensure `SKILL.md` exists in the skill folder
   - Check that all required files are present

3. **Restart OpenClaw** (if needed):
```bash
openclaw gateway restart
```

#### Method 3: Direct Copy

If you're already viewing this repository, you can copy the skill folder directly:

```bash
# Copy the entire skill folder to your OpenClaw workspace
cp -r macos-calendar ~/.openclaw/workspace/skills/
```

### Verification

After installation, verify the skill is available:

```bash
# Check if skill appears in your OpenClaw skills list
openclaw skills list | grep -i calendar

# Or check the skills directory
ls -la ~/.openclaw/workspace/skills/macos-calendar/
```

### Configuration

No additional configuration is required. The skill will automatically:
- Appear in your available skills list
- Be triggered when you mention "calendar", "events", "appointments", etc.
- Use the built-in `osascript` command to interact with macOS Calendar

### Permissions Setup

**Important**: You must grant terminal access to Calendar:

1. Open **System Settings** on your Mac
2. Go to **Privacy & Security** > **Automation**
3. Find your terminal application (Terminal, iTerm, etc.)
4. Enable **Calendar** access

### Troubleshooting Installation

If the skill doesn't work after installation:

1. **Check permissions**:
   ```bash
   # Test Calendar access
   osascript -e 'tell application "Calendar" to get name of calendars'
   ```

2. **Verify skill location**:
   ```bash
   # Ensure skill is in the correct directory
   ls -la ~/.openclaw/workspace/skills/macos-calendar/SKILL.md
   ```

3. **Check OpenClaw logs**:
   ```bash
   # View OpenClaw logs for skill loading errors
   openclaw logs --tail
   ```

4. **Restart OpenClaw**:
   ```bash
   openclaw gateway restart
   ```

### Updating the Skill

To update to the latest version:

```bash
# Using ClawHub
clawhub update macos-calendar

# Or manually update from GitHub
cd ~/.openclaw/workspace/skills/macos-calendar
git pull origin master
```

### Uninstallation

To remove the skill:

```bash
# Remove the skill folder
rm -rf ~/.openclaw/workspace/skills/macos-calendar

# Restart OpenClaw
openclaw gateway restart
```

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

## Contributing

We welcome contributions to improve this skill! Here's how you can help:

### Reporting Issues
If you find a bug or have a feature request:
1. Check if the issue already exists in the [GitHub Issues](https://github.com/yourusername/macos-calendar/issues)
2. If not, create a new issue with:
   - A clear title and description
   - Steps to reproduce the issue
   - Expected vs actual behavior
   - Your macOS version and OpenClaw version

### Submitting Changes
1. **Fork the repository**
2. **Create a feature branch**:
   ```bash
   git checkout -b feature/amazing-feature
   ```
3. **Make your changes** and test them thoroughly
4. **Commit your changes**:
   ```bash
   git commit -m "Add amazing feature"
   ```
5. **Push to your fork**:
   ```bash
   git push origin feature/amazing-feature
   ```
6. **Open a Pull Request**

### Development Guidelines
- Follow the existing code style and structure
- Update documentation (README.md, SKILL.md) for any new features
- Add tests if possible
- Ensure backward compatibility

### Areas for Improvement
- Add support for more calendar operations
- Improve error handling and user feedback
- Add configuration options
- Create more example workflows
- Add integration with other calendar services

## Support

If you need help:
1. **Check the documentation** - This README and SKILL.md
2. **Search existing issues** - Your question might already be answered
3. **Ask in the OpenClaw community** - [Discord](https://discord.com/invite/clawd) or [GitHub Discussions](https://github.com/openclaw/openclaw/discussions)

## Related Projects

- [OpenClaw](https://openclaw.ai) - The AI assistant platform
- [ClawHub](https://clawhub.com) - Skill marketplace for OpenClaw
- [Other macOS Skills](https://clawhub.com/search?q=macos) - More skills for macOS automation

## License

MIT License - See [LICENSE](LICENSE) file for details.

Copyright (c) 2026 JonShon
