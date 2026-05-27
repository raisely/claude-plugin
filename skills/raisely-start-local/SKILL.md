---
name: raisely-start-local
description: Start a local environment of your Raisely Campaign to preview changes
---

# Start local Raisely server

## Considerations

- Never assume or guess which campaign to use. If the user hasn't named a campaign in the current session, stop and ask. Do not infer it from recent files, git history, or `raisely list` output.
- `.raisely.json` is the one allowed hint: if it's present, you may propose its campaign UUID to the user, but you must confirm with them before starting. Don't start until they say yes.
- The default port is 8015. Different ports can be used with the `--port` argument when running `raisely local`
- Do not update the campaign with `raisely update` unless explicitly requested by the user
- If you have access to an embedded browser, use the argument `--no-open` and open the campaign in the embedded browser. Otherwise, do not use `--no-open` and let the command open the URL in the default browser.
- When debug mode is enabled, state the skill name and every command with arguments before running them.
- If you used the embedded browser, after opening the URL, confirm the page is reachable and the campaign slug matches the requested campaign.

## Steps

1. Confirm which campaign to start. If the user already named one in this session, use that. Otherwise, if `.raisely.json` exists, show its campaign UUID (and slug/name if available) and ask the user to confirm before continuing. If neither applies, ask the user which campaign to start and wait for their answer.
2. Verify `raisely` is available using the `raisely-ensure-cli` skill
3. Verify if port 8015 is free. If not, keep incrementing the port by one (8016, 8017, 8018, 8019...) until you find one available. Do not stop existing process unless the user ask for it
4. Use `raisely list --json` to find the uuid of the campaign the user named. If there is no .raisely.json or the uuid doesn't exist there, then run `raisely init --uuid` with that campaign's UUID
5. Run `raisely local` as a background task. Make sure to pass the right parameters based on the current task
6. If the process ends due to an error, notify the user and try to start it again following the previous steps
