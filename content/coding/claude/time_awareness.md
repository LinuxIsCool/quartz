---
title: Time Awareness
---

### .claude/hooks/timestamp-server.sh

```bash
#!/bin/bash

# Claude Code Timestamp Server Hook
# Purpose: Serve the current timestamp context into every user prompt
# Author: Claude Learning Environment
# Date: 2025-08-31

# Get current timestamp in multiple formats
CURRENT_DATE=$(date '+%Y-%m-%d')
CURRENT_TIME=$(date '+%H:%M:%S')
CURRENT_DATETIME=$(date '+%Y-%m-%d %H:%M:%S')
TIMEZONE=$(date '+%Z')
TIMEZONE_OFFSET=$(date '+%z')
DAY_OF_WEEK=$(date '+%A')
DAY_OF_YEAR=$(date '+%j')
WEEK_OF_YEAR=$(date '+%V')
UNIX_TIMESTAMP=$(date '+%s')
ISO_8601=$(date --iso-8601=seconds)
HUMAN_TIME=$(date '+%B %d, %Y at %I:%M %p %Z')

# Build temporal context
TEMPORAL_CONTEXT="[Temporal Context]
- Current: $CURRENT_DATETIME $TIMEZONE ($DAY_OF_WEEK)
- Human: $HUMAN_TIME
- ISO 8601: $ISO_8601
- Unix: $UNIX_TIMESTAMP
- Day: $DAY_OF_YEAR of 365 (Week $WEEK_OF_YEAR)
- Timezone: $TIMEZONE ($TIMEZONE_OFFSET)"

# Output the temporal context
# For UserPromptSubmit hooks, stdout with exit code 0 is injected as context
echo "$TEMPORAL_CONTEXT"

# Exit with 0 to inject the context
exit 0
```


### .claude/settings.json

```json

{
  "hooks": {
    "UserPromptSubmit": [
      {
        "matcher": "*",
        "hooks": [
          {
            "type": "command",
            "command": ".claude/hooks/timestamp-server.sh"
          },
          ]
       }
    ]
  }
}
```
