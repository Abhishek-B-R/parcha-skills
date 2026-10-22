# Setup

The Tether runtime and installers are distributed privately. Installing this skill
only installs agent instructions. Ask the system owner for the approved private
runtime and setup procedure; do not install historical public packages or source.

After the owner provisions the runtime, verify it from the intended agent session:

```bash
tether version
tether doctor
```

Use the existing broker and its configured operator permissions. Keep Slack
credentials in the broker; never load them into the coding session. If readiness
checks fail, report their sanitized result to the owner. Do not start a second
relay or replace the failed operation with a direct Slack API call.
