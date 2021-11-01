yarn init
yarn add lerna -D 
lerna init
yarn global add @vue/cli


demo/package.json to configure:
```json
{
    "private": true, // Prohibition of publication

    "publishConfig": {
       "access": "publish" // If the module needs to be published, for the scope module, it needs to be set to publish; otherwise, permission verification is required
    }
}
```