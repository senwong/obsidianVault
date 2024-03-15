阅读：https://sorrycc.com/react-clean-architecture/

## 每个部分遵循单一职责原则。
## 业务逻辑抽取到一个hook
## 减少硬编码
## 传输层校验
使用第三方校验库：[zod](zod.dev)
```ts
// schemas.ts
export const promptSchema = z.object({
  title: z.string(),
  answers: z.array({
    text: z.string(),
    author: z.object({
      name: z.string(),
    }),
    createdAt: z.string().date(),
  }),
})

// prompts-client.ts
export function getActivePrompt() {
  const { data } = api.get(endpoints.activePrompt)
  return promptSchema.parse(data)
}
```
## 嵌套的条件拆分成子组件

```tsx
export default function PromptPage() {
  // ...

  return (
    <main>
      {prompt ? (
        prompt.answered ? (
          <AnswersList answers={prompt.answers} />
        ) : (
          <AnswerForm />
        )
      ) : (
        <RandomInspirationalQuote />
      )}
    </main>
  )
}
```
修改成
```tsx
// PromptPage.tsx
export default function PromptPage() {
  // ...

  return (
    <main>
      {prompt ? (
        <PromptContent prompt={prompt} />
      ) : (
        <RandomInspirationalQuote />
      )}
    </main>
  )
}

// PromptContent.tsx
export default function PromptContent({ prompt }) {
  return prompt.answered ? (
    <AnswersList answers={prompt.answers} />
  ) : (
    <AnswerForm />
  )
}
```
将if-else转换成guard clause
```tsx
// PromptContent.tsx
export default function PromptContent({ prompt }) {
  if (!prompt.answered) {
    return <AnswerForm />
  }

  return <AnswersList answers={prompt.answers} />
}
```