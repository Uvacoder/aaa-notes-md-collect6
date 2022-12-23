
### Using emotion's css prop

The styled component is only really useful for when you need to reuse a
component. For one-off styles, it's less useful. You inevitably end up creating
components with meaningless names like "Wrapper" or "Container".

Much more often I find it's nice to write one-off styles as props directly on
the element I'm rendering. Emotion does this using a special prop and a custom
JSX function (similar to `React.createElement`). You can learn more about how
this works from emotion's docs, but for this exercise, all you need to know is
to make it work, you simply add this to the top of the file where you want to
use the `css` prop:

```javascript
/** @jsx jsx */
/** @jsxFrag React.Fragment */
import {jsx} from '@emotion/core'
import React from 'react'
```

With that, you're ready to use the CSS prop anywhere in that file:

```jsx
function SomeComponent() {
  return (
    <div
      css={{
        backgroundColor: 'hotpink',
        '&:hover': {
          color: 'lightgreen',
        },
      }}
    >
      This has a hotpink background.
    </div>
  )
}

// or with string syntax:

function SomeOtherComponent() {
  const color = 'darkgreen'

  return (
    <div
      css={css`
        background-color: hotpink;
        &:hover {
          color: ${color};
        }
      `}
    >
      This has a hotpink background.
    </div>
  )
}
```

Ultimately, this is compiled to something that looks a bit like this:

```javascript
function SomeComponent() {
  return <div className="css-bp9m3j">This has a hotpink background.</div>
}
```

📜 https://emotion.sh/docs/css-prop
