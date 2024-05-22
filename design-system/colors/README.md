# How should colors be organized in a design system?

The way I implement the design system follows a three-tiered color organization: Tokens, Aliases, and Components. This ensures that all colors are reusable.

1. **Token Level:** Represents the raw color values, such as `#000000`, `#ffffff`, `#d32f2f`, etc.
2. **Alias Level:** Represents how colors are used in specific contexts, such as `textDefault`, `backgroundLight`, etc.
3. **Component Level:** Specifies colors as they are applied to specific components, for example, `buttonText`, `buttonBackground`, etc.

**1. Token Level:**

```js
export const systemColor = {
  night: '#0d1f1c',
  onyx: '#3c3c3c',
  ruby: '#d32f2f',
  seaSalt: '#f1f1f1',
  white: '#fff',
  ...
};
```

**2. Alias Level**

```js
const textColors = {
  textDefault: systemColor.night,
  textLight: systemColor.white,
  textDark: systemColor.black,
  ...
};

const backgroundColors = {
  backgroundDefault: brandPrimaryColor,
  backgroundLight: systemColor.white,
  backgroundDark: systemColor.black,
  ...
};

const borderColors = {
  borderDefault: systemColor.onyx,
  borderLight: systemColor.seaSalt,
  borderDark: systemColor.night,
  ...
};
```

**3. Component Level**

_Example for component **Button.tsx**_

```js
const btnPositiveBackground = '$backgroundLight';
const btnNegativeBackground = '$backgroundDark';

// Styled Button with variants
const Button = styled(ButtonBase, {
  name: 'Button',
  backgroundColor: '$backgroundDefault',
  color: '$textDefault',

  variants: {
    positive: {
      true: {
        backgroundColor: btnPositiveBackground,
      },
    },
    negative: {
      true: {
        backgroundColor: btnNegativeBackground,
      },
    },
  },
});
```
