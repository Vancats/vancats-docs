#### 设置主题

```ts
import { defineConfigWithTheme } from 'vitepress'
import { ThemeConfig } from 'your-theme'

export default defineConfigWithTheme<ThemeConfig>({
  themeConfig: {
    // Type is `ThemeConfig`
  }
})
```