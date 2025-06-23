# Using react-native-linear-gradient with [Reanimated](https://docs.swmansion.com/react-native-reanimated/)

If you're building dynamic UIs in React Native with smooth animations, chances are you're already using Reanimated (v2 or v3). But what if you want to animate a gradient background using the popular `react-native-linear-gradient` library?

Good news — it’s totally possible! But there’s one important step you need to know.

## Why It Doesn’t Work Out of the Box

By default, `react-native-linear-gradient` is not an animated component. This means you can't directly apply animated styles like `opacity`, `transform`, or `borderRadius` to it using Reanimated.

## The Fix: Wrap It as an Animated Component

To make it work with Reanimated, you need to wrap the gradient with `Animated.createAnimatedComponent`. Here's how:

```tsx
import LinearGradient from 'react-native-linear-gradient';
import Animated from 'react-native-reanimated';

const AnimatedLinearGradient = Animated.createAnimatedComponent(LinearGradient);
```

Once wrapped, you can now apply Reanimated animations to it just like any other animated view:

```tsx
<AnimatedLinearGradient
  colors={['#ff8a00', '#e52e71']}
  style={animatedStyle} // contains animated styles from useAnimatedStyle
/>
```

## What You Can Animate

With this approach, you can animate:

- `opacity` — for fade effects
- `borderRadius` — for smooth corner transitions
- `transform` — for scaling, rotating, translating the gradient
- And any other style property supported by the gradient’s underlying view

## Create Smooth, Dynamic Gradient UIs

This simple trick unlocks a lot of creative possibilities for your app — from animated buttons to full-screen transitions with dynamic color gradients.

So next time you're working with gradients and animations in React Native, remember: just wrap it, and animate away!
