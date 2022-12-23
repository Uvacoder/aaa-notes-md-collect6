### Making a styled component with emotion

Here's how you make a "styled component":

```javascript
import styled from '@emotion/styled'

const Button = styled.button`
  color: turquoise;
`

// <Button>Hello</Button>
//
//     👇
//
// <button className="css-1ueegjh">Hello</button>
```

You can also use object syntax (this is my personal preference):

```javascript
const Button = styled.button({
  color: 'turquoise',
})
```

You can even accept props by passing a function and returning the styles! 📜
https://emotion.sh/docs/styled

```javascript
const Button = styled.button(props => {
  return {
    color: props.primary ? 'hotpink' : 'turquoise',
  }
})

// or with the string form:

const Button = styled.button`
  color: ${props => (props.primary ? 'hotpink' : 'turquoise')};
`
```
