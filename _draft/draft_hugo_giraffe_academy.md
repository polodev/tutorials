#6
hugo new dir/file.md
hugo server -D // it will show draft page as well


page 2 type single page and list pages

hugo made automatic listing first lavel director
for making automatic listing we have to make _index.md inside subdirectory

# 7
front matter
# 8 achchetypes
what will be frontmatter in newly created file using hugo command 
hugo new --kind chapter basics/_index.md

# 9 - shortcode
{{< shortcodeName paramOfShortcode >}}
{{ < youtube youtube_video_id > }}
 
# 10 - taxonomies
default tags  and categories 
---
moods: ['happy', 'upbeat'] 
---
inside config.toml file 
[taxonomies]
  tag="tags"
  categories="categories"
  mood="moods"
 
# 11 - template in hugo
# 12 - list page
layouts/_default/list.html
 
{{ .Content }}
{{ range .Pages}}
  <a href="{{.URL}}">{{.Title}}</a>
{{ end }}

# single page 13
layouts/_default/single.html
{{.Content}}
  {{.Title}}
  {{.Date}}
# homepage 14
layouts/index.html

# different template
keep  template and content folder name same 
https://gohugo.io/templates/section-templates/
# 16 - base templates and block
// layoutes/_default/baseof.html
// baseof.html
<body>
  {{ block "main" . }}
    replacable content
  {{ end }}
</body>
// inside single or list page
{{ define "main" }}
  content from single o
{{ end }}

# 17 hugo variable 
{{ $myValue := "aString" }}
{{ $myValue }}
visit hugo site variable page 

# 18 - function 
{{ fnName param1, param2 }}
{{ trancate 10, "this is a really strong character "}}
{{ add 1 5 }}
{{ singularize "dogs" }}

# 19 if statement 

{{ $var1 := "dog" }}
{{ $var2 := "cat" }}
{{ if eq $var1 $var2 }}
  true block
{{ else}}
 false block
{{ end }}
 
{{ if and (conditionOne) (condition2) }}
{{ end }}

{{ if or (conditionOne) (condition2) }}
{{ end }}

 
# 20 -  Data 

{{ range .Sites.Data.states  }}
  {{.name}} <br> {{ .capital }} <br>
{{ end }}

# 21 - partials
partials/header.html

{{ partial <partialName> <scope> }}
{{ partial header.html . }}

// passing value 
{{ partial header.html (dict "myTitle" "mytitleValue" "myDate" "myDateValue")  }}  // dict is peculier 

# 22 - shortcode 

layouts/shortcodes/myshortcode.html
{{ .Get "color" }} // when passing key
{{ .Get 0 }} // when not passing key it will access by postion
{{.Inner}} // getting value inside of the tags
// to use 
{{< myshortcode color="blue" >}}
{{< myshortcode blue >}}
 
// for text conent
{{< myshortcode >}}
 some bold text
{{< /myshortcode >}}

{{% myshortcode %}}
 some **bold** text where markdown will affected
{{% /myshortcode %}}
 
 
# 23 - building your site

hugo 
====================================================================================================
# menu creation in hugo
====================================================================================================

---
menu: "mainmenu"
  //or
menu:
  mainmenu:
    name: 'Page1'
---
  
{{range .Sites.Menus.mainmenu }}
  <a href="{{.URL}}">{{.Title}}</a>
{{end}}
 
// inside config.toml
sectionPagesMenu = "mainmenu"  // for tags, categories and other taxonomies

If I need a custom menu
// inside config.toml
[menu]
  [[menu.mainmenu]]
  identifier = "home"
  name = "Home"
  url= "/"
  weight= 1
 
  [[menu.mainmenu]]
  identifier = "articles"
  name = "Articles"
  url= "/articles/"
  weight= 2
 
  [[menu.mainmenu]]
  identifier = "recipies"
  name = "Recipes"
  url= "/recipes/"
  weight= 3

====================================================================================================
# pagination in hugo
====================================================================================================
{{ range first 2 .Pages }}
{{end}}

// config.tom
paginate = 2
// list.html
 {{range .Paginator.Pages }}
 {{ end }}
 {{ partial "pagination" . }}
// partials/pagination.html
 
https://codepen.io/vikrammehta/pen/VBvqWg?editors=1100
https://codepen.io/vikrammehta/pen/YjydEM













