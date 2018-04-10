### The ```form``` tag
- submitting a form generates an HTTP request
- remember to include ```action``` and ```method``` attributes to configure the request that gets sent

```
<form action="/some_path_on_your_server" method="post">
...
</form>
```

### Form Fields

```<input type=" " name=" " value=" " id=" "/>```

- ```type```, ```name```, ```value```, ```id``` attributes
- ```id``` must match with ```for``` attribute of corresponding label tag
- types include ```text```, ```radio```, ```checkbox```, ```hidden```, ```file```, ```submit```, ```color```, ```email```, ```url```, ```range```, ```tel```


```<label for=" ">...</label>```

- ```for``` attribute must match with ```id``` attribute of corresponding field
- this is what the reader will see in the UI. It describes the data that will be in the input
- you can wrap the label around the corresponding field to make the use experience better


```<textarea name=" " id=" "></textarea>```

- ```name``` and ```id``` attributes
- whitespace sensitive
- the ```name``` corresponds to the attr_reader of the server

```
<select name=" " id=" ">
<option value=" ">text that the user sees</option>
<!--- more options -->
</select>
```

- ```name```, ```id``` attributes on ```<select>```
- ```value``` attribute on ```<option>```

```
<fieldset>
<legend>description of what data this fieldset is for</legend>
<!-- fields go here-->
</fieldset>
```
- ```<legend>``` to have better semantics of the HTML (ie. for screen readers)

### The ```params``` Hash
- Sinatra's gift to you
- contains all data from submitted form
  - the keys come from the name attributes of your fields
  - the values come from your user

```
{title: "This is the title the user entered", description: "this is the description the user typed in"}
```

### PUT, PATCH, DELETE Requests
- annoyingly complicated!
Here's the workaround for Sinatra:

```
<input type="hidden" name="_method" value="put" />
```
```
<input type="hidden" name="_method" value="patch" />
```
```
<input type="hidden" name="_method" value="delete" />
```

### Redirect vs Render
General guideline:

- render a response at the end of GET request blocks
  - ```erb :name_of_erb_file```
- redirect the user (to a GET route) at the end of all other (POST, PUT, PATCH, DELETE) request blocks
  - ```redirect to('/whatever_url')```
  - redirecting tells the server to send a new get request to itself
