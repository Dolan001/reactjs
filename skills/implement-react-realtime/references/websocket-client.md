# React WebSocket client

- Validate every frame as a versioned discriminated union before changing state. Unknown events are
  observable and ignored safely.
- Authenticate with secure cookies or a short-lived single-use ticket fetched over HTTPS; never put
  a reusable access token in the URL, logs, analytics, or error UI.
- Reconnect with exponential backoff, jitter, caps, online/visibility awareness, and one connection
  per authenticated application scope. Pause on auth failure until refresh succeeds.
- Give each app-wide or route subscriber its own connection ownership handle. Route unmount must
  release only that handle; it must not stop the shared socket while notification, conversation,
  or other subscribers still own it. Make remount cleanup cancelable, and close exactly once when
  the last owner leaves or the authenticated session ends. Do not notify transport-failure
  recovery listeners for an intentional close.
- Track the last applied cursor/sequence, detect gaps, fetch cursor-based HTTP resync, deduplicate by
  event ID, and merge optimistic commands by `client_command_id`.
- Bound outbound queues and pending commands. Expose explicit connecting, live, degraded, offline,
  resyncing, and failed states; never report “sent” before server acknowledgement.
- Keep typing/presence ephemeral. Announce new messages accessibly without stealing focus; respect
  reduced motion and notification permission. Clean timers/listeners on logout and unmount.
- Test two simultaneous subscribers, route teardown/remount, last-owner logout cleanup, reconnect
  capacity, duplicate/out-of-order events, missed-event resync, token expiry, offline send,
  multiple tabs, malformed frames, slow server, accessibility, and REST fallback.
