# recipe-abe-data-1
This recipe shows how you can select pages containing the same attributes

![Screenshot](/site/screenshot.png?raw=true)

# Installation
1. git clone https://github.com/Abejs/recipe-abe-data-1.git
2. cd recipe-abe-data-1
3. abe serve -i
4. Enjoy

# Description
In this recipe, you'll see how you can select other contents from your blog based on attributes of your post.

The example is based on recipes and ingredients:
- You create ingredients with the "product" template
- You create recipes with the "recipe" template. In this template, a drop down list of existing products is displayed. You can choose multiple products (ingredients) for your recipe. Based on this products, a list of recipes containing some of the same ingredients are automatically displayed.

# The recipe template
## How it works

```
{{abe type="data" key="ingredients" source="select title,cover_300x200 from /products where `abe_meta.template`=`product`" desc="select ingredients" display="{{title}}"}}
```
This request will search all ingredients (products) in the directory /products and will display the list in the editor.

```
{{abe type="data" key="other_recipes" source="select title,cover_300x200 from /recipes where `ingredients[].title` IN (`{{ingredients}}`) AND `abe_meta.link`!=`{{abe_meta.link}}`" editable="false"}}
```
This request will take all the products selected in this recipe and will search all the recipes containing one or all of these products. The attribute editable="false" will hide the result from the editor

```
<!DOCTYPE html>
<html>
<head><title>RECIPE</title></head>
<body>
<h1>{{abe type="text" key="title" desc="recipe"}}</h1>
{{abe type="image" key="cover" desc="image" thumbs="300x200" visible="false" reload="true"}}
<img src="{{cover_300x200}}"/>

<h3>ingredients</h3>
<div> {{#each ingredients}} <p>{{title}}</p> {{/each}} </div>
<hr>

<h3>OTHER RECIPES with the same ingredients</h3>
<div> {{#each other_recipes}} <p>{{title}}</p> <img src="{{cover_300x200}}"/> {{/each}} </div>
</body>
</html>
```

The selected ingredients are displayed with a {{#each}} statement

The other recipes containing one or more ingredients of this recipe are displayed also.

