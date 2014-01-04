Scoped Media Query Sass Mixins
==================

An element query workaround. Sass mixins for scoping CSS styles to apply only within given selector/media query pairs.

Addresses the problem explained in this article: http://filamentgroup.com/lab/element_query_workarounds/

  * $width
  * $width, $width, $width, ...
  * ($width, $class)
  * ($width, $class), ($width, $class), ...
  * (($width, $widthType), ($width, $widthType), ...)
  * (($width, $widthType), $class)
  * (($width, $widthType), $class)), (($width, $widthType), $class)), ...
  * any combination of the above

[c]2013 @micahgodbolt, @jpavon, @filamentgroup abd @johnslegers. MIT License.


## The mixins:

``` scss
// Dynamically generate a media query of type $media
@mixin media($parameter, $value, $media : null) {
  @if $media == null {
    @media (#{$parameter}: #{$value}) {
        @content;
    }
  } @else if $media == 'screen' {
    @media screen and (#{$parameter}: #{$value}) {
        @content;
    }
  } @else if $media == 'screen only' {
    @media screen only and (#{$parameter}: #{$value}) {
        @content;
    }
  }
}
 
// Prefix media query with a class of your choice
@mixin mediaQuery($type, $parameter, $value, $query) {
    @include media(#{$parameter}, #{$value}, $type) {
        $length: length($query);
        @if ( $length > 1 ) {
            @for $i from 2 through $length {
                $class : nth($query, $i);
                #{$class} {
                    @content;
                }
            }
        } @else {
            @content;
        }
    }
}
 
 
// Generate media query of type $media
@mixin respond-to($media, $queries...) {
    @each $query in $queries {
        $width : nth($query, 1);
        $parameter : 'min-width';
        $value : $width;
        $lengthwidth : length($width);
        @if($lengthwidth > 1){
            $parameter : nth($width, 2);
            $value : nth($width, 1);
        } @else {
            $parameter : 'min-width';
            $value : $width;
        }
        @include mediaQuery($media, $parameter, $value, $query) {
            @content;
        }
    }
}
 
@mixin respond-to-all($queries...) {@include respond-to(null, $queries...){@content;}}
@mixin respond-to-screen($queries...) {@include respond-to('screen', $queries...){@content;}}
@mixin respond-to-screen-only($queries...) {@include respond-to('screen-only', $queries...){@content;}}
```

## Sample Usage:

``` scss
@include respond-to-screen((32em, '.content'), (60em), (90em, aside, main)) {
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

@include respond-to-all(((32em, 'max-width'), '.tab'), ((64em, 'max-width'), '.tab', '.menu')) {
    li {
        width:100%; 
    }
}
```

### Sample Output:

``` css
@media screen and (min-width: 32em) {
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
@media screen and (min-width: 60em) {
  .schedule-component {
    float: left;
    width: 100%;
    position: relative;
  }

  .schedule-component ul,
  .schedule-component li {
    list-style: none;
    position: absolute;
    margin: 0;
    padding: 0;
  }
}
@media screen and (min-width: 90em) {
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

  main .schedule-component {
    float: left;
    width: 100%;
    position: relative;
  }
  main .schedule-component ul,
  main .schedule-component li {
    list-style: none;
    position: absolute;
    margin: 0;
    padding: 0;
  }
}
@media (max-width: 32em) {
  .tab li {
    width: 100%;
  }
}
@media (max-width: 64em) {
  .tab li {
    width: 100%;
  }

  .menu li {
    width: 100%;
  }
}
```




[![Bitdeli Badge](https://d2weczhvl823v0.cloudfront.net/jslegers/scoped-media-query/trend.png)](https://bitdeli.com/free "Bitdeli Badge")

