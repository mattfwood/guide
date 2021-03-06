# Styling

Mint has CSS support for styling HTML elements built in.

Inside components you can define styles with a **style block** that is connected to an identifier:

```text
component Test {
  style base {
    font-family: sans;
    font-weight: bold;
    color: red;
  }

  fun render : Html {
    <div::base>
      Hello
    </div>
  }
}
```

You can apply the styles to an HTML element with by specifying it after the tag of the element `<div::style/>` .

You can use CSS syntax inside a `style` block.

## Interpolation

Expressions can be interpolated in the value of a property using brackets `{...}`:

```text
component Test {
  style base {
    color: {color};
  }

  get color : String {
    "red"
  }

  fun render : Html {
    <div::base>
      Hello
    </div>
  }
}
```

Here the color of the text evaluates to `"red"`.

## Sub rules

Additional rules that match the element can be defined in the same block:

```text
component Test {
  style base {
    color: cyan;

    &:focus {
      color: red;
    }
    
    & a {
      color: blue;
    }
  }

  fun render : Html {
    <div::base tabindex="0">
      <{ "Hello " }>
      <a>
        <{ "world" }>
      </a>
    </div>
  }
}
```

It is useful when you want to specify styling for **pseudo elements and states** or want to style child elements.

## Media queries

[Media queries](https://www.w3.org/TR/css3-mediaqueries/) can be defined for a style. If they match their criteria their contents will apply to any element that  has that style.

```text
component Test {
  style base {
    color: red;

    @media (min-width: 600px) {
      color: blue;
    }
  }
  fun render : Html {
    <div::base>
      <{ "Hello " }>  
    </div>
  }
}
```

## Behind the scenes

The compiler separates properties that have interpolations from the ones that don't. The properties with interpolations are converted to use CSS variables \(`--test-base-color` \).

During compiling the dynamic properties are converted to use a `style` object that contains the appropriate variables with their expressions.

The defined styles are static and added to the `head` of the document at runtime.

