# scoop-narrate

A [Scoop](https://scoop.sh) bucket for [narrate](https://github.com/felores/narrate)
— the provider-agnostic TTS gateway and CLI for AI coding harnesses.

This is the Windows install path, mirroring the macOS Homebrew tap
[`felores/homebrew-narrate`](https://github.com/felores/homebrew-narrate).

## Install

```powershell
# Install Scoop first if you don't have it:
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
irm get.scoop.sh | iex

# Add this bucket and install narrate:
scoop bucket add narrate https://github.com/felores/scoop-narrate
scoop install narrate

# Start the server, then speak:
narrate-server
narrate "hello from Windows"
```

`bun` is pulled in automatically as a dependency. The **system provider** uses
Windows SAPI (`System.Speech.Synthesis`) out of the box — no API key needed,
works offline. For premium voices add keys to `%USERPROFILE%\.env`
(`ELEVENLABS_API_KEY`, `OPENAI_API_KEY`, `GEMINI_API_KEY`, `XAI_API_KEY`).

## Run at login

Windows has no `brew services`. Use Task Scheduler:

```powershell
$action  = New-ScheduledTaskAction -Execute "narrate-server"
$trigger = New-ScheduledTaskTrigger -AtLogOn
Register-ScheduledTask -TaskName "narrate" -Action $action -Trigger $trigger -Description "narrate TTS server"
```

## Update

```powershell
scoop update narrate
```

## Manifest

The manifest lives at [`bucket/narrate.json`](bucket/narrate.json). It depends on
`bun`, downloads the tagged release tarball, runs `bun install`, and generates
`narrate.cmd` / `narrate-server.cmd` wrappers that `bun run` the CLI and server.

The canonical copy is maintained in the main repo at
[`packaging/scoop/narrate.json`](https://github.com/felores/narrate/blob/main/packaging/scoop/narrate.json);
on each release it is copied here and pushed.

## License

MIT — see the [narrate repo](https://github.com/felores/narrate).
