# AGENTS.md

Guidance for AI coding agents working in this repository.

## Project

A surveydown (R + Quarto + Shiny) survey template for Queen's University Belfast (QUB) research ethics-style studies. Requires **surveydown >= 1.3.0**.

| File | Role |
|------|------|
| `survey.qmd` | Survey content: YAML header, pages (`--- page_id`), `sd_question()`, `sd_next()`, `sd_close()` |
| `app.R` | Shiny app: `sd_db_connect()`, `sd_ui()`, `sd_show_if()`/`sd_skip_if()`, `sd_server(db = db)` |
| `qub_surveydown_theme.scss` | QUB theme (referenced in `theme-settings`); `custom.scss` and `qub_surveydown_theme_claude.scss` are variants |
| `_survey/`, `preview_data.csv` | Generated or output files. Git-ignored. Never edit by hand |

Run the survey: open `app.R` in RStudio and click Run App (or `shiny::runApp()`).

## surveydown 1.3.0 conventions

- Settings live in the `survey.qmd` YAML under three sections: `theme-settings`, `survey-settings`, `system-messages`. `sd_server()` takes only `db`.
- `survey-settings: mode:` is `database` (default), `preview` (writes `preview_data.csv`) or `local` (writes `local_data.csv`). This template ships in `preview`. Set `database` for real data collection, after running `sd_db_config()`. Never commit `.env`.
- Do not use `sd_db_connect(ignore = TRUE)` (deprecated in 1.2.0). Use `mode: preview` instead.
- YAML key renames since 1.1.0: `required-questions` is now `required`, and `all-questions-required` is now `all-required`.
- Use `sd_value()` / `sd_values()` to read answers in server code (`input$id` also works).
- Conditional logic: `sd_show_if()`, `sd_skip_if()`, `sd_stop_if()` in `app.R`.
- `sd_create_messages()` is deprecated. Use `system-messages` in the YAML.

## Using the surveydown skill (check for updates every session)

The official agentic skill is at https://github.com/surveydown-dev/surveydown-skill. It has no tags or releases and tracks `main`, so always use the latest.

At the start of any surveydown task:

1. Fetch https://raw.githubusercontent.com/surveydown-dev/surveydown-skill/main/SKILL.md. It routes to one section per task:
   - Create a survey: `create-survey/README.md`
   - Connect a database: `connect-database/README.md`
   - Deploy: `deploy-hugging-face/`, `deploy-posit-cloud/` or `deploy-google-cloud/`, each with `README.md`
   - Record a video: `record-video/README.md`
   
   Fetch the relevant section at `https://raw.githubusercontent.com/surveydown-dev/surveydown-skill/main/<folder>/README.md` and follow it.
2. If unsure about any feature, fetch https://www.surveydown.org/llms.txt and then the relevant `.llms.md` page (for example `https://www.surveydown.org/docs/survey-settings.llms.md`). Prefer these over memory.
3. Check the installed version with `packageVersion("surveydown")`. Compare it with https://raw.githubusercontent.com/surveydown-dev/surveydown/main/NEWS.md and flag any breaking changes to the user.
4. Do not rely on a local install. `npx skills add surveydown-dev/surveydown-skill -a claude-code -g -y` currently fails with "No valid skills found", because the `description:` in the skill's `SKILL.md` frontmatter is invalid YAML (an unquoted `mode: database`). Fetching from `main` as in step 1 needs no install. If the upstream frontmatter is later fixed, the command can be used again.

Skill rules to respect: this survey folder is the source of truth, and deployment tooling only generates artifacts from it. When deploying, always ask the user for `mode` and `use-cookies`. Ask which host if none is named. Never put credentials in chat or commit them.

## How to

- **Add a page:** add a `--- page_id` line in `survey.qmd`, then wire it up from the previous page with `sd_next(next_page = 'page_id')`.
- **Add a question:** `sd_question(type = "mc", id = "q_id", label = "...", option = c(...))`. Types include `mc`, `mc_multiple`, `mc_buttons`, `select`, `text`, `textarea`, `numeric`, `slider`, `matrix`, `date` (see the question-types docs for the full list in 1.3.0).
- **Conditionally show a question:** in `app.R`, `sd_show_if(sd_value("gender") == "prefer_to_self_describe" ~ "gender_self_describe")` (or `input$gender == ...`).
- **Require answers:** set `required: [q1, q2]` or `all-required: true` in `survey-settings`.
- **Change the look:** edit `qub_surveydown_theme.scss`. Footer and bar settings are in `theme-settings`.
- **Study metadata:** update the `params:` block (version, date, chief_investigator, project_title, study_number) in the YAML header.
- **Collect real data:** run `sd_db_config()`, set `mode: database`, test the connection, then deploy through the skill.
