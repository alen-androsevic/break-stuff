```markdown
# Break the UI: <target>

Seed: <seed> · Attacks: <categories> · Surface: <screens/flows>
Grid: <N> rows · Breaks: <B> · Held: <H> · n/a: <X>

## Breaks

### [<severity>] <one-line title>
- **Attack**: <category / specific attack>
- **Where**: <screen> → <element>
- **Steps**: 1. … 2. … 3. …
- **Expected**: …
- **Actual**: …
- **Oracle**: <which oracle failed>
- **Evidence**: <screenshot path>, <log excerpt>
- **Suspected cause**: <optional, only if visible from logs or code>

## Held
<grid rows that survived, grouped by attack category>

## n/a
<rows skipped, each with a reason>
```
