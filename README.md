# Wafer Connect

Wafer Connect offers minimalist pre-built components for connecting to wallets with [Svelte](https://svelte.dev) decentralized applications (dApps).

Try it out: [https://wafer.vercel.app](https://wafer.vercel.app)

## Getting Started

Install WaferConnect and its peer dependencies, @wagmi/core & viem

```bash
  npm install wafer-connect @wagmi/core viem
```

## Minimal Example

```svelte
<script>
  import { createConfig, configureChains } from "@wagmi/core";
  import { mainnet } from "@wagmi/core/chains";
  import { publicProvider } from "@wagmi/core/providers";
  import ConnectButton, { getDefaultConnectors } from "wafer-connect";

  const { chains, publicClient, webSocketPublicClient } = configureChains(
    [mainnet],
    [publicProvider()]
  );

  const { connectors } = getDefaultConnectors({
    projectId: "...",
    chains,
  });

  const wagmiConfig = createConfig({
    chains,
    connectors,
    publicClient,
    webSocketPublicClient,
  });
</script>

<header>
  <div />
   <ConnectButton {wagmiConfig} />
</header>

<style>
  header {
    display: flex;
    justify-content: space-between;
    padding: 8px;
  }
</style>
```

## Actions

A basic read contract example.

```svelte
<script lang="ts">
  import {
    configureChains,
    createConfig,
    erc721ABI,
    readContract,
  } from "@wagmi/core";
  import { mainnet } from "@wagmi/core/chains";
  import { alchemyProvider } from "@wagmi/core/providers/alchemy";
  import ConnectButton, { getDefaultConnectors } from "wafer-connect";

  const { chains, publicClient } = configureChains(
    [mainnet],
    [alchemyProvider({ apiKey: "..." })]
  );

  const { connectors } = getDefaultConnectors({
    projectId: "...",
    chains,
  });
  const wagmiConfig = createConfig({
    chains,
    connectors,
    publicClient,
  });

  let value: unknown;

  async function getData() {
    value = await readContract({
      address: "0xFBA3912Ca04dd458c843e2EE08967fC04f3579c2" as `0x${string}`,
      abi: erc721ABI,
      functionName: "name",
    });
  }
</script>

<svelte:head>
  <title>Home</title>
  <meta name="description" content="Svelte demo app" />
</svelte:head>
<header>
  <div />
  <ConnectButton {wagmiConfig} />
</header>
<section>
  <h2>{`Name: ${value}`}</h2>
  <button on:click={() => getData()}>Read Data</button>
</section>

<style>
  header {
    display: flex;
    justify-content: space-between;
    padding: 8px;
  }
  section {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    flex: 0.6;
  }
</style>
```

## Basic Theme

Basic themes can be set by passing the interface below

```ts
interface Theme {
    const borderRadius: string; // Controls the border radius of all elements
    const primaryButtonColor: string;
    const primaryButtonHoverColor: string;
    const primaryButtonTextColor: string;
    const secondaryButtonColor: string;
    const secondaryButtonHoverColor: string;
    const secondaryButtonTextColor: string;
    const errorButtonColor: string;
    const errorButtonHoverColor: string;
    const errorButtonTextColor: string;
    const dialogBackgroundColor: string;
    const dialogTextColor: string;
    const dialogBlur: string;
}
```

```svelte

<script>
   const theme = {...} // Your custom theme
</script>

<ConnectButton {wagmiConfig} {theme}/>
```
