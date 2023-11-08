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

#  Introduction to Nuxt server components and Nuxt islands

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

  - Frontend developer @ Leetchi
  - Nuxt Contributor and Nuxt Team
  - Developer since 3 years
  - Doing open-source since a year and a half

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

<!--
I think everyone knows what nuxt is here.
Nuxt is a full-stack meta framework built upon Vue. It's main purpose is to server render your Vue application.
Meaning that your components will be rendered server side AND client side. And there's a process for that called hydration.
-->

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
  <p>Astro Islands</p>
  </div>
  <div flex flex-col >
  <skill-icons-nuxtjs-dark  mx-auto />
  <p>NuxtIslands</p>
</div>

</div>

<!--

React introduced two years ago with react ... 18 and next JS the react server components? On their side, they are using an ast to describe what has to be rendered
On astro side, it's a bit particular, everything is static by default and then you declare which component is interactive.
And on our side, we've got the nuxt island which has been release in 3.2 and it keep gettingmore feature update after update.

-->

---

# Advantages

<!--

- no client bundle
- safe server code within the component

-->

---

# Drawbacks

<!--

- well... it's server only
- delayed updated
- complexity

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

Since we retrieve a server component thorugh an endpointan Island component a isolated from your application even in SSR. 
This means that the component has it's own vueApp and nuxtApp context. 

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
Soem of you may think that it's just like any vue component.
But if you look carefully at line 6-7, there's a line commented with call to db. Let's admit you're using an ORM and that
`Articles.getByid` is a call to a database. This is something that won't work on a normal component because it will also be ran client-side.
The workaround would be to have an endpoint exposed to retrieve an article.
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

This was something that was not possible on the initial implementation of Nuxtislands.
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

This was pretty much everything you can do with islands for now. But there's also one new feature that is coming in 3.8 or 3.9

-->

---
layout: center
---
# Load client components within island components

<!--

Which is the ability to declare component called within server as client.

For those who are familiar with Astro, you can compare this feature to the 
`client:load` attribute to make a component imported client-side.

Why is it necessary in some cases ?

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

Well nothing's better than a small example. 
If you look at this island component, let's admit this is a list of product and that you wish to have a NuxtLink within your last column

The current behavior is that this will be static. And if you click on the anchor tag client-side, this will navigate you to the page but with a hard
navigation meaning you'll have to completly reload the app once again.

If you wish to make NuxtLink instantiated client-side, you can use the `nuxt-client` prop/attribute to declare it as interactive.
So once client-side, NuxtIsland will also import the component's JS chunk

-->

---
layout: center
---

# Roadmap

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
Daniel began something on one of his livestream but i'm not sure what's the current state of it. But it looked quite amazing.
The example here is that everything wrapped within server only such as theShowMeMarkdowngeneratedArtcile will be only rendered server side
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

# To wrap up

<!--

to wrap up, island and server component are not enabled by default. This is not something planned since it would bring too much complexity. 
Islands are here to avoid loading a bunch of javascript for components that are heavy and not interactive.
Now that you know what's a server and island component on nuxt
i guess that the next talk will be a demo...

-->
