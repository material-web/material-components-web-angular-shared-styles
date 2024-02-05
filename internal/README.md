# Material Sass

How we write Sass for M3.

[TOC]

## References

*   [Sass refactoring project doc](http://goto.google.com/material-wiz-sass-refactoring-plan)
*   [Sass simplification & modernization design doc](http://go/material-web-sass-simplification)

## Guidance

Our Sass files should follow this guidance.

### Visibility

Keep visibility scoped to just the directories that need access.

```bazel {.good}
sass_library(
  name = "sass",
  visibility = [
    "//third_party/javascript/angular_components/src/material/checkbox:__subpackages__",
  ],
)
```

### Importing dependencies

Use relative file paths when importing files within the same or child
directories. Otherwise, use full file paths.

#### Example

*javascript/materialdesign/gm3/wiz/button/button_elevated.scss*

```scss {.good}
@use 'third_party/javascript/material/m3/sass/internal/theme';
@use 'third_party/javascript/material/m3/sass/tokens/v0_161' as tokens;
@use './button_elevated';
@use './button_shared';
```

```scss {.bad}
@use 'third_party/javascript/material/m3/sass/button/button_elevated';
@use 'third_party/javascript/material/m3/sass/button/button_shared';
@use '../../../../src/tokens/v0_161' as tokens;
@use '../sass/theme';
```

### Shared component files

Shared component files should only define styles that are truly shared across
all variants (aka the
"[intersection](https://en.wikipedia.org/wiki/Intersection)").

Note: If your shared file contains control flow to *sometimes* emit styles, you
should move those styles into the appropriate variant files. This may result in
some duplication.

<img alt="The intersection of two sets" src="https://docs.google.com/drawings/d/16wZWdQA2xBYgmgNIhKlfCUt7KGyv62VCp8R2o74wKwI/export/png" width="200px">

### Variant component files

Variant component files should only define styles that are truly unique to that
variant (aka the
"[set difference](https://proofwiki.org/wiki/Definition:Set_Difference)").

<img alt="The set difference of set A and set B" src="https://docs.google.com/drawings/d/1LwrQMRg8EztBRaqyOyH5arAlv6gzlzuaXNhJu0v9pGI/export/png" width="200px">

### Ordering CSS declarations

CSS declarations should be ordered as follows:

1.  Static CSS properties
1.  Themeable CSS properties
1.  Subcomponent theme mixin calls

```scss {.good}
&-foo {
  display: flex;
  color: _token($theme, 'color');
  @include ripple.theme((
    'hover-color': red,
  ));
}
```
