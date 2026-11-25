# Microcopy Voice Lint
A voice agent that reviews UI text against plain-language rules. 

## Demo
Coming soon

## How it works
1. The agent greets you and asks for a UI string.
2. User says UI string.
3. Agent:
   - repeats back the string
   - names the rules it breaks
   - suggests one rewrite

## Why voice changes the rules
- Capitalization and punctuation can't be heard, so agent skips those traditional style guide rules.
- Answers have to be short and plain, with no lists or formatting.
- It confirms the UI string because speech recognition makes mistakes.

## Rules
This agent runs as a companion to the [Microcopy Assistant](https://microcopy-assistant.vercel.app/) and the [GitHub repo](https://github.com/jlogan839-forge/microcopy-assistant). Access code available upon request. 
- Use active voice.
- Avoid "click here." Say what the action does.
- Don't use please in instructions.
- Avoid vague labels like "View" or "Submit", name the action.
- Keep it short, cut words that don't add meaning.
- Use plain words, not jargon.
- Avoid blaming the user in error scenarios.
- Keep all text as action-oriented unless supplemental information is required to help the user move to the next step or to provide reassurance.
- Always use contractions.
- Keep toast messages short, usually 5 words or fewer.
- For empty state scenarios, focus on the purpose of the empty state without negative wording.

## Run it yourself

### Requirements
- Python
- uv
- LiveKit CLI
- LiveKit Cloud Account

### Setup
1. Clone the repo: `git clone https://github.com/jlogan839-forge/microcopy-voice-lint.git`
2. Install dependencies: `uv sync`
3. Copy `.env.example` to `.env.local` and add your LiveKit keys.
4. Run locally: `uv run python src/agent.py console`.
4. Deploy: `lk agent deploy`


## Testing
Strings tested:
- Click here to continue
   - Flagged 'click here'
   - Suggested 'Continue'
   - PASS
- Your payment was declined
   - Flagged blaming the user
   - Suggested "Payment couldn't be processed"
   - Didn't flag passive voice
   - Partial PASS
- Please enter your password
   - Flagged 'please'
   - Suggested "Enter your password"
   - PASS
- Your changes were saved by the system
   - Flagged length
   - Suggested "Changes saved"
   - Didn't flag passive voice
   - Partial PASS
- Save changes
   - Noted that it didn't break any rules
   - Said the string was "okay"
   - PASS

Notes: The assistant missed flagging passive voice in two cases, so next I'd test whether listing rules in priority order changes this. 

## What I learned
I used the Python starter template in code to build and configure the agent.
- The deploy failed without `uv.lock`. Running `uv sync` first fixed it.
- A failed `lk agent create` still used the free plan's one agent slot, so I had to use `lk agent deploy` instead.

## What's next
- I want to load rules from the Microcopy Assistant JSON file so the Microcopy Assistant and the Microcopy Voice Lint can be synced.
- I also want to build a Microcopy/Content Dashboard with a curated selection of microcopy and documentation tools.
