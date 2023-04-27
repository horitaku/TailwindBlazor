

create your project

```
dotnet new blazorserver -o {YourProject}
cd {YourProject}

npm init -y
npm i tailwindcss postcss autoprefixer postcss-cli daisyui
npx tailwindcss init

```

add `postcss.config.js` 

``` js
module.exports = {
  plugins: {
    tailwindcss: {},
    autoprefixer: {},
  }
}
```

edit `tailwind.config.js`

```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: ["./**/*.{html,razor,cshtml}"],
  theme: {
    extend: {},
  },
  plugins: [require("daisyui")],
}
```

`{YourProject}/wwwroot/css` フォルダ配下をすべて削除する。

`{YourProject}/wwwroot/css/app.css` を作成する。

```css
@tailwind base;
@tailwind components;
@tailwind utilities;

#blazor-error-ui {
    background: lightyellow;
    bottom: 0;
    box-shadow: 0 -1px 2px rgba(0, 0, 0, 0.2);
    display: none;
    left: 0;
    padding: 0.6rem 1.25rem 0.7rem 1.25rem;
    position: fixed;
    width: 100%;
    z-index: 1000;
}

    #blazor-error-ui .dismiss {
        cursor: pointer;
        position: absolute;
        right: 0.75rem;
        top: 0.5rem;
    }
```

`{YourProject}/package.json` を修正
```json
{
    ...
    "scripts": {
        ...,
        "buildcss": "postcss wwwroot/css/app.css -o wwwroot/css/app.min.css"
    },
    ...
｝
```

cssファイルを生成する。
```bash
npm run buildcss
```

`{YourProject}/Pages/_Host.cshtml` を修正

```html
    <!-- <link rel="stylesheet" href="css/bootstrap/bootstrap.min.css" />
    <link href="css/site.css" rel="stylesheet" /> -->
    <link href="css/app.min.css" rel="stylesheet" />
```
※ あわせてhtmlタグのlangをjaに変更

`{YourProject}.csproj` を修正
```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <Target Name="PostBuild" AfterTargets="PostBuildEvent">
    <Exec Command="npm run buildcss" />
  </Target>

  ...

</Project>
```

`{YourProject}/Pages/Index.razor` を修正
```html
@page "/"

<PageTitle>Index</PageTitle>

<h1 class="text-3xl font-bold underline">
    Hello, world!
</h1>

Welcome to your new app.
```