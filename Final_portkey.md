
# Vercel AI SDK - Portkey Integration

Portkey natively integrates with the Vercel AI SDK to make your apps production-ready and reliable. Import Portkey's Vercel package and use it as a provider in your Vercel AI app to enable all of Portkey's features:

- Full-stack **observability** and **tracing** for all requests
- Interoperability across **250+ LLMs**
- Built-in **50+** state-of-the-art guardrails
- Simple & semantic **caching** to save costs & time
- Conditional request routing with fallbacks, load-balancing, automatic retries, and more
- Continuous improvement based on user feedback

## Getting Started

### 1. Installation

```bash
npm install @portkey-ai/vercel-provider
```

### 2. Import & Configure Portkey Object

Sign up for Portkey, get your API key, and configure the Portkey provider in your Vercel app:

```typescript
import { createPortkey } from '@portkey-ai/vercel-provider';

const portkeyConfig = {
  provider: "openai", // Choose your provider (e.g., 'anthropic', 'aws-bedrock')
  api_key: "OPENAI_API_KEY",
  model: "gpt-4" // Select from 250+ models
};

const portkey = createPortkey({
  apiKey: 'YOUR_PORTKEY_API_KEY',
  config: portkeyConfig,
});
```

Generate your API key in the [Portkey Dashboard](https://app.portkey.ai).

### 3. Running Your First Request

Portkey provider works with the `generateText` and `streamText` functions of the Vercel AI SDK.

#### Using `generateText`:

```typescript
import { createPortkey } from '@portkey-ai/vercel-provider';
import { generateText } from 'ai';

const portkeyConfig = {
  provider: "openai",
  api_key: "OPENAI_API_KEY",
  override_params: {
    model: "gpt-4"
  }
};

const portkey = createPortkey({
  apiKey: 'YOUR_PORTKEY_API_KEY',
  config: portkeyConfig,
});

const { text } = await generateText({
  model: portkey.chatModel(''), // Provide an empty string, we defined the model in the config
  prompt: 'What is Portkey?', 
});

console.log(text);
```

#### Using `streamText`:

```typescript
import { createPortkey } from '@portkey-ai/vercel-provider';
import { streamText } from 'ai';

const portkeyConfig = {
  provider: "openai",
  api_key: "OPENAI_API_KEY",
  override_params: {
    model: "gpt-4"
  }
};

const portkey = createPortkey({
  apiKey: 'YOUR_PORTKEY_API_KEY',
  config: portkeyConfig,
});

const result = await streamText({
  model: portkey.completionModel(''), // Provide an empty string, we defined the model in the config
  prompt: 'Invent a new holiday and describe its traditions.',
});

for await (const chunk of result) {
  console.log(chunk);
}
```

## Portkey's `chatModel` & `completionModel`

Portkey supports `chatModel` and `completionModel` to easily handle chatbots or text completions. In the above examples, we used `portkey.chatModel` for the `useChat` hook and `portkey.completionModel` for `useCompletion`.

## Tool Calling with Portkey

```typescript
import { z } from 'zod';
import { generateText, tool } from 'ai';

const result = await generateText({
  model: portkey.chatModel('gpt-4-turbo'),
  tools: {
    weather: tool({
      description: 'Get the weather in a location',
      parameters: z.object({
        location: z.string().describe('The location to get the weather for'),
      }),
      execute: async ({ location }) => ({
        location,
        temperature: 72 + Math.floor(Math.random() * 21) - 10,
      }),
    }),
  },
  prompt: 'What is the weather in San Francisco?',
});
```

# Portkey Features

Portkey helps make your Vercel app more robust and reliable. The Portkey config is a modular way to customize its functionality for your specific needs.

## Interoperability

Portkey allows you to easily switch between 250+ AI models by simply changing the model name in your configuration. This flexibility enables you to adapt to the evolving AI landscape without significant code changes.

### Switching from OpenAI to Anthropic

Here's how you'd use OpenAI with Portkey's Vercel integration:

```javascript
const portkeyConfig = {
  provider: "openai",
  api_key: "OPENAI_API_KEY",
  model: "gpt-4-turbo"
};
```

To switch to Anthropic, change your provider slug to `anthropic` and enter your Anthropic API key along with the model of choice:

```javascript
const portkeyConfig = {
  provider: "anthropic",
  api_key: "Anthropic_API_KEY",
  model: "claude-3-sonnet-20240229"
};
```

## Observability

Portkey's OpenTelemetry-compliant observability suite gives you complete control over all your requests. Portkey's analytics dashboards provide 40+ key insights you're looking for, including cost, tokens, latency, and more.

![Portkey's Observability Dashboard](https://raw.githubusercontent.com/siddharthsambharia-portkey/Portkey-Product-Images/main/Portkey-Dashboard.png)

![Portkey's Logs](https://raw.githubusercontent.com/siddharthsambharia-portkey/Portkey-Product-Images/main/Portkey-Logs.png)

## Reliability

Portkey enhances the robustness of your AI applications with built-in features such as caching, fallback mechanisms, load balancing, conditional routing, and request timeouts.

Here's how you can modify your config to include the following Portkey features:
- Fallback
- Caching
- Conditional routing

```typescript
import { createPortkey } from '@portkey-ai/vercel-provider';
import { generateText } from 'ai';

const portkeyConfig = {
  strategy: {
    mode: "fallback"
  },
  targets: [
    { virtual_key: "anthropic-key" },
    { virtual_key: "aws-bedrock-key" }
  ]
};

const portkey = createPortkey({
  apiKey: 'YOUR_PORTKEY_API_KEY',
  config: portkeyConfig,
});

const { text } = await generateText({
  model: portkey.chatModel(''),
  prompt: 'What is Portkey?',
});

console.log(text);
```

Learn more about Portkey's AI gateway features in detail [here](https://docs.portkey.ai/docs/product/ai-gateway/configs).

## Guardrails

Portkey Guardrails allow you to enforce LLM behavior in real-time, verifying both inputs and outputs against specified checks. 

You can create Guardrail checks in the UI and then pass them in your Portkey Configs with before-request or after-request hooks.

Read more about Guardrails [here](https://docs.portkey.ai/docs/product/guardrails).

## Security and Compliance

Set budget limits on provider API keys and implement fine-grained user roles and permissions for both your application and the Portkey APIs.


## Advanced Configuration

The `portkeyConfig` object supports various options:

- `provider`: The AI provider to use (e.g., "openai", "anthropic")
- `api_key`: The API key for the chosen provider

- `strategy`: Defines routing and fallback strategies
- `targets`: Specifies multiple providers for fallback or load balancing
- `override_params`: Allows overriding default parameters- model, top_p, top_k, etc.


For a complete list of options, refer to the [Portkey documentation](https://docs.portkey.ai/docs/product/ai-gateway/configs).

## Additional Resources

- [📘 Portkey Documentation](https://docs.portkey.ai/docs/integrations/libraries/vercel)
- [🐦 Twitter](https://twitter.com/portkeyai)
- [💬 Discord Community](https://discord.gg/JHPt4C7r)
- [📊 Portkey App](https://app.portkey.ai)

