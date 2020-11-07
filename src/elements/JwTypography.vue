<template>
  <component :is="componentType" :class="[baseClass, componentTypeClass, sizeClass, boldClass]">
    <slot />
  </component>
</template>

<script>
const TYPOGRAPHY_MAP = {
  body: "p",
  overline: "span",
  label: "span",
  caption: "span",
  h1: "h1",
  h2: "h2",
  h3: "h3",
  h4: "h4",
  h5: "h5",
}

const BASE_CLASS = "jw-typography"

export default {
  name: "JwTypography",
  status: "prototype",
  release: "1.0.0",
  props: {
    /**
     * The html element used for the text.
     *
     * `h1, h2, h3, h4, h5, body, overline, caption, label`
     */
    type: {
      type: String,
      default: "body",
      validator: value => {
        return value.match(/(h1|h2|h3|h4|h5|body|overline|caption|label)/)
      },
    },
    /**
     * Text content size. Use with `type='body'`
     *
     * `normal, big, small`
     */
    size: {
      type: String,
      default: "normal",
      validator: value => {
        return value.match(/(normal|big|small)/)
      },
    },
    /**
     * Make text content bold
     */
    bold: {
      type: Boolean,
      default: false,
    },
  },
  computed: {
    baseClass() {
      return BASE_CLASS
    },
    componentType() {
      return TYPOGRAPHY_MAP[this.type]
    },
    componentTypeClass() {
      return `${BASE_CLASS}-${this.type}`
    },
    sizeClass() {
      if (this.type === "body") {
        return `${this.componentTypeClass}--${this.size}`
      }

      return ""
    },
    boldClass() {
      return this.bold ? `${BASE_CLASS}--bold` : ""
    },
  },
}
</script>

<style lang="scss" scoped>
$heading-font-size: (
  h1: $jw-font-size-h1,
  h2: $jw-font-size-h2,
  h3: $jw-font-size-h3,
  h4: $jw-font-size-h4,
  h5: $jw-font-size-h5,
);

$heading-line-height: (
  h1: $jw-line-height-h1,
  h2: $jw-line-height-h2,
  h3: $jw-line-height-h3,
  h4: $jw-line-height-h4,
  h5: $jw-line-height-h5,
);

$heading-letter-spacing: (
  h1: $jw-letter-spacing-h1,
  h2: $jw-letter-spacing-h2,
  h3: $jw-letter-spacing-h3,
  h4: $jw-letter-spacing-h4,
  h5: $jw-letter-spacing-h5,
);

@mixin font-properties($font-size, $line-height, $letter-spacing) {
  font-size: $font-size;
  line-height: $line-height;
  letter-spacing: $letter-spacing;
}

.jw-typography {
  @include reset;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;

  @each $heading, $font-size in $heading-font-size {
    &-#{$heading} {
      @include font-properties(
        $font-size,
        map-get($map: $heading-line-height, $key: $heading),
        map-get($map: $heading-letter-spacing, $key: $heading)
      );

      font-weight: 900;
    }
  }

  &-body {
    @include font-properties($jw-font-size-body, $jw-line-height-body, $jw-letter-spacing-body);

    &--small {
      @include font-properties(
        $jw-font-size-body-small,
        $jw-line-height-body-small,
        $jw-letter-spacing-body-small
      );
    }

    &--big {
      @include font-properties(
        $jw-font-size-body-big,
        $jw-line-height-body-big,
        $jw-letter-spacing-body-big
      );
    }
  }

  &-caption {
    @extend .jw-typography--bold;
    @include font-properties(
      $jw-font-size-caption,
      $jw-line-height-caption,
      $jw-letter-spacing-caption
    );
    text-transform: uppercase;
  }

  &-overline {
    @extend .jw-typography--bold;
    @include font-properties(
      $jw-font-size-overline,
      $jw-line-height-overline,
      $jw-letter-spacing-overline
    );
    text-transform: uppercase;
  }

  &-label {
    @include font-properties($jw-font-size-label, $jw-line-height-label, $jw-letter-spacing-label);
  }

  &--bold {
    font-weight: bold;
  }
}
</style>

<docs>
  ```jsx
  <div>
    <JwTypography type="h1">
      H1: JustWatch is cool
    </JwTypography>
    <JwTypography type="h2">
      H2: JustWatch is cool
    </JwTypography>
    <JwTypography type="h3">
      H3: JustWatch is cool
    </JwTypography>
    <JwTypography type="h4">
      H4: JustWatch is cool
    </JwTypography>
    <JwTypography type="h5">
      H5: JustWatch is cool
    </JwTypography>
  </div>
  ```
  ```jsx
  <div>
    <JwTypography type="body" size="big">
      Body Big: JustWatch is cool
    </JwTypography>
    <JwTypography type="body">
      Body Normal: JustWatch is cool
    </JwTypography>
    <JwTypography type="body" bold>
      Body Normal: JustWatch is cool
    </JwTypography>
    <JwTypography type="body" size="small">
      Body Small: JustWatch is cool
    </JwTypography>
  </div>
  ```
  ```jsx
  <div>
    <JwTypography type="overline">
      Overline: JustWatch is cool
    </JwTypography>
    <br/>
    <JwTypography type="caption">
      Caption: JustWatch is cool
    </JwTypography>
    <br/>
    <JwTypography type="label">
      Label: JustWatch is cool
    </JwTypography>
    <br/>
    <JwTypography type="label" bold>
      Label bold: JustWatch is cool
    </JwTypography>
  </div>
  ```
</docs>
