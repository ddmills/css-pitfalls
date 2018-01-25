# css-pitfalls


# 1. Over qualified selectors

> Using IDs and chaining selectors with high specificity, especially to set context

```html
<ul id="cookies">
  <li>...</li>
  <li>...</li>
  <li class="danger-item">veggies</a>
</ul>
```

```scss
#cookies a {
  color: orange;
}

.danger-item { // won't work
  color: red
}

#cookies .danger-item { //okay it works now!
  color: red
}
```

### The problem
* Styles under an ID are difficult to re-use (id's are unique on a page)
* In order to re-style any `li` under `#cookies`, it will need an even higher specificity
* Once the Id is there, the rule won't easily go away (usually it will breed more ids)
* can lead to the use of `!important`

### The solution
* Avoid ids (?)
* Avoid chaining, consider adding a new class to flatten the structure
* If you have to increase specificity, consider something lighter than an ID or `!important` (tag qualifier)

# 2. Top-down CSS (coupled to DOM structure)

> CSS that mirrors the DOM

```html
<section class="main-content">
  <div>
    <a>...</a>
  </div>
  ...
</section>
```

```scss
.main-content {
  div:first-child a {
    color: red
  }
}
```

### The problem
* This leads to rules that get lost or disrupted as soon as the DOM changes
* the css *depends* on the dom

### The solution
* Focus on abstracting common (and uncommon!) design components
* Consider composition (modifier classes like `-danger`)

```html
<section class="main-content">
  <div>
    <a class="link link--danger">...</a>
  </div>
  ...
</section>
```

```scss
$danger: red;

.link {
  &--danger {
    color: $danger;
  }
}
```

# 3. Duplication of rules (DRY Violation)
> Duplicating sets of rules to multiple selectors

```scss
.comment-box {
   padding: 10px;
   margin: 10px;
   border: 1px solid #ccc;
}

.video-container {
   padding: 10px;
   margin: 10px;
}
```

### The problem
* rules get duplicated (more code, more to maintain!)
* duplication of rules = duplication of errors

### The solution
* consider a modifier class (composition)
* extract to a variable
* group the selectors (`.comment-box, .video-container { ... }`)

```scss
.padding {
  &--comfy {
    padding: 10px;
    margin: 10px;
  }
  
  &--spacious {
    padding: 30px;
    margin: 30px;
  }
}
```

# 4. Overriding existing rules

> aka, Undoing a rule or multiple that have already been applied (CSS rules, are by nature, cascading!)

```scss
.btn {
  font-weight: 700;
  font-size: 1.4rem;
  border: 1px solid;
  padding: 12px;
}

.btn.cancel-btn {
  font-size: 1rem;
  padding: 0;
}
```

# The problem
* rules were likely applied too early
* more lines of code
* unnecessary rules

# The solution
* this is okay for `normalizing` rules across browsers (`user-agent.css`) - see normalize.css!
* reduce the specificity of the parent
* consider inverting the rules (more modifier classes):

```scss
.btn {
  font-weight: 700;
  border: 1px solid;
  
  &&--lg {
    font-size: 1.4rem;
    padding: 12px;
  }
}
```

# 5. Magical numbers

> magical numbers that are based on an aggregate of preceding or sibling elements

### The problem
* `position: absolute; left -14px;`
* highly coupled to other styles
* magical numbers are often not mobile friendly

### The solution
* Think about "flow", how the element should be positioned *relative* to other elements
* ask an expert (?)

# 6. Modifier classes, gone too far!

> writing classes that have little semantic meaning

```html
<p class="text text--center text--lg text--subtle">~this is fancy pantsy text~</p>
```

### The problem
* CSS classes that have little semantic meaning (HTML now depends on the CSS)
* class overload

### The solution
* consider renaming or combining modifiers into a class with a semantic meaning (`.lead` in this case)



