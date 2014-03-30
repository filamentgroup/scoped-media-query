Scoped Media Query Sass Mixins
==================

An element query workaround. Sass mixins for scoping CSS styles to apply only within given selector/media query pairs.

Addresses the problem explained in this article: http://filamentgroup.com/lab/element_query_workarounds/


``` scss
$screensizes : (
  'default' : 0 infinity,
  'mobile' : 0 767px,
  'phone' : 0 480px,
  'tablet' : 481px 767px,
  'desktop' : 768px 979px,
  'widescreen' : 1200px infinity
);

@mixin respond-to($media) {
    $range : map-get($screensizes, $media);
    @if $range == null {
        @warn 'Unknown screensize "#{$media}" for @mixin respond-to';
    } @else {
        $min-width : nth($range, 1);
        $max-width : nth($range, 2);
        @if $min-width > 0 {
            @if $max-width != infinity {
                @media only screen and (min-width: $min-width) and (max-width: $max-width) { @content; }
            } @else {
                @media only screen and (min-width: $min-width) { @content; }
            }
        } @else {
            @if $max-width != infinity {
                @media only screen and (max-width: $max-width) { @content; }
            } @else {
                @content;
            }
        }
    }
}
```

## Sample Usage:

``` scss
.s1 {
  float : right;
  width: 100%;
  background : 'url(image/cool-background.jpg)';
  @include respond-to(mobile) {
    width : 40%
  }
}
 
.s2 {
    @include respond-to(default) {
      width: 100%;
    }
    @include respond-to(mobile) {
      float: right;
      width: 300px;
    }
    @include respond-to(desktop) {
      width: 125px;
    }
    @include respond-to(widescreen) {
      float: none;
    }
}
 
.s3 {
    @include respond-to(default) {
      float: left;
      width: 100%;
    }
    @include respond-to(desktop) {
      width: 125px;
    }
    @include respond-to(phone) {
        width: 400px;
    }
    @include respond-to(widescreen) {
      float: none;
    }
}
```

### Sample Output:

``` css
.s1 {
  float: right;
  width: 100%;
  background: 'url(image/cool-background.jpg)';
}
@media only screen and (max-width: 767px) {
  .s1 {
    width: 40%;
  }
}

.s2 {
  width: 100%;
}
@media only screen and (max-width: 767px) {
  .s2 {
    float: right;
    width: 300px;
  }
}
@media only screen and (min-width: 768px) and (max-width: 979px) {
  .s2 {
    width: 125px;
  }
}
@media only screen and (min-width: 1200px) {
  .s2 {
    float: none;
  }
}

.s3 {
  float: left;
  width: 100%;
}
@media only screen and (min-width: 768px) and (max-width: 979px) {
  .s3 {
    width: 125px;
  }
}
@media only screen and (max-width: 480px) {
  .s3 {
    width: 400px;
  }
}
@media only screen and (min-width: 1200px) {
  .s3 {
    float: none;
  }
}
```

