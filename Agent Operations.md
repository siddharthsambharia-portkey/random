# Portkey Integration with Autogen

<img src="https://framerusercontent.com/images/TSKshn2UFdTyvUi85EDMIXrXgs.png?scale-down-to=512" alt="Portkey logo" style="width: 40%;" />

<br>

[Portkey](https://portkey.ai) acts as catalyst to bring your Autogen agents from devlment to prod.  Portky has built in production-ready features like observability, reliability, and gaurdrails. Offering access to 200+ LLMs, fallback strategies, performance insights, and optimization — all with **2 lines** of code changes.
See [Portkey Autogen docs](https://docs.portkey.ai/docs/welcome/agents/autogen)  for details 


## Table of Contents

1. [Key Features](#key-features)
2. [Getting Started](#getting-started)
3. [Integration Guide](#integration-guide)
4. [Advanced Features](#advanced-features)
   - [Interoperability](#interoperability)
   - [Reliability](#reliability)
   - [Metrics and Observability](#metrics)
   - [Comprehensive Logging](#comprehensive-logging)
   - [Continuous Improvement](#continuous-improvement)
   - [Guardrails](#guardrails)
   - [Security and Compliance](#security-and-compliance)
5. [Additional Resources](#additional-resources)

## Key Features

| Feature | Description |
|---------|-------------|
| 🌐 **Multi-LLM Integration** | Access 200+ LLMs with simple configuration changes |
| 🛡️ **Enhanced Reliability** | Implement fallbacks, load balancing, retries  and much more  |
| 📊 **Advanced Metrics** | Track costs, tokens, latency, and 40+ custom metrics effortlessly |
| 🔍 **Detailed Traces and Logs** | Gain insights into every agent action and decision |
| 🚧 **Guardrails** | Enforce agent behavior with real-time checks on inputs and outputs |
| 🔄 **Continuous Optimization** | Capture user feedback for ongoing agent improvements |
| 💾 **Smart Caching** | Reduce costs and latency with built-in caching mechanisms |
| 🔐 **Enterprise-Grade Security** | Set budget limits and implement fine-grained access controls |





## Getting Started

1. **Install Required Packages:**

   ```bash
   pip install -qU pyautogen portkey-ai
   ```

2. **Configure Autogen with Portkey:**

   ```python
   from autogen import AssistantAgent, UserProxyAgent, config_list_from_json
   from portkey_ai import PORTKEY_GATEWAY_URL, createHeaders

   config = [
       {
           "api_key": "OPENAI_API_KEY",
           "model": "gpt-3.5-turbo",
           "base_url": PORTKEY_GATEWAY_URL,
           "api_type": "openai",
           "default_headers": createHeaders(
               api_key="YOUR_PORTKEY_API_KEY",
               provider="openai",
           )
       }
   ]
   ```

   Generate your API key in the [Portkey Dashboard](https://app.portkey.ai/).


## Integration Guide

For a hands-on example of integrating Portkey with Autogen, check out our notebook<br> [![Google Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://git.new/Portkey-Autogen) .

## Advanced Features

### Interoperability

Easily switch between 200+ LLMs by changing the `provider` and API key in your configuration.

#### Example: Switching from OpenAI to Azure OpenAI

```python
config = [
    {
        "api_key": "api-key",
        "model": "gpt-3.5-turbo",
        "base_url": PORTKEY_GATEWAY_URL,
        "api_type": "openai",
        "default_headers": createHeaders(
            api_key="YOUR_PORTKEY_API_KEY",
            provider="azure-openai",
            virtual_key="AZURE_VIRTUAL_KEY"
        )
    }
]
```

### Reliability

Implement fallbacks, load balancing, and automatic retries to make your agents more resilient.

```python
portkey_config = {
  "retry": {
    "attempts": 5
  },
  "strategy": {
    "mode": "loadbalance"  # Options: "loadbalance" or "fallback"
  },
  "targets": [
    {
      "provider": "openai",
      "api_key": "OpenAI_API_Key"
    },
    {
      "provider": "anthropic",
      "api_key": "Anthropic_API_Key"
    }
  ]
}
```

### Metrics

Track comprehensive metrics for your AI agents, including cost, tokens used, and latency.

<details>
  <summary><b>Observability</b></summary>
<img src=https://github.com/siddharthsambharia-portkey/Portkey-Product-Images/blob/main/Portkey-Dashboard.png?raw=true width=70%" alt="Portkey Dashboard" />
</details>

### Comprehensive Logging

Access detailed logs of agent activities, function calls, and errors. Filter logs based on multiple parameters for in-depth analysis.

<details>
  <summary><b>Traces</b></summary>
  <img src="https://raw.githubusercontent.com/siddharthsambharia-portkey/Portkey-Product-Images/main/Portkey-Traces.png" alt="Portkey Logging Interface" width=70% />
</details>

<details>
  <summary><b>Logs</b></summary>


  <img src="https://raw.githubusercontent.com/siddharthsambharia-portkey/Portkey-Product-Images/main/Portkey-Logs.png" alt="Portkey Metrics Visualization" width=70% />
</details>

### Guardrails
Autogen agents, while powerful, can sometimes produce unexpected or undesired outputs. Portkey's Guardrails feature helps enforce agent behavior in real-time, ensuring your Autogen agents operate within specified parameters. Verify both the **inputs** to and *outputs* from your agents to ensure they adhere to specified formats and content guidelines. Learn more about Portkey's Guardrails [here](https://docs.portkey.ai/product/guardrails)


### Continuous Improvement

Capture qualitative and quantitative user feedback on your requests to continuously enhance your agent performance.

### Caching

Reduce costs and latency with Portkey's built-in caching system.

```python
portkey_config = {
 "cache": {
    "mode": "semantic"  # Options: "simple" or "semantic"
 }
}
```

### Security and Compliance

Set budget limits on provider API keys and implement fine-grained user roles and permissions for both your application and the Portkey APIs.


## Additional Resources

- [📘 Portkey Documentation](https://docs.portkey.ai)
- [🐦 Twitter](https://twitter.com/portkeyai)
- [💬 Discord Community](https://discord.gg/JHPt4C7r)
- [📊 Portkey App](https://app.portkey.ai)

For more information on using these features and setting up your Config, please refer to the [Portkey documentation](https://docs.portkey.ai).
