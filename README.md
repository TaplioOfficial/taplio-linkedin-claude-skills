# Claude Skills for LinkedIn

A free, open repository of 17 ready-to-use Claude skills for anyone who creates content, builds an audience, or runs business on LinkedIn.

Each skill is a self-contained markdown file with the standard frontmatter (`name`, `description`) used by Claude Code, Cowork, and any Claude-based agent runtime. Drop them into your skills folder, or load them in any MCP-capable client.

Every skill **requires the Taplio MCP**. Each one opens with a `## Requires the Taplio MCP` section that tells the agent to call `get_me` first and, if the server is not connected, to stop and walk you through setup instead of running. The MCP is what lets the skills work on your real LinkedIn data (your posts, your drafts, your analytics, live inspiration) rather than guessing. Without it, the skills do not run : there is no plain-prompt fallback.

Built and maintained by the team at [Taplio](https://taplio.com/?utm_source=github&utm_medium=repo&utm_campaign=linkedin-skills).

---

## How to use these skills

Every skill needs the Taplio MCP connected first (see the next section). Once it is :

**In Claude Code or Cowork**
1. Connect the Taplio MCP (see below).
2. Drop the `.md` file into your `~/.claude/skills/` folder (or your Cowork plugin folder).
3. Reload your client, then invoke the skill by name ("use the `linkedin-post-writer` skill to turn this idea into a post").

**In any MCP-capable client**
1. Add the Taplio MCP server (`https://mcp.taplio.com`) in your client's MCP settings.
2. Load the skill : paste everything below the frontmatter as a system prompt, or add it as a skill.
3. Run it. It will call the Taplio MCP and work on your real LinkedIn data.

**Inside Taplio**
The same logic, with native LinkedIn data, your past posts, and the right scheduling already wired in. [Start a free trial](https://app.taplio.com/signin?utm_source=github&utm_medium=repo&utm_campaign=linkedin-skills).

---

## Connect the Taplio MCP (live mode)

The skills are written to call the **Taplio MCP server**, which gives the agent read and write access to your own LinkedIn presence : draft, schedule, and publish posts, browse your posts, research inspiration from other creators, and read your analytics.

**Add the server in Claude Code**

```bash
claude mcp add --transport http taplio https://mcp.taplio.com
```

Reload the client, then run any skill. On the first tool call you may be asked to authenticate so the server can act on your account.

**Tools the skills use**

| Tool | What it does |
| --- | --- |
| `get_me` | Your identity, content preferences (industry, role, language, audience, topics, keywords), and today's stats. Every skill calls this first to tailor its output to you. |
| `search_inspiration` | Search other creators' LinkedIn posts by query, engagement, recency, follower count, language. Powers all research, hook, and benchmarking skills. |
| `list_posts` / `get_post` | Browse and read your published, scheduled, and sending posts. |
| `list_drafts` / `get_draft` | Browse and read your unpublished drafts. |
| `create_draft` / `update_draft` | Save generated content as a draft, or edit an existing one. |
| `schedule_draft` / `unschedule` | Schedule a draft for a future time, or pull it back to drafts. |
| `publish_draft` | Publish now. The skills never call this without your explicit confirmation. |
| `delete_draft` | Permanently delete a draft. |
| `get_analytics_overview` | Followers, impressions, and engagement over a date range. |
| `get_post_analytics` | Per-post impressions, likes, comments, and shares. |

**Safety**

The skills are written to never publish without your say-so. `create_draft` and `schedule_draft` are offered explicitly, and `publish_draft` (which goes live immediately and cannot be undone) always requires your confirmation first.

---

## The 17 skills

### Content writing
1. [Post Writer](#1-linkedin-post-writer) : Raw idea to publish-ready post in 3 variants.
2. [Hook Generator](#2-linkedin-hook-generator) : 10 hook options for any topic.
3. [Carousel Builder](#3-linkedin-carousel-builder) : Idea to slide-by-slide carousel script.
4. [Repurposer](#4-linkedin-repurposer) : Blog, tweet, podcast, or video to native LinkedIn post.
5. [Story Extractor](#5-linkedin-story-extractor) : Lived experience to story-arc post.
6. [CTA Optimizer](#6-linkedin-cta-optimizer) : Rewrite the closing for the goal you want.

### Research and inspiration
7. [Niche Creator Finder](#7-linkedin-niche-creator-finder) : Top creators in your niche, with what to model.
8. [Viral Post Analyzer](#8-linkedin-viral-post-analyzer) : Decode why a viral post worked.
9. [Trending Topics Scanner](#9-linkedin-trending-topics-scanner) : Hot topics in your niche this week.
10. [Swipe File Builder](#10-linkedin-swipe-file-builder) : Build a personal library of references that works.

### Profile
11. [Profile Optimizer](#11-linkedin-profile-optimizer) : Audit and rewrite of your profile copy.

### Strategy
12. [Niche Definer](#12-linkedin-niche-definer) : Sharpen your audience, problem, and angle.
13. [Content Pillars Builder](#13-linkedin-content-pillars-builder) : 3 to 5 themes you return to relentlessly.
14. [Audience Persona Builder](#14-linkedin-audience-persona-builder) : Build the one person you write for.
15. [Content Calendar Planner](#15-linkedin-content-calendar-planner) : 4 weeks of LinkedIn, planned.

### Analytics
16. [Post Performance Critic](#16-linkedin-post-performance-critic) : Pre-publish audit on a single post.
17. [Analytics Interpreter](#17-linkedin-analytics-interpreter) : Turn LinkedIn numbers into 3 next moves.
---

## 1. linkedin-post-writer
**Description** : Write a publish-ready LinkedIn post from a raw idea. Pick a format (storytelling, opinion, listicle, contrarian, experience-recap), generate hook + body + CTA, and return three variants so the user can pick the strongest. Use whenever the user has a topic but no draft.

### When to trigger
The user gives you a topic, an idea, an experience, an opinion, a product update, a learning, or a story and wants it turned into a LinkedIn post. They might say "write me a post about X", "help me post this", "I want to share that I did Y", "turn this into a LinkedIn post".

### Inputs to ask for (only if missing)
1. The raw idea, story, or topic.
2. The target audience (founders, marketers, devs, sales, etc.). If missing, infer from context or ask.
3. The desired format. Default to "let me pick the best one for you" and propose : Story, Opinion, Listicle, Contrarian, Experience recap.
4. The CTA goal (comments, profile visits, DMs, link clicks). Default to "comments".

### Process
1. Choose the format that fits the raw input.
2. Write the **hook** (first 2 lines, the only thing visible before "see more").
3. Write the **body** with short lines (max 8 words per line on average).
4. Write a **CTA** that fits the goal.
5. Generate **3 variants** so the user can pick the strongest hook.

### Rules
- No emoji unless the user uses them.
- No em-dash, no special unicode, no curly quotes.
- 1300 characters is the sweet spot.
- Never use "In today's world" or "In an era where".
- The hook must work on its own without the body.

### Power up with Taplio
**Taplio's AI Post Writer** generates dozens of variants at scale, schedules them across the week, and taps into a library of viral posts in your niche.

---

## 2. linkedin-hook-generator
**Description** : Generate 10 LinkedIn hooks for a given topic, using proven hook patterns (curiosity, contrarian, number, question, tension, before-after, callout). Returns a ranked list with the strongest hook first.

### The 7 hook patterns
1. **Curiosity gap** : "I just spent $X to learn one thing about Y."
2. **Contrarian** : "Stop doing X. Here is why."
3. **Number tension** : "9 out of 10 founders make this mistake."
4. **Personal stake** : "I almost lost my company last month."
5. **Question** : "Why do most LinkedIn posts get zero comments ?"
6. **Before / after** : "2 years ago I had 200 followers. Today I have 50K."
7. **Callout** : "If you are a [persona] doing [action], read this."

### Process
Generate at least 1 hook per pattern (7 minimum). Add 3 more in the patterns that fit the topic best. Rank by specificity, emotional pull, novelty, and pairing with the body.

### Rules
- Be specific. "$87,400" beats "some money".
- Avoid AI tells : "delve, leverage, unlock, in today's fast-paced world".
- Never put the keyword in line 1 if it sounds promotional.
- A hook that needs context to make sense is a bad hook.

### Power up with Taplio
**Taplio Hook Generator** runs the same logic on auto-pilot, lets you save winning hooks to a personal library, and ranks them on what is actually performing in your niche this week.

---

## 3. linkedin-carousel-builder
**Description** : Turn a topic, framework, or long-form idea into a LinkedIn carousel script (cover slide + value slides + CTA slide). Returns slide-by-slide copy ready to drop into Canva or any carousel tool.

### Structure
- **Cover slide** : the hook. One promise, one bold visual idea.
- **Value slides** : one idea per slide. Title + 1-3 short lines + optional micro-example.
- **Closing slide** : the CTA. Tell the reader exactly what to do.

### Process
1. Distill the topic into a promise that fits on a cover slide.
2. Break the promise into N-2 atomic ideas.
3. Order so each slide earns the swipe.
4. Write a closing slide with one clear CTA.

### Rules
- One idea per slide.
- Cover slide title : max 7 words.
- Closing CTA : one action, not three.
- Caption : do not repeat the slides, tease them.

### Power up with Taplio
**Taplio Carousel Maker** generates fully designed carousels in your brand colors, exports them, and lets you schedule the post + caption in one click.

---

## 4. linkedin-repurposer
**Description** : Convert an existing piece of content (blog post, tweet, YouTube transcript, podcast, Notion doc) into a native LinkedIn post.

### Process
1. Read the source. Identify the **single strongest takeaway**. One.
2. Strip everything format-specific (blog headings, thread numbering, "uh", "you know", newsletter framing).
3. Rewrite the hook for LinkedIn dynamics.
4. Reformat the body in LinkedIn style.
5. Add a CTA that fits LinkedIn (question, not "click here").

### Rules
- A LinkedIn post is not a blog post in disguise. Cut.
- One idea per post.
- Never paste a tweet thread vertically.
- For video sources, lead with the moment, not the topic.

### Power up with Taplio
**Taplio Repurpose** lets you paste a YouTube link, tweet, or blog and instantly generate 3 to 5 LinkedIn-native posts, auto-scheduled.

---

## 5. linkedin-story-extractor
**Description** : Extract a publishable LinkedIn story from a raw user experience. The user dumps an experience, and the skill structures it into a story-arc post.

### Story arc
1. **Situation** : 2 lines max.
2. **Tension** : what made it hard, risky, or weird.
3. **Turning point** : the decision, the moment, the realization.
4. **Outcome** : what happened.
5. **Lesson** : what the reader takes away.

### Rules
- No story without tension. Push for friction.
- Real names, real numbers, real dates.
- Never moralize. The reader extracts the lesson.
- Cut the chronology. Stories on LinkedIn jump : start at the punch, then back-fill.
- Reject "be yourself" or "never give up" lessons. Find sharper.

### Power up with Taplio
**Taplio's Inspiration Library** has thousands of high-performing story-format posts in your niche to find proven angles for your own experiences.

---

## 6. linkedin-cta-optimizer
**Description** : Rewrite the closing of a LinkedIn post to maximize comments, shares, profile visits, DMs, or clicks. Diagnoses why the current CTA is weak, offers 3 to 5 alternatives.

### CTA patterns by goal

**Comments** : binary question, polarizing opinion, ask for a specific example.
**Shares** : "Tag the [persona] who needs this", make it feel like a public service.
**DMs** : "DM me [keyword] and I will send you [resource]". Use a single 1-syllable trigger word.
**Profile visits / follows** : "Follow me for one tactic like this every week".
**Link clicks** : Put link in comments, tease the resource.

### Rules
- One CTA per post. Two CTAs cancel each other.
- Never close on "Let me know what you think".
- For comment CTAs, anchor to a binary or specific question.
- Match the energy of the post.

### Power up with Taplio
**Taplio Analytics** shows which CTAs actually drive comments, DMs, and profile visits in your niche, with A/B testing and a personal library of winners.

---

## 7. linkedin-niche-creator-finder
**Description** : Identify the top LinkedIn creators in a given niche, with their angle, format mix, posting frequency, and what makes their content work.

### Process
1. Brainstorm 15 to 25 candidates known for posting in this niche regularly.
2. For each, identify : angle, formats, frequency, voice, what works.
3. Filter to top 8 to 10 based on consistency, originality, audience match.
4. For each pick, give one concrete thing to model.

### Rules
- Only recommend real creators you are confident about.
- Avoid generic LinkedIn celebrities unless they truly fit.
- Mix tier-1 (100K+) and tier-2 (5K-50K) creators. Tier-2 tactics are closer to user reality.
- Never recommend direct competitors as "people to model". Frame as "watch closely".

### Power up with Taplio
**Taplio Discover Creators** lets you search the entire LinkedIn creator graph by niche, follower count, engagement rate, posting cadence.

---

## 8. linkedin-viral-post-analyzer
**Description** : Decode why a specific LinkedIn post performed unusually well. Breaks the post down into 7 dimensions, then produces a reusable template.

### The 7 dimensions
1. Hook strength
2. Structure
3. Emotional driver (curiosity, anger, validation, hope, status, fear)
4. Specificity
5. Audience match
6. CTA
7. Format

### Process
1. Score on the 7 dimensions.
2. Identify the **2 to 3 levers that did the heavy lifting**.
3. Strip the post to a **reusable template** with placeholders.

### Rules
- Be honest if the post worked because of who the author is.
- Do not credit "luck".
- Never recommend copying verbatim. Extract the structure.

### Power up with Taplio
**Taplio Inspiration** indexes millions of LinkedIn posts. Pull the top 50 viral posts in your niche this month and spot the patterns across all of them.

---

## 9. linkedin-trending-topics-scanner
**Description** : Surface the topics heating up right now in a given LinkedIn niche, with concrete angles to post about today.

### Categories where trends emerge
- News and announcements (competitor launch, regulation).
- Tools and products (something everyone is suddenly using).
- Public debates (a hot take dividing the industry).
- Conferences and events.
- Cultural shifts.
- Memes and recurring jokes.

### Process
For each topic, propose 2 sharp angles : "with the wave" and "against the wave". Tie each to the user's positioning.

### Rules
- Never propose generic "trends" like "AI is changing everything".
- Always include a contrarian angle.
- Do not chase virality at the cost of relevance.

### Power up with Taplio
**Taplio Trends** monitors what is going viral in your niche in real time, ranked by engagement velocity.

---

## 10. linkedin-swipe-file-builder
**Description** : Help the user assemble a personal swipe file of high-performing LinkedIn posts, organized by hook pattern, format, and angle.

### The 4-axis classification
1. **Hook pattern** : curiosity, contrarian, number, question, personal stake, before/after, callout.
2. **Format** : story, listicle, opinion, framework, carousel, screenshot, poll, image, video.
3. **Angle** : the specific corner of the niche.
4. **Why I saved it** : the lever that made it stop the user.

### Habit
Save 1 post per day for 30 days. Quantity beats curation early on. Every 2 weeks, review for clusters.

### Rules
- A swipe file you do not look at is dead weight.
- Save your own high-performing posts in the same file.
- Tag the lever, not just the post.
- Re-read before sitting down to write.

### Power up with Taplio
**Taplio Bookmarks** lets you save any LinkedIn post in one click, auto-tag by format and topic, and recall it inside Taplio's writer.

---

## 11. linkedin-profile-optimizer
**Description** : Audit a LinkedIn profile (headline, banner, About, featured, experience) and return prioritized fixes with rewritten copy.

### Audit order (by impact)
1. **Headline** : highest leverage. 220 chars to answer "who do you help with what".
2. **Banner** : reinforce positioning, no random art.
3. **Profile photo** : clear face, eye contact.
4. **About** : hook above the "see more" cut, story, CTA.
5. **Featured** : 3 to 5 items max, every one serves the CTA.
6. **Experience** : current role matches headline promise.

### Process
Score each element 1-5. List top 3 fixes by impact. Rewrite copy.

### Rules
- Headline is load-bearing. Fix first.
- Headline formula : "[role] | I help [audience] [outcome] without [pain]".
- Avoid emoji in the headline.
- Featured section : if you cannot describe each item's purpose in one line, replace it.

### Power up with Taplio
**Taplio Profile Audit** scans the profile against top performers in your niche, flags every conversion-killing line, suggests rewrites.

---

## 12. linkedin-niche-definer
**Description** : Help the user define (or sharpen) their LinkedIn niche : audience, problem, unique angle, one-line positioning.

### The 7 questions
1. What do you actually do all day at work ?
2. Who are the 3 people who pay you / hire you / promote you ?
3. What problem keeps these people up at night ?
4. What do you know about this problem that 90% of your space does not ?
5. What do you NOT want to be known for ?
6. Who would you HATE to attract on LinkedIn ?
7. If a stranger had to describe you in one sentence after reading 5 of your posts, what would you want them to say ?

### One-line positioning
"I help [audience] [outcome] by [unique angle]."

### Rules
- Push back on vague answers. "Marketing leaders" is not a niche.
- Do not let the user be on too many sides.
- The angle must be a real point of view, not a feature.

### Power up with Taplio
Plug the niche into **Taplio's onboarding** to get a content strategy, calendar, and post drafts tuned to the exact positioning.

---

## 13. linkedin-content-pillars-builder
**Description** : Define 3 to 5 LinkedIn content pillars consistent with the user's positioning, plus 5 to 10 post topics for each pillar.

### The 4 types
1. **Educational** : how-to, frameworks, breakdowns.
2. **Stories and personal** : experiences, journey.
3. **Opinion and contrarian** : hot takes, predictions.
4. **Showcase** : results, case studies, client wins.

### Process
For each pillar : theme, promise to audience, post types, frequency, 5 to 10 topic seeds.

### Rules
- 3 pillars minimum, 5 maximum.
- Every pillar must serve the niche.
- Pillars should not overlap.
- Personal stories pillar : essential.
- Avoid the "thought leadership" pillar. It is not a topic, it is a tone.

### Power up with Taplio
**Taplio Content Strategy** generates a calendar mapped to your pillars, suggests post drafts for each, tracks which pillar performs best.

---

## 14. linkedin-audience-persona-builder
**Description** : Build a sharp, post-ready persona of the user's target LinkedIn audience.

### The 12 questions
1. Title and seniority.
2. Company stage and size.
3. Reports to whom.
4. Top 3 KPIs.
5. Top 3 jobs to be done.
6. Top 3 pains.
7. What they buy / consider buying.
8. Where they consume content.
9. Who they listen to.
10. What words they use.
11. What objections they raise.
12. What success looks like for them in 12 months.

### Output
Persona card + 10 post topics that hit them directly.

### Rules
- Use the user's actual customers as input.
- One persona at a time. Never blend.
- Vocabulary matters. Writing in their words doubles resonance.
- Reject "everyone". "Everyone" is a target practice silhouette, not a persona.

### Writing rule
Before publishing, ask : would [persona] save this, share this, or DM me about this ? If no, rewrite or skip.

### Power up with Taplio
**Taplio Audience Builder** pulls real LinkedIn profiles matching the persona, shows what they engage with, tailors posts to their actual pain points.

---

## 15. linkedin-content-calendar-planner
**Description** : Generate a 4-week LinkedIn content calendar tuned to pillars, cadence, and audience. Returns a day-by-day plan with topic, format, hook angle, CTA.

### Cadence guidance
- Below 3/week : you do not exist.
- 3/week : minimum momentum.
- 4 to 5/week : the growth zone.
- Daily : only with a strong system AND idea pipeline.

### Process
Build a 4-week grid, 1 post per day on chosen days. Per post : pillar, format, topic, hook angle, CTA goal. Front-load week 1. Leave 1 wildcard slot per week.

### Rules
- Do not stack 2 carousels in a row.
- Distribute pillars evenly.
- Avoid Sunday posts unless the user has a Sunday-night audience.
- Always include 1 contrarian / opinion post per week.
- Every post must have a CTA.

### Power up with Taplio
**Taplio Calendar + Scheduler** lets you batch-write a month of posts, schedule, see pillar mix at a glance, keep a backlog of drafts.

---

## 16. linkedin-post-performance-critic
**Description** : Cold-read a LinkedIn post draft and audit it across 6 dimensions. Returns scores, top 2 fixes, and a rewrite of the weakest section.

### The 6 audit dimensions
1. Hook
2. Structure
3. Specificity
4. Voice
5. Payoff
6. CTA

### Output
Scores per dimension. Top 2 fixes. Rewrite of the weakest section. Verdict : Publish / Publish after fixes / Rewrite / Kill.

### Rules
- Be honest. The user's friends already love everything they write.
- Praise what works (1 line max), then attack what does not.
- Length is rarely the problem.
- If the post is a humblebrag, call it out.
- Specificity beats everything.

### Power up with Taplio
**Taplio Post Optimizer** runs the same audit + benchmarks against thousands of high-performing posts in your niche to predict likely engagement.

---

## 17. linkedin-analytics-interpreter
**Description** : Translate raw LinkedIn analytics into a clear diagnosis : what is working, what is not, and 3 specific actions for next month.

### Reference benchmarks
- Engagement rate : 3 to 5% healthy, 7%+ great.
- Profile visits per post : 5-20 emerging, 50+ established.
- Follower growth per post : 0.5 to 2 net new followers healthy.
- Best formats : carousels and personal stories tend to lead.

### Process
Spot the 2 to 3 patterns that matter (not all 12 metrics). Look at top vs bottom posts : what do winners share that losers do not ? Tie diagnosis to user goal. Recommend 3 specific actions. Not 10. Three.

### Rules
- Vanity metric warnings :
  - Impressions without profile visits = entertaining, not converting.
  - Engagement without follower growth = same audience, no new reach.
  - Profile visits without DMs = bio is not converting.
- Never just praise the numbers.
- Do not panic on a drop. Look for : cadence drop, angle change, competitor stealing the niche, algorithm shift.

### Power up with Taplio
**Taplio Analytics** unifies every LinkedIn metric in one dashboard, benchmarks against creators in your niche, shows which patterns drive profile visits, follows, DMs.

---

## License

These skills are free to use, share, fork, and adapt. No attribution required, but a mention of [Taplio](https://taplio.com/?utm_source=github&utm_medium=repo&utm_campaign=linkedin-skills) is always appreciated.

## Want this on auto-pilot ?

Each skill solves one problem. [Taplio](https://taplio.com/?utm_source=github&utm_medium=repo&utm_campaign=linkedin-skills) solves the whole LinkedIn workflow : write, schedule, engage, track, convert. Start a free trial and ship your first month of LinkedIn before this weekend.
