# Local fork (priard)

Branch `priard`, built on upstream `production` (Erijl/omarchy-fido2-key-suite,
remote `upstream`). Plugin id stays `erijl.lock`, so shell.json keeps working.

Upstream PRs merged as-is:

- #2 Reach presence over IPC, not the service object (bar glyph works again;
  settings fall back to `shell.barConfig`; drift message shows the real version)
- #1 Keep FIDO authentication out of suspend (settle window, resume restart,
  no presence-poll retry loop)
- #3 Inherit the vendor PAM stack instead of inventing one

Not merged: #4 -- the same `barConfig` fallback #2 already carries.

Own changes on top:

- `blankDelay` setting for the idle blank (upstream's 5 s by default), and the
  countdown restarts when a screen reconnects, so a panel that drops off the
  bus while blanked still shows the lock screen for the full delay.
- The display is held up only while a PIN is being verified, not for the whole
  FIDO2 conversation (#1 kept a locked screen lit forever with a PIN key).
- `enable` repairs the bare pam_unix polkit stack older versions wrote;
  `disable` removes an /etc copy that only shadows /usr/lib/pam.d.
- `doctor` no longer dies on a key with no PIN set ("pin retries: undefined").
- Management panel reads its presence toggles from `shell.barConfig`, so
  "When the key comes and goes" is visible again.
- `lockScreen` setting (auto/on/off, panel toggle): steps aside for Lock
  Screen Explorer by disabling the `lock` IPC handler, which otherwise
  shadowed it ("Function not found."). Presence and status moved to the
  `erijl.lock` target; lock-on-unplug hands off via `omarchy-shell lock lock`.

Pulling upstream later: `git fetch upstream && git merge upstream/production`.
