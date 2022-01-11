<!--
title:   SvelteKit+Prismaで作成したアプリをVercelにデプロイするとエンドポイントが404になる件への対応
tags:    Svelte,SvelteKit,Vercel,prisma
id:      7faea0dad145c6b92b9f
private: false
-->
## やりたかったこと

タイトル通りですが、[SvelteKit](<(https://kit.svelte.dev/)>)と[Prisma](https://www.prisma.io/)で構成したアプリを Vercel にデプロイして動かしてみたところ、ページから API への通信が 404 エラーで失敗していました。
SvelteKit はいわば、Next.js の Svelte 版で`/routes`以下に`.(js|ts)`ファイルを配置するとエンドポイントとして利用することができますが、ここへの通信が失敗していました。

## 解決方法

以下のように`postbuild`コマンドを package.json に追加する。

```json:package.json
{
  "scripts": {
      ...
    "build": "svelte-kit build",
    + "postbuild": "cp prisma/schema.prisma .vercel_build_output/functions/node/render/ && cp node_modules/@prisma/engines/*query* .vercel_build_output/functions/node/render/",
      ...
  }
}

```

## 参考

<https://github.com/sveltejs/kit/issues/1547>
<https://github.com/prisma/prisma/issues/7019>