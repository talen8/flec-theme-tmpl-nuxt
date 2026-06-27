# Markdown

Markdown 渲染、目录提取相关的函数。

## renderMarkdown

完整渲染 Markdown（支持代码高亮、KaTeX 数学公式）。

```ts
const html = renderMarkdown(content)
```

**参数：**

| 参数 | 类型 | 说明 |
|---|---|---|
| `content` | `string` | Markdown 文本 |

**返回值：** 渲染后的 HTML 字符串

**示例：**

```vue
<script setup>
const { data: article } = useArticle(slug)
</script>

<template>
  <div v-html="renderMarkdown(article?.content || '')" />
</template>
```

## renderSimpleMarkdown

简单渲染 Markdown（不含代码高亮，性能更好）。

```ts
const html = renderSimpleMarkdown(content)
```

**参数：**

| 参数 | 类型 | 说明 |
|---|---|---|
| `content` | `string` | Markdown 文本 |

**返回值：** 渲染后的 HTML 字符串

**使用场景：** 评论内容、简介等不需要完整渲染的场景。

## extractToc

提取文章目录。

```ts
const toc = extractToc(content)
```

**参数：**

| 参数 | 类型 | 说明 |
|---|---|---|
| `content` | `string` | Markdown 文本 |

**返回值：** `TocItem[]` 目录树

**TocItem 结构：**

```ts
interface TocItem {
  id: string        // 锚点 ID
  text: string      // 标题文本
  level: number     // 标题级别 (1-6)
  children?: TocItem[] // 子标题
}
```

**示例：**

```vue
<script setup>
const { data: article } = useArticle(slug)
const toc = computed(() => extractToc(article.value?.content || ''))
</script>

<template>
  <nav class="toc">
    <div v-for="item in toc" :key="item.id">
      <a :href="`#${item.id}`">{{ item.text }}</a>
      <div v-if="item.children?.length">
        <a v-for="child in item.children" :key="child.id" :href="`#${child.id}`">
          {{ child.text }}
        </a>
      </div>
    </div>
  </nav>
</template>
```

## countWords

统计字数。

```ts
const count = countWords(content)
```

**参数：**

| 参数 | 类型 | 说明 |
|---|---|---|
| `content` | `string` | Markdown 文本 |

**返回值：** 字数（数字）

## estimateReadingTime

预估阅读时间（分钟）。

```ts
const minutes = estimateReadingTime(content, wordsPerMinute?)
```

**参数：**

| 参数 | 类型 | 默认值 | 说明 |
|---|---|---|---|
| `content` | `string` | - | Markdown 文本 |
| `wordsPerMinute` | `number` | `300` | 每分钟阅读字数 |

**返回值：** 阅读时间（分钟）

**示例：**

```vue
<script setup>
const { data: article } = useArticle(slug)
const wordCount = computed(() => countWords(article.value?.content || ''))
const readTime = computed(() => estimateReadingTime(article.value?.content || ''))
</script>

<template>
  <span>{{ wordCount }} 字 · 约 {{ readTime }} 分钟</span>
</template>
```

## copyCodeBlock

复制代码块内容（用于代码块的复制按钮）。

```ts
copyCodeBlock(button)
```

**参数：**

| 参数 | 类型 | 说明 |
|---|---|---|
| `button` | `HTMLElement` | 复制按钮元素 |

**示例：**

```vue
<template>
  <button class="copy-btn" @click="copyCodeBlock($event.target)">复制</button>
</template>
```

## switchTab

切换标签页（用于代码块的多标签切换）。

```ts
switchTab(tabsId, tabName)
```

**参数：**

| 参数 | 类型 | 说明 |
|---|---|---|
| `tabsId` | `string` | 标签容器 ID |
| `tabName` | `string` | 标签名称 |

## toggleFold

切换折叠块（用于可折叠的内容块）。

```ts
toggleFold(foldId)
```

**参数：**

| 参数 | 类型 | 说明 |
|---|---|---|
| `foldId` | `string` | 折叠块 ID |

## toggleAudioPlay

切换音频播放状态。

```ts
toggleAudioPlay(audioId)
```

**参数：**

| 参数 | 类型 | 说明 |
|---|---|---|
| `audioId` | `string` | 音频元素 ID |

## seekAudio

跳转音频进度。

```ts
seekAudio(audioId, event)
```

**参数：**

| 参数 | 类型 | 说明 |
|---|---|---|
| `audioId` | `string` | 音频元素 ID |
| `event` | `MouseEvent` | 鼠标事件 |

## toggleMusicPlay

切换音乐播放状态（通过 Meting API）。

```ts
toggleMusicPlay(audioId, server, musicId)
```

**参数：**

| 参数 | 类型 | 说明 |
|---|---|---|
| `audioId` | `string` | 音频元素 ID |
| `server` | `string` | 音乐平台 |
| `musicId` | `string` | 音乐 ID |

## seekMusic

跳转音乐进度。

```ts
seekMusic(audioId, event)
```

**参数：**

| 参数 | 类型 | 说明 |
|---|---|---|
| `audioId` | `string` | 音频元素 ID |
| `event` | `MouseEvent` | 鼠标事件 |
