# Upwork Proposal Skill

Two Claude Code skills that work together to write personalized, high-converting Upwork proposals, without hardcoding anyone's personal data into the skill itself.

- `upwork-profile-builder` interviews you once (and any time you want to update it) and saves your experience, portfolio projects, tech stack, achievements, and testimonials to a local profile file.
- `upwork-proposal-writer` reads that profile, plus a job description you paste in, and writes a tailored proposal in your voice, following rules tuned to avoid generic "AI-sounding" proposals.

Your data never leaves your machine and is never committed to this repo. The profile lives at `~/.claude/upwork-profile.json`.

## Why two skills

A proposal writer that already knows your background writes much better proposals than one you re-explain every time. But personal data doesn't belong hardcoded into a shared skill. Splitting it into a one-time interview + a generation step means:

- You set up your profile once, in your own words, and can re-run the builder any time to add projects or edit details.
- The proposal writer stays generic and shareable: anyone can install it and build their own profile.
- Every proposal pulls from the same source of truth, so your story stays consistent across applications.

## Install

Clone or download this repo, then copy both skill folders into your Claude Code skills directory:

```bash
git clone https://github.com/Adithya-13/upwork-proposal-skill.git
mkdir -p ~/.claude/skills
cp -r upwork-proposal-skill/skills/upwork-profile-builder ~/.claude/skills/
cp -r upwork-proposal-skill/skills/upwork-proposal-writer ~/.claude/skills/
```

Restart Claude Code (or start a new session) so it picks up the new skills.

## First run

1. Tell Claude: `set up my upwork profile` (or `/upwork-profile-builder` if your setup supports slash-invoking skills by name).
2. Answer the interview questions honestly and with specifics: numbers, real project names, real links. Vague answers produce generic proposals.
3. The skill saves everything to `~/.claude/upwork-profile.json`.

See `profile.example.json` in this repo for the shape of the file if you want to hand-edit it directly instead of (or in addition to) using the interview.

## Writing a proposal

Once your profile exists, paste a job description and ask Claude to write a proposal for it:

```
Here's the job description: <paste>
Write me an Upwork proposal for this.
```

The skill will:
1. Load your profile.
2. Ask up to 3 quick personalization questions it can't infer from the job post or your profile (e.g. which project is most relevant, any timeline constraint).
3. Pick the 2-3 most relevant projects from your profile based on the job's domain and tech stack.
4. Return a ready-to-paste proposal, 200-350 words, formatted for Upwork (Unicode bold for emphasis and headers, no markdown, no em dashes).

## Updating your profile

Re-run the profile builder any time:

```
update my upwork profile, I want to add a new project
```

It reads the existing file first and asks whether you want to add, edit, or start over, rather than blindly overwriting it.

## Privacy

- `~/.claude/upwork-profile.json` is a local file. This repo's `.gitignore` excludes any file matching that name so you don't accidentally commit your own data if you fork this repo to version your profile.
- Nothing in either skill sends data anywhere outside your own Claude Code session.

## License

MIT. See `LICENSE`.
