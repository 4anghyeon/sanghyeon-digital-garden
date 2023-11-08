---
{"tags":["React"],"dg-publish":true,"dg-hide":true,"permalink":"/front-end/react/context-api/","hide":true,"dgPassFrontmatter":true,"noteIcon":""}
---

# Props의 문제점 (Prop Drilling)
리액트에서는 일반적으로 위에서 아래, 즉 부모에서 자식으로 props를 통해 데이터를 전달한다.

그러나 앱이 커지고, 컴포넌트가 복잡해질 수록 전달해야 하는 props의 깊이는 더 깊어지고, 많은 컴포넌트를 거쳐야 데이터가 전달될 수 있다.

예를 들어, 접점이 없는 두 컴포넌트가 `state`를 전달하기 위해서는 두 컴포넌트가 공통으로 가지는 부모 컴포넌트까지 올라가야 한다.