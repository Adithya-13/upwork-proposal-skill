---
name: upwork-proposal-writer
description: Generates tailored, high-converting Upwork proposals using the user's own freelancer profile (built via upwork-profile-builder). Use whenever the user pastes a job description and asks for a proposal, cover letter, or bid for Upwork. Also trigger when the user says things like "write a proposal for this", "help me apply to this job", or shares any Upwork job posting.
---

# Upwork Proposal Writer

Generates high-converting Upwork proposals personalized to the user, using their profile data.

Profile file location: `~/.claude/upwork-profile.json`

## Step 0: Load the profile

Read `~/.claude/upwork-profile.json`.

- If it does not exist or is empty, stop and tell the user to run the `upwork-profile-builder` skill first. Do not write a generic, unpersonalized proposal as a fallback.
- If it exists, use it as the only source of truth for the user's identity, experience, projects, tech stack, links, and tone. Never invent experience, projects, or metrics that aren't in the profile.

## Step 1: Parse the input

The user will provide a job description. They may also provide some or all of the following optional context:

- CLIENT_NAME
- CURRENT_RELEVANT_PROJECT
- SPECIAL_EXPERIENCE_TO_HIGHLIGHT
- TIMELINE_ESTIMATE
- Technologies to emphasize

**Read the input carefully first.** Extract any already-answered variables from what the user has written. Only ask about what is genuinely missing and required to write a good proposal.

**Do not ask questions that are already answered in the input.**

**Secret word check:** Scan the job description for phrases like "start your proposal with [word]", "reply with [word]", or "include [word] in your response." Clients use this to filter freelancers who didn't read the post. If found, open the proposal with that word or phrase exactly as instructed.

If the job description is detailed enough to infer the answers (e.g., no client name is visible, tech stack is obvious from the JD), use your best judgment and proceed. Only pause to ask if the missing info would meaningfully change the proposal.

Minimum required to proceed: the job description itself, plus a loaded profile.

**Before writing, always ask the user 2-3 personalization questions using the ask_user_input tool.** These make the proposal feel personal and specific, not templated. Ask only what you cannot infer from the JD or the profile. Good questions to consider:

- Is there a specific project from the profile that's most relevant to this job?
- Is there a current project or recent work that's directly relevant (not yet in the profile)?
- Any personal connection to this domain or problem (used the product, built something similar)?
- Is there a specific tech from the JD they want to emphasize?
- Any constraint to flag (timeline, budget, availability)?

Keep questions tight. Max 3. Do not ask things already answered in the JD or the profile.

## Step 2: Select relevant projects

From `profile.projects`, pick the 2-3 whose `good_for` tags and `description` most closely match the job description's domain and technical requirements. Do not default to listing every project. Prefer specific technical overlap over generic recency.

## Step 3: Write the proposal

Follow all rules and structure below exactly, using the loaded profile for every factual claim.

---

## Writing Rules (mandatory)

1. NEVER use markdown bold like **text**. Use Unicode bold characters instead.
   Unicode bold is used TWO ways:
   a) Section headers (as specified in the structure below)
   b) Inline key phrases within normal sentences — highlight the 2-4 words that are most relevant to THIS specific job. Pick phrases that show direct capability match: a tech, a metric, a project name, a specific skill.
   Example: "I build native iOS apps and have shipped 𝘀𝘂𝗯𝘀𝗰𝗿𝗶𝗽𝘁𝗶𝗼𝗻 𝗴𝗮𝘁𝗶𝗻𝗴, 𝗦𝘁𝗿𝗶𝗽𝗲 𝗶𝗻𝘁𝗲𝗴𝗿𝗮𝘁𝗶𝗼𝗻, and 𝗿𝗲𝗮𝗹-𝘁𝗶𝗺𝗲 𝘀𝘁𝗮𝘁𝗲 𝗺𝗮𝗻𝗮𝗴𝗲𝗺𝗲𝗻𝘁 in production."
   Do not bold entire sentences or random words. Bold only the phrases that directly map to what the client asked for.
2. NEVER use em dashes (— or --). If you find yourself writing one, replace it with a comma, period, or colon. Scan the full output before finishing and remove every em dash.
3. Proposal must scan well in 5 seconds. Use spacing and short bullet points.
4. Tone: use `profile.tone`. Default is confident but natural, not salesy.
5. Do not exaggerate experience. Only claim what's in the profile.
6. Word count: 200 to 350 words. Stay in range.
7. Adapt every proposal to the specific job description.
8. Respect `profile.banned_words` (never use) and `profile.required_words` (always consider including naturally) if set.

### Anti-AI writing rules (mandatory)

These patterns make proposals read as AI-generated. Avoid all of them:

- NO AI vocabulary: never use "additionally", "moreover", "testament", "landscape", "showcasing", "underscoring", "fostering", "pivotal", "seamless", "leverage", "delve", "comprehensive", "robust", "innovative", "groundbreaking", "streamline"
- NO significance inflation: never say things are "transformative", "game-changing", or "reshaping the industry"
- NO "it's not just X, it's Y" constructions. State the point directly.
- NO rule of three padding: "innovation, inspiration, and insights" — just say what you mean
- NO copula avoidance: don't write "serves as", "functions as", "stands as" — use "is" or "does"
- NO filler openers: cut "In order to", "Due to the fact that", "It is worth noting that"
- NO excessive hedging: "could potentially possibly" becomes "may"
- NO generic conclusions: "the future looks bright", "exciting times ahead" — end on something concrete
- NO promotional language: "world-class", "breathtaking", "cutting-edge"
- Write like a person talking to another person. Short sentences. Direct.

---

## Proposal Structure

Follow this structure in order:

**Greeting**
- If client name known: Hey [Name],
- If unknown: Hey there,

**Opening**
- Do not write a manufactured hook or attention-grabbing opener. That reads as AI.
- Instead, open with 1-2 plain, honest sentences that show you read the JD and get what they need.
- Speak directly. No drama.
- Inline bold key phrases where relevant (see writing rules).

**Domain exploration (conditional)**
- Only include if the job has a specific industry context: fintech, healthcare, civic tech, sports analytics, e-commerce, legal, education, etc.
- Add 1-2 sentences right after the opening, before credibility.
- Write this in first-person as the user, like they actually looked into the client's world before applying. Not a stat dump. Their POV.
- Good: "I looked into how budgeting apps typically lose users, most drop off not because the data is wrong, but because the interface makes them feel judged."
- Bad: "Research shows 70% of users abandon budgeting apps within the first week." (reads like a Wikipedia citation, not a person)
- The insight must be directly relevant to their specific problem in the JD. Not generic industry trivia.
- Do not fabricate. Only include if you can derive a credible, specific insight from what the JD reveals.
- Skip entirely for generic "build me an app" JDs with no specific industry context.

**Credibility**
- Brief: production work, real users, backed by `profile.experience_bullets`.
- Mention the 2-3 projects selected in Step 2, with their links.
- If `profile.profile_links` includes a developer/portfolio page, add one line pointing to it.

**Relevant experience section**

Header (Unicode bold):
𝗥𝗲𝗹𝗲𝘃𝗮𝗻𝘁 𝗲𝘅𝗽𝗲𝗿𝗶𝗲𝗻𝗰𝗲 𝗳𝗼𝗿 𝘁𝗵𝗶𝘀 𝗽𝗿𝗼𝗷𝗲𝗰𝘁

Bullet points matching specific job requirements to the profile's experience and tech stack. Be concrete.

**Micro-milestone (conditional)**
- Only include if the project scope is clear enough to propose a concrete first step.
- One sentence offering a small, testable deliverable the client can evaluate before committing fully.
- Example: "I can ship a working prototype of the camera screen in 3 days so you can validate the feel before we go further."
- This lowers the client's risk and makes saying yes easier.
- Skip if the project scope is vague or exploratory.

**Design philosophy**

Header (Unicode bold):
𝗪𝗵𝗮𝘁 𝗴𝗼𝗼𝗱 𝗱𝗲𝘀𝗶𝗴𝗻 𝗮𝗻𝗱 𝗲𝘅𝗽𝗲𝗿𝗶𝗲𝗻𝗰𝗲 𝗺𝗲𝗮𝗻𝘀 𝘁𝗼 𝗺𝗲

One sentence. Make it specific to the type of app or project.

**Something cool**

Header (Unicode bold):
𝗦𝗼𝗺𝗲𝘁𝗵𝗶𝗻𝗴 𝗰𝗼𝗼𝗹 𝗮𝗯𝗼𝘂𝘁 𝗺𝗲

Pick ONE fact from `profile.achievements` that best fits the job context (e.g. a fintech award for a fintech client, an international program for an international client, active learning/education credential for a client who values growth). Do not list all achievements, just the most relevant one.

**Timeline**
- Only include if the job asks for it or if a TIMELINE_ESTIMATE was provided.
- Estimate realistically.

**Portfolio**
Portfolio: `profile.portfolio_url`

**Ending question**
- One thoughtful technical question about their product. Not generic.
- Shows you read the job description.

---

## Output Rules

- Return ONLY the final proposal text.
- No explanations, no markdown, no preamble.
- Do not add headers outside the ones specified in the structure.
