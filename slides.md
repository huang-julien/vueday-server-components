---
theme: default
class: text-center
highlighter: shiki
lineNumbers: false
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
drawings:
  persist: false
transition: slide-left
title: Welcome to Slidev
mdc: true
---

#  Nuxt server components and Nuxt islands

## Current state and the future

Julien Huang

<!--

-->

---
transition: fade-out
layout: intro
---

<div  class="grid grid-cols-2 gap-5">
  <div>
  <h1> Julien Huang</h1>


<ul>

  <li>
  Frontend developer @ Leetchi in <twemoji-flag-france />
  </li>
  <li>
Nuxt Contributor and Nuxt Team <logos-nuxt-icon  />
  </li>
  <li>
Developer since 3 years  
</li>
  <li>
Doing open-source since a year and a half  
</li>
</ul>
  
  </div>

  <div class="flex flex-cols justify-center">
    <img class="w-1/4 mx-auto rounded-full h-fit my-auto" src="https://media.licdn.com/dms/image/D4D03AQFj3ih9Vrs0XQ/profile-displayphoto-shrink_800_800/0/1676657110062?e=1701907200&v=beta&t=G_9NEMNXfYLA5d4Om64Y6ZQDWCTJVCQNE4nxMxS8G3s" />
  </div>
</div>

<!--
You can have `style` tag in markdown to override the style for the current page.
Learn more: https://sli.dev/guide/syntax#embedded-styles
-->

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

---
layout: intro
---

# Nuxt <logos-nuxt-icon class="text-5xl" />

- auto-import
- eco-system of modules
- one project for multiple providers
- SSR - SSG - CSR
- nitro server
- file based routing

<!--
I think everyone knows what Nuxt is here since Daniel gave you an amazing talk this morning. 
-->

---
layout: center
---

# Server components ? <bxs-component text-green-400 />

<!--
What are server components ?
Server componentsare somethingthat is becoming morend more common in the web developement. It is mainly pushed by react and vercel through next JS
-->

---
layout: center
---

# How does it work ?

<v-click>

## Server-side <uil-server />

- The component is "normally" rendered

</v-click>


<v-click>

## Client-side <icon-park-outline-browser />

- The component is rendered but only with render information

</v-click>

<!--
A server component is simply a component that will be rendered server side but will remain static client side.
This means that client-side, it won't be instantiated normally but it will only get information about how it should be rendered. 
It also implies that no JS chunk with be imported client-side since we don't need it
-->

---

# Server component from different meta-frameworks


<div flex justify-around text-5xl mt-50>
  <div flex flex-col>
  <skill-icons-react-dark mx-auto  />
  <p>RSC</p>
  </div>
  <div flex flex-col>
  <skill-icons-astro  mx-auto />
  <p>Astro</p>
  </div>
  <div flex flex-col >
  <skill-icons-nuxtjs-dark  mx-auto />
  <p>NuxtIslands</p>
</div>

</div>

<!--

React introduced two years ago with react ... 18 and next JS the react server components? On their side, they are using an ast to describe what has to be rendered
On astro side, it's a bit particular, everything is static by default so everything is server component and then you declare which component is loaded client side.
And on our side, we've got the nuxt island.

-->

---

# How does island components works on Nuxt ? 🏝️

<!--

So... how does Nuxtislands works

-->

---

<v-click>

- an endpoint exposed at `/__nuxt_island/**`


````ts
interface NuxtIslandResponse {
  id?: string
  html: string
  state: Record<string, any>
  head: {
    link: (Record<string, string>)[]
    style: ({ innerHTML: string, key: string })[]
  }
}
````

</v-click>
<v-click>

- `NuxtIsland`
  - call the endpoint and render the response as static HTML


</v-click>

<!--

Nuxt provide an endpoint which starts by  `/__nuxt_island/`. This endpoint is responsible for returning an `NuxtIslandResponse`
Then you have the `NuxtIsland` component which will make calls to this endpoint and render the response into static HTML

Since we retrieve an island component thorugh an endpoint, your component will be isolated from your application even in SSR. 
This means that your application and your component does not share the same Vue and nuxt context

-->


---
layout: center
---

# Advantages

<v-clicks>

- no javascript imported client side
- Island responses are cached
- safely write server code within your component
- access server environement

</v-clicks>

<!--

- no client bundle
- safe server code within the component

-->

---
layout: center
---

# Drawbacks

<v-clicks>

- not interactive
- takes time to update

</v-clicks>

<!--

- well... it's server only
- delayed updated
- complexity

-->



---

# How to write server components and island components with Nuxt:

---
layout: center
---

# Enable it within the configuration

```ts
export default defineNuxtConfig({
  experimental: {
    componentIslands: true,
  }
})
```

<!--
So first to write a server component or an island component you need to enable the feature flag within the configuration

Small note about experimental features:
This does not mean that the feature is unstable but rather that the API of a feature may change in the future. BTW there is a lot of experimental features enabled by default.


So once this feature is enabled you can start to write your island and server components.
-->

---

# To create an island component

<LeftRight mt-10>
<template #left>
<div>

## Auto-scan
<v-clicks>

- create a Vue component in the `components/island` directory
- ex: `components/island/MyIsland.vue`

</v-clicks>
</div>
</template>
<template #right>

## Nuxt kit


<v-clicks>

```ts
import { addComponent } from "@nuxt/kit"
export default defineNuxtModule({
  setup(){
    addComponent({
      name: "MyIsland",
      filePath: resolver.resolve('./runtime/MyIsland'),
      island: true
    })
  }
})
```
</v-clicks>

</template>
</LeftRight>

<!--

So there's 2 ways to create an island component.

If you're using the auto-scan/auto-import feature from nuxt, you can create a file within the island directory within the components directory
ex..
If you wish to use the nuxt kit helper, you just need to set island to true

-->

---

# To create a server component

<LeftRight  mt-10>
<template #left>
<div>

## Auto-scan
<v-clicks>

- Use `.server.vue` suffix when you create a Vue component
- ex: `components/MyServerComponent.server.vue`

</v-clicks>
</div>
</template>
<template #right>

## Nuxt kit


<v-clicks>


```ts
import { addComponent } from "@nuxt/kit"
export default defineNuxtModule({
  setup(){
    addComponent({
      name: "MyServerComponent",
      filePath: resolver.resolve('./runtime/MyServerComponent'),
      mode: 'server'
    })
  }
})
```
</v-clicks>

</template>
</LeftRight>

<!--
To create a server component

If you're using the auto-scan/auto-import feature from nuxt, you just need to set the `.server.vue` suffix on you vue file
ex..
If you wish to use the nuxt kit helper, you just need to set the mode of the component to 'server'

-->

---
layout: default
---

```vue
<script setup lang="ts">
const props = defineProps({
  id: string
})

const { privateKey } = useRuntimeConfig()

const article = $fetch('some.api.com', { key: privateKey, id: props.id })
</script>

<template>
  <div>
    <div>
      title : {{ article.name }}
    </div>
  </div>
</template>

<style scoped>
pre {
  color: blue
}
</style>
```

<!--

So this is an exemple of an island component. It's basically like any vue component, except that all this code will be ran only server side.
if you look at the privateKey, this is something that wouldn't work on a universal component because privateKey would be undefined client side.

-->

---
layout: center
---

# So... What's the difference between a server component and an island component ? 🤔


<v-click>

````ts
export const createServerComponent = (name: string) => {
  return defineComponent({
    name,
    inheritAttrs: false,
    props: { lazy: Boolean },
    setup (props, { attrs, slots }) {
      return () => {
        return h(NuxtIsland, {
          name,
          lazy: props.lazy,
          props: attrs
        }, slots)
      }
    }
  })
}
````

</v-click>

<!--
Some of you may have noticed that i've talked a lot about server components and island. What's the difference  ?
They're technically the same. A server component is simply a wrapper around NuxtIsland. They both call the same endpoint to retrieve an island.
And there's a good reason why server components exist on Nuxt.
-->

---


# How to use an island component and a server component ?

`/pages/index.vue`

```vue
<template>
  <div>
    <!-- island -->
    <NuxtIsland name="MyIsland" :props="{id: '123'}" />
    <!-- server component -->
    <MyServerComponent id="123" />
  </div>
</template>
```

<!--
As you can see here, island components and server component are not called in the same way.
If you look at the first line here. 
This is how you call an Island. By usingthe NuxtIsland component.
It receives a prop called name forthe name of the component, a prop props for the props to be passed down to the island and also an optionnal boolean lazy prop.

And then you have a the second line, how you call a server component. It is the same way of using a normal component. And this is the 
purpose of server components on nuxt, it is to keep a good developer experience because you'll get autocompletion which is something that you don't really have on NuxtIsland. 

You can consider ServerComponents as a higher level of islands.

That was pretty much everything for how server and island components works. There's also other features such as

-->

---
layout: center
---

# Slots

```vue
<template>
  <div>
    <!-- island -->
    <NuxtIsland name="MyIsland" :props="{id: '123'}">
      <template #named-slot="{ count }">
        <Counter :count="count" />
      </template>
    </NuxtIsland>
    <!-- server component -->
    <MyServerComponent id="123">
      <div>
        A slot within server component
      </div>
    </MyServerComponent>
  </div>
</template>
```

<!--

Slots !

You can now pass down slots from your client component to an island. BTW Since a slot is rendered by the parent component and if
the parent component is interactive you're slot will also be interactive within the server component

-->

---
layout: center
---

# Payload cache

<!--

We also cache the response of an island component. This means that if you already rendered an island with some props,
NuxtIsland won't call the server but rather load what has been cached.

-->


---
layout: center
---

# Remotes islands

<v-click>

```ts
export default defineNuxtConfig({
  experimental: {
    componentIslands: 'locale+remote',
  }
})
```

</v-click>

<v-click>


```vue
<template>
  <div>
    <NuxtIsland name="MonComposant" source="https://localhost:3001" :props="{id: '123'}" />
  </div>
</template>
```

</v-click>

<!--

Remote islands
Something that was released in 3.7

You are now able to get your component from a remote server. It can be a Nuxt Server or either any other CMS.
The only condition is that the response has to be a NuxtIslandResponse.


-->

---
layout: center
---

# Roadmap

nuxt/nuxt#19772

<!--


This was pretty much everything you can do with islands for now. We do have a roadmap available on github

-->


---
layout: center
---
# Load client components within island components

<!--

first selective client component within server components is coming in a future minor or patch release.
For those who are familiar with Astro, you can compare this feature to the 
`client:load` attribute to make a component imported client-side.


-->

---

<div class="overflow-auto h-full">


```vue
<script setup lang="ts">
const products = await Products.getAll()
</script>

<template>
    <table>
      <tbody>
        <tr v-for="product in products">
          <!-- ... -->
          <td>
            <NuxtLink
              :to="{name: 'ProductDetails', query: product.id}"
              nuxt-client
            >
              See details
            </NuxtLink>
          </td>
        </tr>
      </tbody>
    </table>
</template>
```

</div>

<!--

for exemple
Currently clicking on the link rendered by this server component will make your client do a native navigation. Not handled by the router, it means that you'll reload completly your application.

If you wish to make NuxtLink instantiated client-side, you can use the `nuxt-client` prop/attribute to declare it as interactive.
So once client-side, NuxtIsland will also import the component's JS chunk

-->



---
layout: center
---

# API stabilisation

- Slot refactoring
- `NuxtIslandResponse`

<!--
First we'll start with the API stabilisation so it is easier to understand how NuxtIsland works for new comers.
Then we will also probably change the type of NuxtIslandResponse for remote source so they can have something more ... stable to rely on
-->

---
layout: center
---

# `<ServerOnly>` component

<v-click>

```vue
<template>
  <div>
    hello ! This is a normal component

    <ServerOnly>
      <p>But this will be server only</p>
      <ShowMeMarkdownGeneratedArticle />
    </ServerOnly>  
  </div>
</template>
```

</v-click>

<!--
We also have something called <ServerOnly> component on the roadmap.
This component will give you the ability to make parts of your universal component server only.
-->

---
layout: center
---

# Deep client components

<!--

And then deep client components is also something planned for the future. 
I talked about client components within island. This means that it is something only possible to declare a component as client 
within server components for now.
The reason this hasn't been made on the current PR is because i wish to have some feedback before propagating the nuxtclient prop to a whole app

-->

---
layout: center
---

<ul>
<li>
 <mdi-github /> @huang-julien
</li>
<li> 
  <ic-baseline-discord /> @chakraecho or Julien Huang on the Nuxt discord
</li>
<li> 
  <mdi-twitter /> @JulienHuang_dev
</li>
</ul>


<!--

to wrap up, island and server component are not enabled by default. This is not something planned since it would bring too much complexity. 

-->
