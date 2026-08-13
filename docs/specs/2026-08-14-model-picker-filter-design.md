# Model Picker Filtering Design

## Problem

The composer model dropdown can contain too many runtime models to scan efficiently. ompweb already supports a persistent shortlist under **Settings → Models → Composer model picker**, but the dropdown has no temporary text filter.

## Decision

Keep the persistent shortlist and add a search input to the existing composer model dropdown. This avoids changing OMP's model registry or replacing the working picker.

## Behavior

- The search input appears at the top of the open model dropdown.
- Matching is case-insensitive and uses a plain substring across provider, display name, and model ID.
- Search runs only against models allowed by the persistent composer shortlist.
- Existing provider grouping and active-model highlighting remain unchanged.
- When the shortlist contains models but none match, the dropdown shows **No matching models**.
- Closing the dropdown clears the query. Reopening shows the full shortlist.
- Selecting a filtered model keeps the existing model-change behavior.

## Implementation Boundary

The change stays in the composer picker. It adds client-side query state and derives filtered provider groups from the existing `modelOptions` in `components/ChatInput.tsx`.

No API, RPC, model-registry, Settings persistence, or new dependency changes are required.

## Verification

- A focused test covers case-insensitive matching by provider, display name, and model ID, including the no-match result.
- A browser smoke check confirms typing filters the visible groups, an empty result shows the message, selecting a result changes the model, and closing/reopening clears the query.

## Non-goals

- Fuzzy matching or ranking
- Persisting the search query
- Replacing the picker with `cmdk`
- Changing the persistent composer shortlist
- Changing which models OMP exposes
