# HTML 

##### REFERENCES
```
https://developer.mozilla.org/en-US/docs/Learn/Forms/How_to_structure_a_web_form
```

##### TEMPLATES

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    
</body>
</html>
```

##### LINK CSS

```
<link href="payment-form.css" rel="stylesheet">
```

##### LINK BOOTSTRAP 5

```
<!-- CSS only -->
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta1/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-giJF6kkoqNQ00vy+HMDP7azOuL0xtbfIcaT9wjKHr8RbDVddVHyTfAAsrekwKmP1" crossorigin="anonymous">

<!-- JavaScript Bundle with Popper -->
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta1/dist/js/bootstrap.bundle.min.js" integrity="sha384-ygbV9kiqUc6oa4msXn9868pTtWMgiQaeYH7/t7LECLbyPA2x65Kgf80OJFdroafW" crossorigin="anonymous"></script>
```

##### RADIO BUTTONS
```
<form>
  <fieldset>
    <legend>Fruit juice size</legend>
    <p>
      <input type="radio" name="size" id="size_1" value="small">
      <label for="size_1">Small</label>
    </p>
    <p>
      <input type="radio" name="size" id="size_2" value="medium">
      <label for="size_2">Medium</label>
    </p>
    <p>
      <input type="radio" name="size" id="size_3" value="large">
      <label for="size_3">Large</label>
    </p>
  </fieldset>
</form>
```

##### WAYS TO ASSOCIATE LABELS

1. SHOWN IN ABOVE TEMPLATE
```
<label for="name">Name:</label> <input type="text" id="name" name="user_name">
```
2. NESTED
```
<label for="name">
  Name: <input type="text" id="name" name="user_name">
</label>
```

##### CHECKBOXES

```
<form>
  <p>
    <input type="checkbox" id="taste_1" name="taste_cherry" value="cherry">
    <label for="taste_1">I like cherry</label>
  </p>
  <p>
    <input type="checkbox" id="taste_2" name="taste_banana" value="banana">
    <label for="taste_2">I like banana</label>
  </p>
</form>
```

##### MULTIPLE LABELS

```
<p>Required fields are followed by <abbr title="required">*</abbr>.</p>

<!-- So this: -->
<div>
  <label for="username">Name:</label>
  <input id="username" type="text" name="username">
  <label for="username"><abbr title="required" aria-label="required">*</abbr></label>
</div>

<!-- would be better done like this: -->
<div>
  <label for="username">
    <span>Name:</span>
    <input id="username" type="text" name="username">
    <abbr title="required" aria-label="required">*</abbr>
  </label>
</div>

<!-- But this is probably best: -->
<div>
  <label for="username">Name: <abbr title="required" aria-label="required">*</abbr></label>
  <input id="username" type="text" name="username">
</div>
```

##### FORM STRUCTURE

```
<form>
<h1>Payment form</h1>
<p>Required fields are followed by <strong><abbr title="required">*</abbr></strong>.</p>
<section>
    <h2>Contact information</h2>
    <fieldset>
      <legend>Title</legend>
      <ul>
          <li>
            <label for="title_1">
              <input type="radio" id="title_1" name="title" value="K" >
              King
            </label>
          </li>
          <li>
            <label for="title_2">
              <input type="radio" id="title_2" name="title" value="Q">
              Queen
            </label>
          </li>
          <li>
            <label for="title_3">
              <input type="radio" id="title_3" name="title" value="J">
              Joker
            </label>
          </li>
      </ul>
    </fieldset>
    <p>
      <label for="name">
        <span>Name: </span>
        <strong><abbr title="required">*</abbr></strong>
      </label>
      <input type="text" id="name" name="username">
    </p>
    <p>
      <label for="mail">
        <span>E-mail: </span>
        <strong><abbr title="required">*</abbr></strong>
      </label>
      <input type="email" id="mail" name="usermail">
    </p>
    <p>
      <label for="pwd">
        <span>Password: </span>
        <strong><abbr title="required">*</abbr></strong>
      </label>
      <input type="password" id="pwd" name="password">
    </p>
</section>

<!--  SECOND SECTION -->

<section>
    <h2>Payment information</h2>
    <p>
      <label for="card">
        <span>Card type:</span>
      </label>
      <select id="card" name="usercard">
        <option value="visa">Visa</option>
        <option value="mc">Mastercard</option>
        <option value="amex">American Express</option>
      </select>
    </p>
    <p>
      <label for="number">
        <span>Card number:</span>
        <strong><abbr title="required">*</abbr></strong>
      </label>
      <input type="tel" id="number" name="cardnumber">
    </p>
    <p>
      <label for="date">
        <span>Expiration date:</span>
        <strong><abbr title="required">*</abbr></strong>
        <em>formatted as mm/dd/yyyy</em>
      </label>
      <input type="date" id="date" name="expiration">
    </p>
</section>

</form>

```




