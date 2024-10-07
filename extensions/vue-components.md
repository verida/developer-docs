# Vue Components

#### Using the @verida/vue-account Vue components[​](https://developers.verida.network/docs/extensions/vue-starter-kit#using-the-veridavue-account-vue-components) <a href="#using-the-veridavue-account-vue-components" id="using-the-veridavue-account-vue-components"></a>

Verida supplies open source Vue components for easy app development. See the [Verida Vue Components repo](https://github.com/verida/verida-vue-components/tree/develop/components/account) for comprehensive documentation all options.



{% hint style="info" %}
This component can be customized to suite your application styles and themes. See [the available props](https://github.com/verida/verida-vue-components/tree/develop/components/account#props)
{% endhint %}

{% hint style="warning" %}
This supports Vue 3 only
{% endhint %}

## Usage[​](https://developers.verida.network/docs/extensions/vue-starter-kit#usage) <a href="#usage" id="usage"></a>

Create a base vue project using the command below

```
 vue create myapp
```

install our Verida package.

```
yarn add  @verida/vue-account
```

The `@verida/vue-account` component library registration enables the `vda-account` and `vda-login` component to be accessed across your application.

main.js

```typescript
import { createApp } from 'vue';
import App from './App.vue';
import Account from '@verida/vue-account';


const app = createApp(App);

app.use(Account);

app.mount('#app');

```

* NOTE : You can retrieve the user application `context` from the parameter of the `onConnected` event emitter pass to the component .

This works for both the `vda-login` and `vda-account`

### Using the `vda-login` component[​](https://developers.verida.network/docs/extensions/vue-starter-kit#using-the-vda-login-component) <a href="#using-the-vda-login-component" id="using-the-vda-login-component"></a>

This component is used to handle authentication via Verida Connect, it leverages our `@verida/client-ts` and `@verida/account-web-vault` packages under the hood.

Home.vue

```typescript
<template>
  <div id="app">
    <vda-login
      @onError="onError"
      @onConnected="onSuccess"
      :contextName="contextName"
      :logo="logo"
      :loginText: 'LOGIN_TEXT',
    />
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "Login",
  data() {
    return {
      contextName: "Verida: Account Component",
      logo: "https://assets.verida.io/verida_login_request_logo_170x170.png",
    };
  },
  methods: {
    onSuccess(context) {
     console.log(context)
    },
    onError(error) {
      console.log("Login Error", error);
    },
  },
});
</script>

```

### Using the `vda-account` component[​](https://developers.verida.network/docs/extensions/vue-starter-kit#using-the-vda-account-component) <a href="#using-the-vda-account-component" id="using-the-vda-account-component"></a>

This component is used to display a logged in user's profile details such as name, did and avatar. If the user is not logged in, it will supply a login link and generate the single sign-on login form in the same way the vda-login component does.

* Example code:

Login.vue

```typescript
<template>
  <div id="app">
    <vda-account 
      :logo="logo"
      :contextName="contextName"
      @onLogout="onLogout" 
      @onError="onError"
      @onConnected="onSuccess"
    />
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "Login",
  data() {
    return {
      contextName: "Verida: Account Component",
      logo: "https://assets.verida.io/verida_login_request_logo_170x170.png",
    };
  },
  methods: {
    onLogout() {
      console.log("Handle Logout");
    },
    onSuccess(response: any) {
      // The response is the application context of the connected user..
      console.log(response)
    },
    onError(error) {
      console.log("Login Error", error);
    },
  },
});
</script>
```
