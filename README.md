# Thunderbird

Animation project repository.

This repo uses **Git LFS** to version large binaries (scene files, textures, audio,
footage). Git itself only stores small text pointers; the real bytes live on the
LFS server and download when you check a branch out.

---

## First-time setup

**You must install Git LFS before cloning.** Without it you'll get 3-line text
files where your Maya scenes and comps should be.

**macOS**

```bash
brew install git-lfs
git lfs install          # one time, per machine
```

**Windows** — install from <https://git-lfs.com>, then in Git Bash or PowerShell:

```bash
git lfs install
```

Then clone as normal:

```bash
git clone https://github.com/braedawg02/Thunderbird.git
```

If you cloned *before* installing LFS, fix it with:

```bash
git lfs install
git lfs pull
```

---

## Working on scene files

Maya, 3ds Max, Cinema 4D, After Effects, Premiere, and Photoshop files **cannot be
merged**. If two people edit the same scene, one of them loses their work — Git has
no way to combine them.

To prevent that, these file types are marked `lockable`. They appear **read-only**
until you take a lock.

```bash
git lfs lock shots/sq010_sh020/anim.ma     # claim it — do this BEFORE you edit
# ... work, save, commit ...
git push
git lfs unlock shots/sq010_sh020/anim.ma   # release it when you're done
```

Useful commands:

```bash
git lfs locks                    # who currently holds what
git lfs locks --verify           # check before pushing
git lfs unlock --force <file>    # steal a stale lock (coordinate first!)
```

**Rule of thumb:** lock it, do your work, push, unlock. Don't sit on a lock overnight.

---

## Folder structure

```
assets/
  characters/      models + rigs, one folder per character
  environments/    sets and locations
  props/
  rigs/            shared/reusable rigs
  textures/        source textures (.psd, .exr, .tga)
shots/             per-shot scene files: shots/sq010_sh020/
comps/             After Effects projects
edit/              Premiere projects
audio/
  dialogue/
  music/
  sfx/
reference/         video reference, boards, style frames
renders/           LOCAL ONLY — gitignored, see below
scripts/           pipeline scripts, expressions, tools
docs/              notes, breakdowns, schedules
```

---

## What is *not* in this repo

**Renders and image sequences are gitignored.** They're reproducible from the scene
files, and an image sequence will exhaust our LFS storage quota almost immediately.
Put finished renders in shared storage (Drive, Dropbox, a NAS) and keep `renders/`
as your local scratch space.

Also ignored: simulation and render caches, After Effects auto-saves, Premiere media
cache and preview files, Maya incremental saves, and OS junk (`.DS_Store`, `Thumbs.db`).

See [.gitignore](.gitignore) for the full list, and [.gitattributes](.gitattributes)
for exactly which extensions go to LFS.

---

## Storage budget

GitHub's free LFS tier is **1 GB storage and 1 GB/month bandwidth**, and additional
capacity is $5/month per 50 GB. That's the reason renders stay out of the repo.

Before committing something unusually large, check what you're about to add:

```bash
git lfs status                   # what's staged for LFS
du -sh <file>                    # how big is it
```

If the project grows past a few hundred GB, we should move to a dedicated art
pipeline host rather than paying for GitHub LFS packs.
