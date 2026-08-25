# Thunderbird

Thanks for checking out our project! If you are a student working with us, We're so excited to meet you, and get to see the awesome stuff you can make!

Thunderbird is an indie animation done by UVU students. 
[description of what it actually is here]

---

## First-time setup

**You must install Git LFS before cloning.** Without it Maya scenes will be lost.

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
merged**. ONLY ONE PERSON CAN WORK ON THESE FILES AT A TIME! Git has
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

**Rule of thumb:** lock it, do your work, push, unlock.

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
