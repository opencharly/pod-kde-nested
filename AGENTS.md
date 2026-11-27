# AGENTS.md — pod-kde-nested

The org rulebook at the umbrella (`opencharly/opencharly` `AGENTS.md`) applies in full.
This file adds only what is specific to this repo.

Canonical files:

- `charly.yml` holds:
  - the `kde-nested` candy: the nested Plasma session service, the `KDE_NESTED_*` knobs
    and the caps strip;
  - the `kde-nested-skill` routing node.
- `kde-nested-session`: the launcher. It waits for the host compositor, starts
  `kwin_wayland` nested in it and `plasmashell` on KWin's socket, on the pod's session
  bus or a private one.
- `.agents/skills/kde-nested/SKILL.md`: the skill body, including the knob table per
  consuming pod.
- `.github/workflows/tag-on-merge.yml`: the CalVer tag and `CHANGELOG/` on merge,
  installed by `bootstrap-repo-main`. Never hand-edit it.
- `README.md`: the user overview only, never agent guidance.

## Load these skills first (R0)

- `/charly-selkies:kde-nested`: this repo's own skill.
- `/charly-selkies:kde-shell`: the Plasma package set this leaf composes.
- `/charly-image:layer`: the candy authoring surface.
- `/charly-check:check`: the plan steps and the consumers' R10 beds.
- `/charly-internals:git-workflow`: before any git or PR action.

## Rules specific to this repo

- **One launcher for every nested Plasma.** pod-kde-vnc and pod-kde-vnc-cdp (through
  pod-kde-vnc) compose this candy and set `KDE_NESTED_*`; neither carries a launcher of
  its own (R2). A need the knobs cannot express is a new knob here, not a copy there.
  pod-kde-selkies and pod-kde-cstream composed this leaf too and were retired and
  archived by opencharly/opencharly#1293.
- **No transport, no host compositor, no GPU wiring here.** The composing pod owns those
  (sway/wayvnc; pod-cdi for the GPU).
- **The leaf's runtime checks stay bus-independent.** `KDE_NESTED_BUS=private` puts KWin
  on a bus the venue's `dbus:` verb does not see, so bus-level assertions (KWin's
  renderer) belong to each pod's own bed.
- **R10 gate.** This repo has a bed of its own: `check-kde-nested-locked` (image
  `kde-nested-bed-image`, `disposable: true`), which proves the leaf's screen-locker
  policy holds across Plasma's idle timeout on the CDI GPU base. A change to the leaf's
  runtime path is ALSO proven by a consuming pod's bed, run with
  `CHARLY_REPO_OVERRIDE=opencharly/pod-kde-nested=<this tree>` — today
  `check-kde-vnc-pod` (pod-kde-vnc: session bus, `1920x1080`, socket `wayland-kde`,
  runtime dir `/tmp`) and `check-kde-vnc-cdp-pod` (the pod that adds Chrome). The
  private-bus branch has no live consumer since the #1293 retirement — the runs it owed
  were tracked by opencharly/pod-kde-nested#2, closed with those consumers — so it stays
  covered by the launcher shape checks, never by a claim.
