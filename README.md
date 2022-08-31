:warning: This project is archived and the repository is no longer maintained.

# Scoped Media Query Sass Mixin

[![Filament Group](http://filamentgroup.com/images/fg-logo-positive-sm-crop.png) ](http://www.filamentgroup.com/)

An element query workaround. A Sass mixin for scoping CSS styles to apply only within given selector/media query pairs.

Addresses the problem explained in this article: http://filamentgroup.com/lab/element_query_workarounds/

Accepts sets of selector/media query pairs as arguments. Enclosed styles' selectors are prefixed by each passed selector within an outputted media query block.

©2013 @micahgodbolt, @jpavon, and @filamentgroup. MIT License.


## The mixin:

``` scss
@mixin scopedmediaquery($queries...) {
    $length: length($queries);
    @for $i from 1 through $length{
        @if $i % 2 == 1 {
            @media #{nth($queries, $i)} {
                #{nth($queries, $i+1)} {
                  @content;
                }
            }
        }
    }
}
```

## Sample Usage:

``` scss
  @include scopedmediaquery(
    '(min-width : 30em)', '.content',
    '(min-width : 90em)', 'aside'
  ) {
  /* breakpoint styles here */
  .schedule-component {
      float: left; 
      width: 100%;
      position:relative; 
  }
  .schedule-component ul,
  .schedule-component li {
    list-style: none;
    position: absolute;
    margin: 0;
    padding: 0;
  }
}
```

### Sample Output:

``` css
@media (min-width: 30em) {
  .content .schedule-component {
    float: left;
    width: 100%;
    position: relative;
  }
  .content .schedule-component ul,
  .content .schedule-component li {
    list-style: none;
    position: absolute;
    margin: 0;
    padding: 0;
  }
}
@media (min-width: 90em) {
  aside .schedule-component {
    float: left;
    width: 100%;
    position: relative;
  }
  aside .schedule-component ul,
  aside .schedule-component li {
    list-style: none;
    position: absolute;
    margin: 0;
    padding: 0;
  }
}
```


