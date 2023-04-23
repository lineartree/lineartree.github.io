# Build Blog with Hugo


Build a blog with Hugo and DoIt theme.

<!--more-->

## Install Hugo

```bash
yay -S hugo
```

```bash
hugo new site my_website
cd my_website
```

add [DoIt :(fas fa-up-right-from-square fa-2xs):](https://hugodoit.pages.dev/) theme

```bash
git clone https://github.com/HEIGE-PCloud/DoIt.git themes/DoIt
```


{{< admonition question "What to do if the favicon doesn't show up on the website">}}
try `Ctrl` + `F5` to refresh the cache

or add `?v=2`

```html
<link rel="shortcut icon" type="image/x-icon" href="/favicon.ico?v=2">
````

{{< /admonition >}}


