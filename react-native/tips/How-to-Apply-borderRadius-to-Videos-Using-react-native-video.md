# How to Apply borderRadius to Videos Using react-native-video

When working with videos in React Native, a common design requirement is to give the video player rounded corners. However, the `react-native-video` component doesn't directly support the `borderRadius` style. To achieve this effect, you can wrap the `<Video />` component in a `View` and apply the `borderRadius` and `overflow` styles to that wrapper.

## Step-by-Step Implementation

Here’s how you can do it:

```tsx
import React from 'react';
import { View, StyleSheet } from 'react-native';
import Video from 'react-native-video';

export default function MyVideoComponent() {
  return (
    <View style={styles.videoContainer}>
      <Video
        source={{ uri: 'https://your-video-url.mp4' }}
        style={styles.video}
        resizeMode="cover"
        muted
        repeat
      />
    </View>
  );
}

const styles = StyleSheet.create({
  videoContainer: {
    width: 300,
    height: 200,
    borderRadius: 16,
    overflow: 'hidden', // Clips the video corners
  },
  video: {
    width: '100%',
    height: '100%',
  },
});
```

## Key Points

- Wrap the Video: Always place the <Video /> inside a container View when styling for rounded corners.
- Use borderRadius on the Container: Apply the borderRadius to the outer container.
- Don’t Forget overflow: 'hidden': This is essential. Without it, the video will not be clipped to fit the rounded corners.

## Why This Works

The `react-native-video` component renders content using a native view that doesn’t automatically respect styles like `borderRadius`. By wrapping it in a View and applying clipping via `overflow: 'hidden'`, you ensure that any content inside stays within the rounded bounds.
