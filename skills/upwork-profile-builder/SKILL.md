---
name: upwork-profile-builder
description: Interviews the user to build or update their freelancer profile for Upwork proposal writing (name, experience, portfolio projects, tech stack, achievements, testimonials, writing tone). Use when the user wants to set up, edit, or redo their Upwork profile, before first using upwork-proposal-writer, or when they say things like "build my upwork profile", "set up my freelancer profile", "update my portfolio data", or "/upwork-profile-builder".
---

# Upwork Profile Builder

Builds the local profile file that `upwork-proposal-writer` reads from. Without this, that skill has nothing to personalize with.

Profile file location: `~/.claude/upwork-profile.json`

## Step 1: Check for an existing profile

Read `~/.claude/upwork-profile.json` if it exists.

- If it exists, show the user a short summary (name, role, number of projects, last updated) and ask: start fresh, edit specific sections, or add a new project. Don't silently overwrite.
- If it doesn't exist, proceed to a full interview.

## Step 2: Interview the user

Ask in rounds, not one giant form. Wait for answers before moving to the next round. Grill for specifics — vague answers produce generic proposals. Push back once if an answer is too thin (e.g. "shipped some apps" → ask which ones, what they did, what results).

### Round 1: Identity and positioning
- Full name (as they want it to appear to clients)
- Role/title (e.g. "Mobile Product Engineer", "Full-Stack Developer")
- One or two sentences on what they do and who they do it for
- Years of experience
- Current location/timezone (optional, only if relevant to client-facing framing)

### Round 2: Track record
- Key experience bullets: past roles, what was built, measurable outcomes (users, ratings, revenue impact, resolution times). Push for numbers wherever possible.
- Notable achievements: awards, hackathons, competitions, recognitions, media features. Ask which ones are worth surfacing to clients (not every internal award matters).
- Client testimonials: exact quotes if available, and where they're from (Upwork, LinkedIn, email). Do not paraphrase real quotes.

### Round 3: Portfolio projects
For each project worth referencing in proposals (aim for 5-15), collect:
- Name
- One-line category (e.g. "Fintech", "Social AI Platform", "E-commerce")
- 2-4 sentence description: what it does, notable technical complexity, scale (users/ratings if public)
- Tech stack used
- Links: live app/store link, GitHub, case study, etc.
- "Good to reference for" tags: what job types/domains/problems this project is strong evidence for (e.g. "fintech", "camera/vision work", "subscriptions", "real-time sync"). This is what lets the proposal writer pick the right 2-3 projects per job instead of listing everything.

Keep asking "any other projects worth including?" until the user says that's everything.

### Round 4: Tech stack
- Primary stacks/languages/frameworks
- Architecture patterns they use (e.g. Clean Architecture, MVVM)
- Backend/infra they're comfortable with
- Any specialized domains (computer vision, ML, payments, real-time, etc.)

### Round 5: Links and voice
- Portfolio site URL
- Any professional profile links worth citing (App Store developer page, GitHub, LinkedIn, Dribbble, etc.)
- Desired tone: confident-but-natural is the default and works well; ask if they want something different (more formal, more casual, more technical)
- Any words/phrases they personally never want used, or always want used

## Step 3: Write the profile file

Structure `~/.claude/upwork-profile.json` as:

```json
{
  "name": "",
  "role": "",
  "positioning": "",
  "years_experience": "",
  "location": "",
  "experience_bullets": [""],
  "achievements": [""],
  "testimonials": [{"quote": "", "source": ""}],
  "projects": [
    {
      "name": "",
      "category": "",
      "description": "",
      "tech": [""],
      "good_for": [""],
      "links": {"primary": "", "github": "", "other": ""}
    }
  ],
  "tech_stack": {
    "languages_frameworks": [""],
    "architecture": [""],
    "backend": [""],
    "specialized": [""]
  },
  "portfolio_url": "",
  "profile_links": [{"label": "", "url": ""}],
  "tone": "confident but natural, not salesy",
  "banned_words": [],
  "required_words": [],
  "last_updated": ""
}
```

Fill every field from the interview. Do not invent data the user didn't provide. Leave arrays empty rather than fabricating entries. Set `last_updated` to the current date.

Write the file with the Write tool, creating `~/.claude/` if it doesn't exist.

## Step 4: Confirm

After writing, tell the user in 1-2 lines: profile saved, how many projects captured, and that they can now use the `upwork-proposal-writer` skill by pasting a job description. Mention they can re-run this skill anytime to add projects or update details.
