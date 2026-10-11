# Agent Benchmark Plan: Three.js Lake and Boat

## Goal

Compare four coding agents—Pi native, customized Pi, OpenCode, and Codex—on the same bounded frontend task. Run two distinct tracks: first hold the LLM model and effort constant to compare agents; then vary model and effort to compare configurations. Measure completion time, functional quality, and visual quality separately. This benchmark evaluates agent workflows and model/effort configurations without conflating the two.

## Benchmark challenge

Each agent receives the same prompt and an identical clean starter repository. It must produce a small, runnable browser app using **Three.js only** (no Phaser) that contains:

- A lake with visibly animated water.
- A boat that the user can steer with the keyboard.
- A camera that follows the boat without making it hard to control.
- On-screen control instructions and a way to return to the initial state.
- A short README with install/run instructions and known limitations.

Keep the brief implementation-focused: agents may choose their own visual style and internal architecture, but must not add unrelated features. The exact prompt should be frozen before the first run and reused verbatim.

## Fair-run protocol

1. **Freeze the environment.** Record operating system, hardware, runtime/package-manager versions, network conditions, repository commit, and the exact starter files. Make a fresh identical copy for every run.
2. **Freeze model settings.** In the agent-comparison track, use the same exact model ID, provider where possible, reasoning/effort setting, temperature (if exposed), context/tool permissions, and budget/limits. In the model/effort track, vary only the planned model and effort factors while keeping the other settings fixed. Record the resolved backend model ID and settings (not just a UI alias), and report settings that cannot be made identical rather than implying equivalence.
3. **Freeze agent versions.** Record Pi/native and customized Pi versions/configuration, OpenCode version, Codex version, enabled tools, and any custom instructions. Only the agent product/configuration should vary.
4. **Freeze the task.** Send the same prompt, once, to each agent. Do not give one agent extra hints. If a run requires intervention, log the exact intervention and elapsed time; decide in advance whether to stop the run or count it as an assisted run.
5. **Measure time consistently.** Start when the prompt is submitted. Stop when the agent says it is done and the app is ready to run. Record wall-clock time, active interaction time if measurable, and any setup/dependency wait separately. Do not compare raw token-generation time from one tool with wall-clock time from another.
6. **Preserve outputs.** Save each final repository snapshot, agent transcript, settings, run log, and screenshots/video under a run ID. Do not let later agents see earlier outputs.
7. **Repeat.** Run at least three independent trials per agent if practical; randomize agent order to reduce order, fatigue, and environment effects. Report median and range, not only the best result.

## Model and effort experiment

Keep the agent-only comparison separate from this track. Begin with one agent and the same task/environment to pilot the matrix; later, if time allows, repeat selected configurations across all four agents. Use exact provider model IDs, since product names and free-tier availability can change.

| Role | Candidate configuration | Effort plan | Notes |
| --- | --- | --- | --- |
| Reference / control | OpenAI GPT-6 Luna | High (user-selected baseline), if exposed by the chosen agent/provider | The user specified Luna at high effort as the sure baseline. Record the actual model ID and whether the adapter maps `high` exactly. |
| OpenAI comparison | OpenAI GPT-6.1 Sol | High plus other effort levels supported by every compared route (for example, medium) | Keep this distinct from GPT-6 Luna; do not assume the effort labels or availability match. |
| OpenRouter paid candidate | DeepSeek V4.1 Flash | Record supported reasoning/effort controls; do not invent an effort setting if unavailable | This is the closest current catalog match to “DeepSeek Flash 4.1”; it is not shown as free in the current catalog. |
| OpenRouter free candidate | DeepSeek V4 Flash 0731 (`deepseek/deepseek-v4-flash-0731:free`) | Record supported controls; compare only settings actually exposed | Treat this as a separate free model, not as the free version of V4.1 Flash. Free endpoint availability/rate limits can change. |

For effort comparisons, choose common levels that the route actually supports (e.g. low/medium/high where available). Do not compare unlike labels as if they were equivalent. Save the model ID, provider/route, effort value, and any fallback/resolved model for every run. If the desired model or effort cannot be used through one agent, mark that cell unavailable; do not silently substitute another model. Report latency, intervention count, quality scores, and cost/usage separately by model × effort × agent. Use repeated trials where practical and randomize run order.

Current catalog references (recheck before scored runs): [OpenAI model catalog](https://developers.openai.com/api/docs/models), [OpenRouter DeepSeek V4.1 Flash](https://openrouter.ai/deepseek/deepseek-v4.1-flash), and [OpenRouter DeepSeek V4 Flash 0731 free](https://openrouter.ai/deepseek/deepseek-v4-flash-0731:free). Availability and identifiers can change.

## Evaluation

### Functional score (0–5 each)

- App starts using the documented commands.
- Water motion is visible and stable.
- Boat is visible and remains convincingly on the lake.
- Keyboard controls move and turn the boat responsively.
- Camera follows usefully; controls and reset instructions work.
- No major runtime errors during a short play session.

### Visual score (1–5 each, human-rated)

- Coherent composition and readable scene.
- Water, boat, lighting, and palette feel intentional.
- Motion looks smooth and reinforces the sense of a lake.
- Boat movement and camera motion feel believable.
- Overall polish and aesthetic appeal.

Use the same rubric and, ideally, at least two raters who do not know which agent produced each anonymized build. Capture screenshots from the same viewport and record a short identical interaction for every run. Keep aesthetic ratings distinct from functional scores.

### Report per run and configuration

Group results by agent × model × effort; summarize the agent-only track separately.

- Model/settings and agent/tool versions.
- Median completion time and trial range.
- Functional score and visual score (with per-category results).
- Number/type of human interventions.
- Whether the app ran successfully and notable defects.
- Notes on setup friction and limitations of the comparison.

## Suggested project milestones

1. **Benchmark specification:** finalize the exact prompt, starter repository, model/tool settings, timing rules, and scoring sheet.
2. **Starter app:** build and validate one reference Three.js lake-and-boat task, with a clean resettable starter state shared by all runs.
3. **Run capture:** establish a repeatable folder/template for transcripts, environment metadata, screenshots, and timing/intervention logs.
4. **Pilot:** perform one trial per agent, identify ambiguities or unfair conditions, then freeze the protocol before scored runs.
5. **Evaluation:** execute repeated randomized trials, score anonymized outputs, and publish a concise results table with limitations.

## Acceptance criteria for the benchmark setup

- All agents receive the identical frozen prompt and starter commit.
- A fresh working copy is used for each trial.
- Model identity/settings and agent versions are recorded for every run.
- Timing start/stop rules and intervention handling are written down before scored trials.
- Each output can be launched and evaluated with the same functional and visual rubric.
- Results include repeated trials or explicitly disclose when only a pilot/single run was possible.

## Non-goals for the first benchmark

- Automatically ranking agents from a single run.
- Claiming a universal winner from subjective visual ratings.
- Claiming universal or intrinsic model quality from this small, task-specific benchmark; model results describe only the tested configurations and conditions.
- Building an automated harness that launches all four agents. This can be a later project phase after the manual protocol is validated.
