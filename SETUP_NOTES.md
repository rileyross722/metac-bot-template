# FutureEval Bot — Setup Notes (this project)

Scaffolded from the official Metaculus template (https://github.com/Metaculus/metac-bot-template).
Dependencies are already installed into `.venv` (Python 3.11). `forecasting-tools==0.3.4` is
already pointed at the live **Fall 2026** tournaments by default (`CURRENT_AI_COMPETITION_ID`,
`CURRENT_MINIBENCH_ID = "minibench"`), so no tournament-ID config is needed.

## What's done
- Cloned the template into `futureeval-bot/`
- Created `.venv` and installed all dependencies (`forecasting-tools`, `asknews`, `openai`, etc.)
- Copied `.env.template` -> `.env` (still has placeholder values — see below)
- Verified `main.py` compiles cleanly (syntax-checked, not executed — needs real API keys to run)

## What you need to do (I can't create accounts or submit forms on your behalf)

1. **Metaculus account + bot token**
   - Sign up at metaculus.com, go to Settings -> "My Forecasting Bots" -> "Create a Bot"
   - Copy the bot's API token into `.env` as `METACULUS_TOKEN`

2. **Required Participation Form** (3 questions) — fill out via the link on the
   [resources page](https://www.metaculus.com/notebooks/38928/futureeval-resources-page/)

3. **Free LLM credits** — fill out the LLM-credits request form (same resources page,
   "Donated LLM credits" section) to get an `OPENROUTER_API_KEY` funded by OpenAI/Anthropic/Google
   credits. Put it in `.env`.

4. **AskNews (free research/search API)**
   - Create an account at `my.asknews.app` using the **same email** as your Metaculus bot
   - Message `@freqai` on the AskNews Discord or email contact@asknews.app with your bot name,
     registered email, name, LinkedIn, and affiliation to get free call credits activated
   - Generate `ASKNEWS_CLIENT_ID` / `ASKNEWS_SECRET` and add to `.env`

5. **Payout check** — confirmed: India is listed as a supported destination for Ramp
   international transfers (both SWIFT USD and local INR/FX rails), so the prize payout channel
   should work. (One narrow Ramp caveat: INR transfers are restricted for non-profit *organizations*
   as the paying customer — shouldn't affect an individual prize recipient, but flagging it since
   it's the one ambiguous line in their docs.)

## Recommended first move: MiniBench, not the Seasonal tournament

MiniBench is explicitly designed to lower the barrier to entry: 2-week rounds, ~60
auto-generated questions, fast feedback. The Seasonal tournament (~$50k, 4-month season) is where
the big prize money is, but `main.py`'s default `tournament` mode already forecasts on **both**
simultaneously (`CURRENT_AI_COMPETITION_ID` + `CURRENT_MINIBENCH_ID`), so there's no need to choose
— running the bot once you have keys enters you in both automatically.

Prize money (both tournaments) is only paid out 3x/year on the seasonal cycle, not per MiniBench
round — MiniBench is fast *feedback*, not fast *payout*. Keep that expectation calibrated.

## Running it once keys are set

```bash
# from futureeval-bot/, with .venv active and .env filled in
.venv/Scripts/python.exe main.py --mode test_questions   # sanity check on 4 example questions, no submission effect on real score
.venv/Scripts/python.exe main.py --mode tournament        # forecasts on live Seasonal + MiniBench questions, submits for real
```

`test_questions` mode and the Metaculus Cup/MiniBench testing area let you iterate without
touching your live score — use those first.

## Ideas for improving the bot beyond the template baseline
See the bottom of `README.md` ("Ideas for bot improvements") — notable low-effort wins:
calibration/extremizing past forecasts, running multiple LLMs and aggregating, a dedicated
base-rate researcher. Worth revisiting once the baseline bot is confirmed working end-to-end.
