# Masaraat adaptive practice app (Flutter)
Planned structure: the project is at its start; the paths below are the agreed layout.

## Versions
- Flutter 3.47.9 stable, Dart 3.13.7. Do not use APIs newer than these.
- State management: flutter_bloc only. Never add Provider or Riverpod.

## Commands
- Tests: `flutter test`
- Lint: `flutter analyze` (must be clean before any PR)
- Format: `dart format .`
- Low-end device build: `flutter build apk --release`

## Architecture
- One feature per folder: lib/features/<name>/{bloc,data,ui}/
- Blocs never call the network; they call a repository in data/.
- Colors and spacing come from lib/core/theme.dart. No hardcoded values.

## Conventions that differ from defaults
- All UI text is Arabic, from l10n files. No hardcoded strings.
- Use EdgeInsetsDirectional and AlignmentDirectional, never left/right.
- Save every answer locally first, then sync. Each answer gets a
  client-generated unique ID so a retry never creates a duplicate.

## Known traps
- Mobile data drops often: every network call needs a timeout and an offline state.
- Target phones are low-end Android: use const widgets and ListView.builder.
- Most users are under 18: never log or store names, emails or phone numbers.

## Before saying a task is done
- `flutter analyze` and `flutter test` pass.
- Every new Bloc has a bloc_test covering loading, success and offline failure.
- Test data comes from test/fixtures/ with fake students only.
```
