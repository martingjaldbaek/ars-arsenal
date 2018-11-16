# CHANGELOG

## 3.0.0

This release adds pagination to ArsArsenal. In the process of doing this, we've made some breaking changes to the way URLs are constructed. For most users, this upgrade process should be minimal:

### `makeURL` is now `listUrl` and `showUrl`

The old `makeURL` option relied on a null check to determine if the requested url is for a list of items or a single record. To avoid that ambiguity, there are now two endpoints for URL construction:

Instead of:

```javascript
function makeURL(url, slug) {
  if (slug == null) {
    return url
  } else {
    return `${url}/${slug}`
  }
}

ArsArsenal.render({ makeURL })
```

Change this to:

```javascript
let listUrl = url => url
let showUrl = (url, slug) => `${url}/${slug}`

ArsArsenal.render({ listUrl, showUrl })
```

**These are the default implementations of each option.** We anticipate
that this change affects very few users.

### `makeQuery` is now `buildQuery` and returns an object

With pagination, ArsArsenal must now manage multiple query
parameters. For improved ergonomics, ArsArsenal now builds the query
string on behalf of the user. Instead of returning a string, return an object of key/value pairs:

Instead of:

```javascript
function makeQuery(term) {
  return `q=${term}`
}

ArsArsenal.render({ makeQuery })
```

Return an object:

```javascript
const PAGE_SIZE = 10

function makeQuery({ page, search }) {
  // Return your pagination/search query implementation:
  let offset = page * PAGE_SIZE
  let limit = offset + PAGE_SIZE

  return { q: search, offset, limit }
}
```

## 2.5.1

- Handle `null` in captions and titles

## 2.5.0

- Export project as CommonJS module for better support

## 2.4.5

- Visual updates based on testing in a few apps

## 2.4.4

- Fix bad method reference

## 2.4.3

- Fix another case where indexOf check failed on null selection in the
  TableView

## 2.4.2

- Fix case where indexOf check failed on null selection

## 2.4.1

- Fix a style issue with selection clearing on mobile

## 2.4.0

- Add the ability to what table columns display

## 2.3.0

- Added a table view
- Significant animation and aesthetic improvements

## 2.2.0

- Do not fetch when given a `NaN` slug

## 2.1.1

- Remove PropType to avoid unexpected warning in <SelectionFigure />

## 2.1.0

- Upgrade react-focus-trap dependency

## 2.0.0

- Upgrade dependencies
- Remove peer dependency on React
- Remove deprecation warnings in React 15.x

## 1.0.0

- **Important Update**: This update makes breaking changes to support
  React 0.14. ars-arsenal now takes advantage of
  `react-addons-css-transition-group` and utilizes `react-dom` for rendering.

## 0.4.2

- Adds the ability to clear existing image selections.
- Adds a `resource` option for customizing file type language. For changing the "Photos" reference in "Pick a photo" selection text:

  - Setting `resource` to "File" renders "Pick a file"
  - Setting `resource` to "Image" renders "Pick an image"

- Updates basic selection styles

  - Centers the selection text, re-positions icons to reflect the loading state within the selection button.
  - Adds "Loading" text while a selected image is fetching.
  - Applies the "loaded" class to the image shortly after onload for image-to-image transitions.
  - Adds an explicit `-webkit-transition` to workaround autoprefixr not generating `-webkit-filter` as a transition property.

- Resets `isLoaded` state when the image re-renders with a different src.

## 0.4.1

### Noticeable Changes

- Updated style
- Added multiselection option, see https://github.com/vigetlabs/ars-arsenal/pull/14
- Fixed an issue with Picker `onExit` where "Cancel" clicks could bubble and immediately re-open the Picker dialog

### Upgrading

- The style folder for ars-arsenal is now placed within `ars-arsenal/style`. For those on 0.3.0, you will need to change this path.
