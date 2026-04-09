# Motion for React — Quick Start

> Motion for React (previously Framer Motion) is a production-grade animation library for React. It provides a declarative `motion` component plus hooks for gestures, layout animations, timelines, and scroll-linked effects.

Source: <https://motion.dev/docs/react-quick-start>

Motion is compatible with React 18.2 and higher.

## Installation

Install with your package manager of choice:

```bash
# npm
npm install motion

# yarn
yarn add motion

# pnpm
pnpm add motion
```

Motion for React is exported from the `motion/react` entry point:

```jsx
import { motion } from "motion/react"
```

You can also load Motion directly from a CDN (jsDelivr) without a build step — see the canonical docs for the `<script>` snippet.

## Your first animation

The `<motion />` component is the foundation of Motion for React. Prefix any HTML or SVG tag (`div`, `span`, `button`, `path`, ...) with `motion.` to unlock animation props like `initial`, `animate`, `exit`, `transition`, `whileHover`, and `whileTap`.

```jsx
import { motion } from "motion/react"

export function FadeIn() {
  return (
    <motion.div
      initial={{ opacity: 0, y: 20 }}
      animate={{ opacity: 1, y: 0 }}
      transition={{ duration: 0.6, ease: "easeOut" }}
    >
      Hello, Motion!
    </motion.div>
  )
}
```

- `initial` defines the starting state when the component mounts.
- `animate` is the target state. When the values change, Motion animates to them.
- `transition` controls duration, easing, delay, and spring physics.

## Transitions

Override the animation type, duration, easing, or delay via the `transition` prop:

```jsx
<motion.div
  animate={{ scale: 2 }}
  transition={{ duration: 2 }}
/>
```

By default, physical properties like `x`, `y`, `scale`, and `rotate` use **spring** physics, while visual properties like `opacity` and `backgroundColor` use **tween** easing. You can opt in explicitly:

```jsx
<motion.div
  animate={{ x: 100 }}
  transition={{ type: "spring", stiffness: 300, damping: 20 }}
/>
```

## Gestures

### Hover

```jsx
<motion.button
  whileHover={{ scale: 1.1 }}
  whileTap={{ scale: 0.95 }}
>
  Click me
</motion.button>
```

`whileHover` and `whileTap` are declarative target states. Motion animates to them when the gesture starts and back to the previous state when it ends.

### Drag

```jsx
<motion.div
  drag
  dragConstraints={{ left: -100, right: 100, top: -50, bottom: 50 }}
/>
```

Use `drag`, `dragConstraints`, `dragElastic`, and `dragMomentum` to build interactive drag behaviour without managing pointer events yourself.

## Keyframes

Pass an array of values to animate through keyframes:

```jsx
<motion.div
  animate={{
    x: [0, 100, 0],
    rotate: [0, 180, 360],
  }}
  transition={{ duration: 2, repeat: Infinity }}
/>
```

## Exit animations

Wrap conditionally rendered components in `AnimatePresence` to animate them out of the tree:

```jsx
import { AnimatePresence, motion } from "motion/react"

function Modal({ isOpen }) {
  return (
    <AnimatePresence>
      {isOpen && (
        <motion.div
          initial={{ opacity: 0 }}
          animate={{ opacity: 1 }}
          exit={{ opacity: 0 }}
        />
      )}
    </AnimatePresence>
  )
}
```

## Variants

Variants let you orchestrate animations across a tree of components by label rather than by value:

```jsx
const container = {
  hidden: { opacity: 0 },
  visible: {
    opacity: 1,
    transition: { staggerChildren: 0.1 },
  },
}

const item = {
  hidden: { opacity: 0, y: 20 },
  visible: { opacity: 1, y: 0 },
}

function List({ items }) {
  return (
    <motion.ul initial="hidden" animate="visible" variants={container}>
      {items.map((text) => (
        <motion.li key={text} variants={item}>
          {text}
        </motion.li>
      ))}
    </motion.ul>
  )
}
```

Parent variants propagate to children automatically, and `staggerChildren` sequences them.

## Imperative control with `useAnimate`

`useAnimate` provides imperative controls for cases that need sequencing, timeline scrubbing, or triggering animations from events outside React's render cycle. It returns a scope ref and an `animate` function:

```jsx
import { useAnimate } from "motion/react"
import { useEffect } from "react"

function Sequence() {
  const [scope, animate] = useAnimate()

  useEffect(() => {
    const run = async () => {
      await animate(scope.current, { opacity: 1 })
      await animate("li", { x: 100 }, { delay: 0.1 })
    }
    run()
  }, [])

  return (
    <ul ref={scope}>
      <li>One</li>
      <li>Two</li>
      <li>Three</li>
    </ul>
  )
}
```

The scope ref scopes CSS-selector-style targets (`"li"`) to descendants of that element.

## Layout animations

Add the `layout` prop and Motion will automatically animate layout changes (position, size, flex reordering) using FLIP:

```jsx
<motion.div layout />
```

Use `layoutId` to animate between different components that share an identity (e.g. a thumbnail expanding into a full view).

## Next steps

- **Animation reference**: `animate`, `initial`, `exit`, `transition` — <https://motion.dev/docs/react-animation>
- **Gestures**: hover, tap, drag, pan — <https://motion.dev/docs/react-gestures>
- **`motion` component API**: <https://motion.dev/docs/react-motion-component>
- **`useAnimate`**: <https://motion.dev/docs/react-use-animate>
- **Installation details**: <https://motion.dev/docs/react-installation>

This file is a condensed quick start. Refer to the canonical docs at <https://motion.dev/docs/react-quick-start> for the latest examples and any new APIs.
