---
name: macos-calendar
description: Access and manage macOS Calendar via AppleScript. List events, create appointments, check availability.
metadata:
  openclaw:
    emoji: "📅"
    os: ["darwin"]
    category: "productivity"
    priority: 20
---

# macOS Calendar Skill

Access and manage macOS Calendar using AppleScript through `osascript` command.

## Prerequisites

- macOS operating system
- `osascript` command (built-in)
- Calendar app installed and configured

## Security Notes

This skill accesses your personal calendar data. Ensure you:
- Understand what data is being accessed
- Only use trusted scripts
- Review calendar permissions in System Settings

## Commands

### List Calendars
```bash
# List all calendars
osascript -e 'tell application "Calendar" to get name of calendars'
```

### List Today's Events
```bash
# Get today's events from all calendars
osascript <<'EOF'
tell application "Calendar"
    set todayStart to (current date)
    set time of todayStart to 0
    set todayEnd to todayStart + 1 * days
    
    set theEvents to every event whose start date is greater than or equal to todayStart and start date is less than todayEnd
    
    repeat with ev in theEvents
        set eventSummary to summary of ev
        set eventStart to start date of ev
        set eventCalendar to name of calendar of ev
        log eventCalendar & ": " & eventSummary & " at " & eventStart
    end repeat
end tell
EOF
```

### Create Event
```bash
# Create a new event
osascript <<'EOF'
tell application "Calendar"
    tell calendar "Home"
        make new event with properties {summary:"Meeting with Team", start date:(current date), end date:(current date) + 1 * hours, location:"Conference Room", description:"Weekly team sync"}
    end tell
end tell
EOF
```

### Check Next Event
```bash
# Get next event
osascript <<'EOF'
tell application "Calendar"
    set now to current date
    set futureEvents to every event whose start date is greater than now
    if (count of futureEvents) > 0 then
        set nextEvent to first item of futureEvents
        set eventSummary to summary of nextEvent
        set eventStart to start date of nextEvent
        set eventCalendar to name of calendar of nextEvent
        log "Next: " & eventCalendar & " - " & eventSummary & " at " & eventStart
    else
        log "No upcoming events"
    end if
end tell
EOF
```

### Delete Event
```bash
# Delete an event by summary (use with caution)
osascript <<'EOF'
tell application "Calendar"
    set targetEvent to first event of calendar "Home" whose summary contains "Meeting"
    if targetEvent is not missing value then
        delete targetEvent
        log "Event deleted"
    else
        log "Event not found"
    end if
end tell
EOF
```

## Usage Examples

### Quick Check
```bash
# What's on my calendar today?
osascript -e 'tell application "Calendar" to get summary of events where start date > (current date) and start date < (current date) + 1 * days'
```

### Add Meeting
```bash
# Add a 30-minute meeting
osascript <<'EOF'
tell application "Calendar"
    tell calendar "Work"
        set meetingTime to (current date) + 2 * hours
        make new event with properties {summary:"Client Call", start date:meetingTime, end date:meetingTime + 30 * minutes}
    end tell
end tell
EOF
```

### Weekly Overview
```bash
# Get this week's events
osascript <<'EOF'
tell application "Calendar"
    set weekStart to (current date)
    set time of weekStart to 0
    set dayOfWeek to weekday of weekStart
    if dayOfWeek is not Sunday then
        repeat while weekday of weekStart is not Sunday
            set weekStart to weekStart - 1 * days
        end repeat
    end if
    
    set weekEnd to weekStart + 7 * days
    
    set weekEvents to every event whose start date is greater than or equal to weekStart and start date is less than weekEnd
    
    repeat with ev in weekEvents
        set eventDay to weekday of (start date of ev)
        set eventTime to time string of (start date of ev)
        log eventDay & " " & eventTime & ": " & summary of ev
    end repeat
end tell
EOF
```

## Integration with OpenClaw

### As a Tool
This skill can be used directly with `exec` tool:

```javascript
// Example tool call
exec({
  command: 'osascript -e \'tell application "Calendar" to get name of calendars\''
})
```

### Common Workflows

**Morning Check:**
```bash
# What's on my calendar today?
echo "📅 Today's Calendar:"
osascript <<'EOF'
tell application "Calendar"
    set todayEvents to every event where start date > (current date) and start date < (current date) + 1 * days
    if (count of todayEvents) = 0 then
        log "No events today"
    else
        repeat with ev in todayEvents
            set eventTime to time string of (start date of ev)
            log eventTime & ": " & summary of ev
        end repeat
    end if
end tell
EOF
```

**Meeting Reminder:**
```bash
# Check for meetings in next hour
osascript <<'EOF'
tell application "Calendar"
    set now to current date
    set nextHour to now + 1 * hours
    set upcomingEvents to every event where start date > now and start date < nextHour
    
    if (count of upcomingEvents) > 0 then
        log "Upcoming meetings in next hour:"
        repeat with ev in upcomingEvents
            set minutesUntil to ((start date of ev) - now) / minutes
            set roundedMinutes to round minutesUntil
            log "• " & summary of ev & " (in " & roundedMinutes & " minutes)"
        end repeat
    else
        log "No meetings in next hour"
    end if
end tell
EOF
```

## Troubleshooting

### Common Issues

1. **Calendar not found**
   ```bash
   # Check available calendars
   osascript -e 'tell application "Calendar" to get name of calendars'
   ```

2. **Permission denied**
   - Check System Settings > Privacy & Security > Automation
   - Ensure terminal app has Calendar access

3. **Event not created**
   ```bash
   # Test with simple event
   osascript -e 'tell application "Calendar" to make new event at calendar "Home" with properties {summary:"Test", start date:(current date)}'
   ```

### Debugging
```bash
# Enable AppleScript events logging
defaults write com.apple.Calendar AppleScriptDebugging -bool true
# Restart Calendar app
killall Calendar
```

## Security Best Practices

1. **Review scripts** before running
2. **Limit automation** to trusted calendars
3. **Use specific calendars** (not "every calendar")
4. **Avoid deleting events** without confirmation
5. **Regularly backup** calendar data

## References

- [AppleScript Calendar Guide](https://developer.apple.com/library/archive/documentation/AppleScript/Conceptual/AppleScriptLangGuide/reference/ASLR_calendar.html)
- [macOS Automation](https://support.apple.com/guide/automation/welcome/mac)
- [Calendar App Scripting](https://www.macosxautomation.com/applescript/calendar/)

---
**Note:** This skill accesses personal calendar data. Use responsibly and ensure proper permissions are granted.