# Shadow DOM

<p class="description">The shadow DOM lets you encapsulate parts of an app to keep them separate from global styles that target the regular DOM tree.</p>

## How to use the shadow DOM with Material UI

### 1. Styles

The shadow DOM is an API that provides a way to attach a hidden separated DOM to an element.
This is useful when you need to keep the structure, style, and behavior of different components separate from the rest of the code on the page, to prevent conflicts.
See [the MDN docs on the shadow DOM](https://developer.mozilla.org/en-US/docs/Web/Web_Components/Using_shadow_DOM) for more information.
The following code snippet shows how to apply styles inside of the shadow DOM:

```tsx
const container = document.querySelector('#root');
const shadowContainer = container.attachShadow({ mode: 'open' });
const emotionRoot = document.createElement('style');
const shadowRootElement = document.createElement('div');
shadowContainer.appendChild(emotionRoot);
shadowContainer.appendChild(shadowRootElement);

const cache = createCache({
  key: 'css',
  prepend: true,
  container: emotionRoot,
});

ReactDOM.createRoot(shadowRootElement).render(
  <CacheProvider value={cache}>
    <App />
  </CacheProvider>,
);
```

### 2. Theme

MUI components like `Menu`, `Dialog`, `Popover` and others use [`Portal`](/material-ui/react-portal/) to render a new "subtree" in a container outside of current DOM hierarchy.
By default, this container is `document.body`.
But since the styles are applied only inside of the Shadow DOM, we need to render portals inside the Shadow DOM container as well:

```tsx
const theme = createTheme({
  components: {
    MuiPopover: {
      defaultProps: {
        container: shadowRootElement,
      },
    },
    MuiPopper: {
      defaultProps: {
        container: shadowRootElement,
      },
    },
  },
});

// ...

<ThemeProvider theme={theme}>
  <App />
</ThemeProvider>;
```

## Demo

In the example below you can see that the component outside of the shadow DOM is affected by global styles, while the component inside of the shadow DOM is not:

{{"demo": "ShadowDOMDemo.js", "defaultCodeOpen": false}}
