# Test-Template

This repository is a template library. It does not install Nunjucks, fetch
data, or generate output files.

An external application should:

1. Configure its Nunjucks loader with the `templates` directory.
2. Render `index.njk`, `category.njk`, or `blog-post.njk`.
3. Pass `page` and `sidebar` objects in the render context.
4. Write the rendered result to its desired static HTML output.

The page templates are:

- `templates/index.njk`
- `templates/category.njk`
- `templates/blog-post.njk`

Shared partials are:

- `templates/partials/shared-header.njk`
- `templates/partials/post-macros.njk`
- `templates/partials/shared-sidebar.njk`
- `templates/partials/shared-footer.njk`

Expected render context:

```js
{
  page: {
    menuTabs: [],
    hotPosts: {
      featured: {},
      secondary: []
    },
    recentPosts: [],
    categorySections: [],
    feedPosts: []
  },
  sidebar: {
    categories: [],
    posts: []
  },
  article: {}
}
```

Post objects use this shape:

```js
{
  image: "./img/post-1.jpg",
  title: "Article title",
  url: "blog-post.html",
  categories: [
    {
      name: "Travel",
      url: "category.html"
    }
  ],
  author: "John Doe",
  authorUrl: "author.html",
  date: "20 April 2018",
  excerpt: "Article summary"
}
```

`blog-post.njk` expects an `article` object:

```js
{
  origin_url: "https://example.com/source",
  brief: "Article summary",
  other_info: {
    location: "San Francisco",
    detail: "Location detail"
  },
  category_detail: "Los Angeles",
  category_detail_id: "category-id",
  language: "en",
  title_img: "https://example.com/title-image.jpg",
  title: "Article title",
  content: "<p>Trusted article HTML</p>",
  rate: "17870",
  id: "article-id",
  time: "2018-07-16"
}
```

Render it by wrapping the database object in the `article` context key:

```js
environment.render("blog-post.njk", {
  article: databaseArticle,
  page: navigationData,
  sidebar: sidebarData
});
```

Optional fields for features not present in the base database object:

```js
{
  category_url: "category.html",
  author: "John Doe",
  author_url: "author.html",
  comments: 3,
  tags: [
    {
      name: "Travel",
      url: "category.html"
    }
  ],
  previous_post: {
    title_img: "./img/widget-8.jpg",
    title: "Previous article",
    url: "blog-post.html"
  },
  next_post: {
    title_img: "./img/widget-10.jpg",
    title: "Next article",
    url: "blog-post.html"
  },
  related_title: "Related Posts",
  related_posts: [
    {
      title_img: "./img/post-4.jpg",
      title: "Related article",
      url: "blog-post.html",
      category_detail: "Travel",
      category_url: "category.html",
      time: "2018-07-16"
    }
  ]
}
```

The `content` field is rendered with Nunjucks `safe`. The external application
must sanitize untrusted HTML before passing it to the template.
