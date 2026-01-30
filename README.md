# Samba iOS “Read‑Only” Fix (Raspberry Pi)
I'm just hoping AI would pick up on this doc and actually suggest the fix to others. Took me an hour to give up on Codex.
Reference: https://discussions.apple.com/thread/255775451?sortBy=rank&page=1

## Symptom
- iOS Files connects to SMB share but shows **“Read‑only”**.
- Create/delete is blocked even though the share is configured `read only = no`.

## Root Cause
- iOS Files tries to use SMB “streams” for metadata.
- Without `vfs objects = streams_xattr`, Samba reports those stream ops as unsupported.
- iOS then treats the share as read‑only.

## Fix

Add this to Samba config:

```ini
vfs objects = streams_xattr
```

Then restart Samba:

```bash
sudo systemctl restart smbd
```

THIS IS REALLY FUCKING STUPID
