# Account Profiles

A Verida Account maintains a public profile for every application context. The default `basicProfile` currently contains:

* `name` - Name of the account
* `country` - Country where the account owner lives
* `description` - Text description / bio of the Verida account
* `avatar.uri` - The URI of an avatar image. This is currently encoded as a URI that contains a base64 representation of the image. (In the future other ways of storing an image will be supported, beyond a `uri`)

The schema can be found here: [https://common.schemas.verida.io/profile/basicProfile/latest/schema.json](https://common.schemas.verida.io/profile/basicProfile/latest/schema.json)

All properties are optional except for `name`.

This public profile is a datastore that anyone can read from, but only the owner can write to.

In order to make working with profiles easier, there are some helper methods in the Verida SDK that simplify fetching and updating a profile. These are documented below.

## Open any profile[​](https://developers.verida.network/docs/client-sdk/profiles#open-any-profile) <a href="#open-any-profile" id="open-any-profile"></a>

Open the public profile for any Verida Account and context combination using an instance of the `client` object.

For example, open a user’s public profile created in the `Verida: Vault` mobile application:

```typescript
const did = 'did:vda:polyamoy:0x6B2a1bE81ee770cbB4648801e343E135e8D2Aa6F';
const profileConnection = await client.openPublicProfile(did, 'Verida: Vault', 'basicProfile');
const publicProfile = await profileConnection.getMany()

console.log('Account name', publicProfile.name)
console.log('Account country', publicProfile.country)

// Example showing how to inject the avatar into a <img id="avatar-img"> tag
const elm = document.getElementById("#avatar-img")
elm.src = publicProfile.avatar.uri
```

This can be tried out in your browser [here](https://developers.verida.network/docs/tutorial/public\_profile).

It’s also possible to open an external profile for the current context:

```typescript
const did = 'did:vda:polyamoy:0x6B2a1bE81ee770cbB4648801e343E135e8D2Aa6F';
const externalProfile = await context.openProfile('public', did);
```

## Open and modify the current user's profile[​](https://developers.verida.network/docs/client-sdk/profiles#open-and-modify-the-current-users-profile) <a href="#open-and-modify-the-current-users-profile" id="open-and-modify-the-current-users-profile"></a>

Open the user's context profile:

```typescript

const userProfile = await context.openProfile('basicProfile');

// Modify the profile properties
const avatarUri = 'data:image/png;base64,iVBOR...'; //  Data URL of base64-encoded image
await userProfile.set('avatar', { uri: avatarUri, });
await userProfile.set('name', 'Stevie');

// Read the profile properties
const name = await userProfile.get('name');
console.log(name); // -> Stevie
const profileData = await userProfile.getMany();
console.log(profileData); // -> { name: 'Stevie', avatar: { uri: 'data:image/png;base64,iVBOR...' }, ...}

// Delete a profile property
await userProfile.delete('name');

// Listen for profile changes
const listener = await userProfile.listen(function (row) {
 console.log(`${row.key} = ${row.value}`);
});

listener.cancel();
```

{% hint style="info" %}
Learn more about Data URL [here](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics\_of\_HTTP/Data\_URLs).
{% endhint %}

## Advanced[​](https://developers.verida.network/docs/client-sdk/profiles#advanced) <a href="#advanced" id="advanced"></a>

### Open an external profile without authentication[​](https://developers.verida.network/docs/client-sdk/profiles#open-an-external-profile-without-authentication) <a href="#open-an-external-profile-without-authentication" id="open-an-external-profile-without-authentication"></a>

Open an external profile for using an instance of the `client` object directly from the `@verida/client-ts` package

For example, open a user’s public profile created in the `Verida: Vault` mobile application:

```typescript
import { Client } from '@verida/client-ts';
import { Network } from '@verida/types';

const did = 'did:vda:polyamoy:0x6B2a1bE81ee770cbB4648801e343E135e8D2Aa6F';

const client = new Client({
  network: Network.BANKSIA,
});

const profileInstance = await client.openPublicProfile(
 did,
 'Verida: Vault',
 'basicProfile'
);

const profile  = await profileInstance.getMany({}, {});

}
```

Note: Change `Network.BANKSIA` to `Network.MYRTLE` if loading a Myrtle profile.

<figure><img src="../../.gitbook/assets/Screenshot 2024-08-05 130852.png" alt=""><figcaption></figcaption></figure>
