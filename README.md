# Study_Pilot
AI-powered study assistant that analyzes student performance and provides personalized study recommendations.
# StudyPilot

StudyPilot looks at a student's quiz and assignment scores, figures out which topics they're actually weak in, and tells them what to study first.

Course project for CAI5009 (AI in Applications), Fall 2026.

## Team

- Lakshmi Samhitha Marella
- Tiffany Stickings
- Garet Hufnagle

## Why we're building this

Most study apps we looked at (Quizlet, StudyFetch, etc.) give you flashcards and quizzes, but they don't look at how you're actually doing and adjust. You still have to decide for yourself what needs work.

StudyPilot starts from the score data instead. You put in quiz and assignment results tagged by topic, and the app ranks your topics from weakest to strongest, labels them, and generates a short explanation of why a topic got flagged and what to review first. Later on it should also generate practice questions for the weak topics and build a week-by-week study plan.

## Current status

Week 5/6 planning stage. **There is nothing to run yet.** This repo currently holds the planning document and will hold the dummy dataset, prompt templates, and eventually the export/config of the app itself.

The app is being built in Base44 (no-code), so the deployed version will be a shareable link rather than something you clone and `npm install`. That link goes here once we publish it in Week 13.

## MVP scope

The MVP is the end-to-end loop, nothing more:

dummy student records go in → analysis engine aggregates scores by topic → topics ranked weakest to strongest → dashboard shows the ranked list with strong / inconsistent / struggling labels → clicking a topic shows an AI-generated explanation and what to review first.

We consider the MVP done when all of our dummy profiles run through that whole loop with no manual steps in the middle.

**Must have**
- Performance analysis by topic
- Topic ranking (weakest to strongest)
- Plain-language study recommendations
- Dashboard UI with strong / inconsistent / struggling labels

**Should have (after the MVP loop works)**
- AI-generated practice questions for the weakest topics
- Course material upload and linking to topics
- Auto-generated study plan / calendar

**Could have**
- Canvas LMS integration to pull grades automatically
- Condensed study notes from uploaded material

Nothing in Should/Could gets started until the MVP loop passes its end-to-end test. That's a deliberate rule, not just a preference, because scope creep is the risk we're most worried about.

## How it fits together

- **UI** — Base44 frontend: data entry/upload screen, dashboard, topic detail view.
- **Orchestration** — Base44 workflow logic. Validates input, runs the scoring engine, builds the prompt, calls the LLM, formats the response for the UI.
- **Analysis engine** — plain math (averages, variance), no LLM. This is on purpose. The ranking has to be the same every time you run it.
- **LLM** — an API model (OpenAI / Claude / Gemini) used only for the explanation and recommendation text. It does not decide the rankings.
- **Prompts** — two templates for the MVP (performance summary, recommendation). A third for practice questions comes later.
- **Data** — synthetic quiz and assignment records for the whole semester. Uploaded course material and Canvas data are stretch scope.
- **Storage / logging** — Base44's built-in database, plus a timestamped log of every LLM call (prompt, response, latency). The evaluation work runs off that log.

## How we're evaluating it

Three things, all against dummy data:

1. **Accuracy** — we wrote a ground-truth answer key for each dummy student (which topics are actually their weak ones) before building anything, and the AI pipeline never sees it. We compare the system's ranked weak topics against that key.
2. **Latency** — logged per recommendation, at least 20 runs, reporting average and worst case.
3. **Consistency** — same profile run three times, outputs compared, contradictions documented.

## Responsible AI

- The app only uses synthetic data this semester. No real student records, no FERPA-covered data. If this ever went past a class project, real grade data would need a proper handling plan first.
- The recommendation is guidance, not a grade. That disclaimer is visible on the dashboard and on the topic detail view.
- A student can dismiss or edit any recommendation. The app shouldn't be the final word on what someone is bad at.

## Repo setup

- `main` stays deployable.
- Everyone works on their own feature branch and opens a PR before merging. The PR is also where we leave comments on each other's work.

Planning document (MVP scope, full task breakdown with owners and effort, Week 5–15 roadmap, risk register): `docs/CAI5009_TechTroop_Map.docx`

## Roadmap at a glance

| Week | What |
|---|---|
| 6 | Planning doc submitted, MVP scope locked, Base44 project set up |
| 7–9 | Dummy data, analysis logic, LLM wired in, dashboard built |
| 9–10 | Internal MVP demo, full loop working |
| 10–12 | Should-have features, testing, evaluation |
| 12–14 | Canvas stretch goal (hard cutoff at Week 14, dropped if it isn't working) |
| 13 | Deployed to a shareable link |
| 14–15 | Report, slides, demo rehearsal |
| 15 | Final presentation |
