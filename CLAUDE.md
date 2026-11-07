# Masaraat adaptive practice app (Flutter)

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

### 2. Removal test (line → mistake it prevents)

1. "flutter_bloc only": the agent would mix Provider or Riverpod into one feature, leaving two state patterns in the same screen.
2. "Blocs never call the network": the agent would put HTTP calls inside a Bloc, which makes it impossible to unit test without a real server.
3. "Colors and spacing from theme.dart": the agent would hardcode hex colors and pixel values on every new screen, so the app drifts from the design.
4. "All UI text is Arabic from l10n": the agent would write English string literals directly in widgets, so Masaraat's Arabic-speaking students see English text.
5. "EdgeInsetsDirectional, never left/right": the agent would use EdgeInsets.only(left: ...), and the layout breaks in right-to-left Arabic screens.
6. "Client-generated unique ID per answer": after a dropped connection, a retried sync would store the same answer twice and corrupt the student's mastery estimate.
7. "Every network call needs a timeout and offline state": the agent would write a request with no timeout, and the screen would spin forever on weak mobile data.
8. "const widgets and ListView.builder": the agent would build long question lists with a plain ListView, which lags on low-end Android phones.
9. "Never log names, emails or phone numbers": the agent would add debug prints of the student object, leaking data of minors into logs.
10. "Fake students only in test/fixtures/": the agent would copy a real row from the answer extract into a test, putting real student data into the repository.

### 3. Left out on purpose

1. Generic advice such as "write clean, well-documented code": the model already does this, so removing it changes nothing.
2. A full list of API endpoints: it is too long and will change, so the agent should read the API contract file when needed. (We will add one pointer line once the contract exists.)
3. Sprint goals and current tickets: they change every two weeks, so they belong in the ticket, not in a file read at every session.

### 4. Experiment

**Task (from the squad backlog):** build the first piece of the offline answer flow.

**Checks, written before running anything:**
- C1: Uses flutter_bloc (Bloc or Cubit) and no other state-management package.
- C2: Every answer gets a client-generated unique ID before it is saved.
- C3: The Bloc calls a repository and does not call the network directly.

**Prompt (identical in both runs, each in a fresh session):**
```
Create an AnswerBloc for a Flutter app where a student answers a multiple
choice question. Save the answer locally first, then sync it to the server
when the connection is available. Include the states and events.
```

**Output without the rules file (relevant part, max 40 lines):**
```
[PASTE RUN 1 OUTPUT HERE]
```

**Output with the rules file pasted at the top (relevant part, max 40 lines):**
```
[PASTE RUN 2 OUTPUT HERE]
```

**Check table (fill honestly):**

| Check | Without file | With file |
|---|---|---|
| C1: flutter_bloc only | [pass/fail] | [pass/fail] |
| C2: client-generated unique ID | [pass/fail] | [pass/fail] |
| C3: Bloc uses a repository | [pass/fail] | [pass/fail] |

### 5. Conclusion and one change

[Max 100 words. State what the table showed, including "no difference" if that is what happened. Then name one specific change, for example: "I will add a line naming the local storage package (drift or Hive), because the agent chose one on its own in both runs."]
