# Pressure Scenario: Portable Visual Companion Script Path

## Failure Mode

The visual companion instructions show a maintainer-specific absolute path such as `/Users/<maintainer>/...`, and an agent copies that path when running on another machine or plugin version.

## Scenario

User asks for visual brainstorming in Codex. The installed skill path differs from the maintainer's local cache path.

## Expected Behavior

The agent resolves the script path from the currently installed brainstorming skill directory surfaced by the active skill list or plugin runtime, then runs that session-local path. The instructions must not contain a real maintainer home directory.

## Regression Check

Search shipped skill docs for a real maintainer home directory. The result must be empty.
