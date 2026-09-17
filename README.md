# Upwork Proposal Kit

Build your freelancer profile once, then generate tailored, high-converting Upwork proposals from it, in Claude Code or in any other AI chat tool.

- **Profile builder** interviews you (and can be re-run any time to update) and captures your experience, portfolio projects, tech stack, achievements, and testimonials into a single profile.
- **Proposal writer** reads that profile, plus a job description you paste in, and writes a tailored proposal in your voice, following rules tuned to avoid generic "AI-sounding" proposals (no em dashes, no AI vocabulary, no manufactured hooks, 200-350 words).

Two ways to use it, same content, different handoff mechanism depending on the tool:

| | `skills/` (Claude Code) | `universal-prompts/` (any AI) |
|---|---|---|
| Where it runs | Claude Code, or any tool that supports the Agent Skills format (SKILL.md) with file read/write | ChatGPT, Gemini, or any chat AI, including Claude used as plain chat |
| Where your profile lives | `~/.claude/upwork-profile.json`, read/written automatically by the AI | A JSON block the AI prints in chat; you save it yourself as a text file and paste it back in later |
| Setup | `/plugin install` (or copy skill folders manually) | Paste a prompt file's contents into the tool's system/custom instructions or as your first message |

Your profile data is personal to you. It's never committed to this repo, and neither version sends it anywhere beyond the conversation you're having.

## Option A: Claude Code

### As a Claude Code plugin (recommended)

```
/plugin marketplace add Adithya-13/upwork-proposal-skill
/plugin install upwork-proposal-skill@upwork-proposal-skill
```

That installs both skills at once. Restart Claude Code (or start a new session) so it picks them up.

### As plain Claude Code skills (manual copy)

If you'd rather not add a plugin marketplace, clone the repo and copy the skill folders in directly:

```bash
git clone https://github.com/Adithya-13/upwork-proposal-skill.git
mkdir -p ~/.claude/skills
cp -r upwork-proposal-skill/skills/upwork-profile-builder ~/.claude/skills/
cp -r upwork-proposal-skill/skills/upwork-proposal-writer ~/.claude/skills/
```

Restart Claude Code (or start a new session) so it picks up the new skills.

**First run:**
1. Tell Claude: `set up my upwork profile` (or `/upwork-profile-builder` if your setup supports slash-invoking skills by name).
2. Answer the interview questions honestly and with specifics: numbers, real project names, real links. Vague answers produce generic proposals.
3. The skill saves everything to `~/.claude/upwork-profile.json`.

**Writing a proposal**, once your profile exists:

```
Here's the job description: <paste>
Write me an Upwork proposal for this.
```

The skill loads your profile automatically, asks up to 3 personalization questions it can't infer from the JD or profile, picks the 2-3 most relevant projects, and returns a ready-to-paste proposal.

**Updating your profile:** re-run the builder any time, e.g. `update my upwork profile, I want to add a new project`. It reads the existing file first and asks whether to add, edit, or start over, rather than blindly overwriting it.

## Option B: Any other AI (ChatGPT, Gemini, etc.)

These tools generally can't read or write files on your machine mid-conversation, so the workflow is copy/paste based instead of automatic.

**Step 1: Build your profile.**
1. Open `universal-prompts/upwork-profile-builder-prompt.md` in this repo.
2. Paste its full contents as your first message (or as the tool's custom/system instructions, if it supports that) to a new chat.
3. Answer the interview questions.
4. At the end, the AI prints your profile as a JSON code block. Copy it and save it as a text file, e.g. `upwork-profile.json`, somewhere you'll find it again.

**Step 2: Write a proposal.**
1. Start a new chat and paste the full contents of `universal-prompts/upwork-proposal-writer-prompt.md` as your first message.
2. In the same or a follow-up message, paste your saved profile JSON and the job description together.
3. Answer any personalization questions the AI asks.
4. It returns the finished proposal.

Since these tools don't remember your profile between chats, keep the JSON file somewhere handy, you'll paste it in every time you want a new proposal. Some tools (e.g. a ChatGPT Custom GPT with saved instructions, or a Gemini Gem) let you bake the profile JSON permanently into the custom instructions alongside the proposal writer prompt, so you don't have to paste it every time. Check that tool's docs for how it persists custom instructions.

## Profile shape

See `profile.example.json` in this repo for the full field reference, useful if you want to hand-edit your profile directly instead of (or in addition to) running the interview.

## Privacy

- Your profile is either a local file (`~/.claude/upwork-profile.json`, Claude Code path) or a JSON block you keep in your own text file (universal prompts path). Either way, it's yours, not stored by this repo.
- This repo's `.gitignore` excludes any file matching `upwork-profile.json`, so you won't accidentally commit your own data if you fork this repo to version your profile.
- Nothing in these skills/prompts sends your data anywhere beyond the AI conversation you're having.

## License

MIT. See `LICENSE`.
