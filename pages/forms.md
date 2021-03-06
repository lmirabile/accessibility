---
title: Forms
description: 'How we work with forms'
permalink: /forms/
page_title: Forms
---
Making forms accessible is a simple process. Each form element should be associated with its instructions and errors, and everything should be accessible via the keyboard. 

### Testing 

1. Identify each form element
2. Find all instructions associated with each element
  * __If a form element isn't programatically associated with ALL instructions, this is a failure__
3. Ensure all field elements are accessible via the keyboard
  * __If the form cannot be filled out with just a keyboard, this is a failure__
4. Check for title tags
  * Title tags can be a substitute for labels
  * __If the title tag provides all the related information it passes, if it provides extra information it fails.__
    * Title tags are not accessible via keyboard


### Examples
#### Passes

<fieldset>
<legend>Name</legend>
<label for="firstname">First&nbsp;</label><input type='text' id='firstname'><br>
<label for="lastname">Last&nbsp;</label><input type='text' id='lastname'>
</fieldset>

<fieldset>
<legend>Favorite Soup?</legend>
<input type='radio' name='soup' value='pea' id='peasoup' title='Pea Soup'>&nbsp;Pea Soup<br>
<input type='radio' name='soup' value='chicken' id='chicken' title='Chicken Noodle'>&nbsp;Chicken Noodle<br>
<input type='radio' name='soup' value='tomato' id='tomato' title='Tomato'>&nbsp;Tomato
</fieldset>
<br>

```html
<fieldset>
<legend>Name</legend>
<label for="firstname">First&nbsp;</label><input type='text' id='firstname'><br>
<label for="lastname">Last&nbsp;</label><input type='text' id='lastname'>
</fieldset>

<fieldset>
<legend>Favorite Soup?</legend>
<input type='radio' name='soup' value='pea' id='peasoup' title='Pea Soup'>&nbsp;Pea Soup<br>
<input type='radio' name='soup' value='chicken' id='chicken' title='Chicken Noodle'>&nbsp;Chicken Noodle<br>
<input type='radio' name='soup' value='tomato' id='tomato' title='Tomato'>&nbsp;Tomato
</fieldset>
```
> ___Name:___ Each form element has a ```label```, and its associated with the ```for``` tag. The ```for``` tag refers to the ```id``` of the ```input```. When looking at this form, 'First' and 'Last' wouldn't make since without 'Name.' This is associated with the ```fieldset``` and ```legend```. All elements are wrapped in a ```fieldset```. There can only be one ```legend``` tag per ```fieldset```. Anything in the ```legend``` tag will be associated.

> ___Favorite Soup:___ ```Fieldset``` and ```legend``` is often used for radio buttons as its the easiest way to associate the radio buttons with the question. Notice there are no ```label```s for the radio buttons, but each button has a ```title``` tag for assistive technology to read. 

#### Fails

<fieldset>
<legend>Name</legend>
<label for="first_name">First&nbsp;</label><input type='text' id='firstname'><br>
<label for="1lastname">Last&nbsp;</label><input type='text' id='1lastname'>
</fieldset>

<fieldset>
<legend>Favorite Soup?</legend>
<p><span style='color:red;'>This Question Is Required</span></p>
<input type='radio' name='soup' value='pea' id='peasoup' title='Chick Pea Soup'>&nbsp;Pea Soup<br>
<input type='radio' name='soup' value='chicken' id='chicken' title='Chicken Noodle'>&nbsp;Chicken Noodle<br>
<input type='radio' name='soup' value='tomato' id='tomato' title='Tomato'>&nbsp;Tomato
</fieldset>
<br>

```html
<fieldset>
<legend>Name</legend>
<label for="first_name">First&nbsp;</label><input type='text' id='firstname'><br>
<label for="1lastname">Last&nbsp;</label><input type='text' id='1lastname'>
</fieldset>

<fieldset>
<legend>Favorite Soup?</legend>
<p><span style='color:red;'>This Question Is Required</span></p>
<input type='radio' name='soup' value='pea' id='peasoup' title='Chick Pea Soup'>&nbsp;Pea Soup<br>
<input type='radio' name='soup' value='chicken' id='chicken' title='Chicken Noodle'>&nbsp;Chicken Noodle<br>
<input type='radio' name='soup' value='tomato' id='tomato' title='Tomato'>&nbsp;Tomato
</fieldset>
<br>
```

> ___Failure:___ First name label ```for``` and ```id``` don't match.

> ___Failure:___ Last name has an invalid ```id```.

> ___Failure:___ "This Question Is Required" is not associated with the form fields

> ___Failure:___ The ```title``` tag for Pea Soup indicates it's 'Chick Pea Soup.' This information is not available to keyboard, sighted users.


#### How ARIA afects form inputs

Screen readers vary on what they read and the additional information they provide by default. This is a broad summary of what is read based on VoiceOver for Mac OSX.

You can test these with your own screen reader. If you have a OSX you can turn voice over on by hitting command+f5.

**TL;DR** Using `aria-label` or `aria-labeledby` will cause a screen reader to only read them and not the default label. If you want an input to read from multiple things like an error message, use `aria-labeledby` and pass it the `for` attribute of the label and any aditional `id`s you want read. ex. `aria-labeledby='car1 car_description car-error-message'`

##### No ARIA

Reads just the `label` and not the description

<label for="car_1">Car</label>
<input type="text" id="car_1"/><br/>
<span id='carmakedescription'><i>Please enter Make and Model</i></span>

```html
<label for="car_1">Car</label>
<input type="text" id="car_1"/><br/>
<span id="carmakedescription_1"><i>Please enter Make and Model</i></span>
```

**Screen Reader reads input as:** `Car Edit text`
<hr>

##### With aria-label

Reads the `aria-label` and doesn't read the normal `label`.

<label for="car_2">Car</label>
<input type="text" id="car_2" aria-label="Car, please enter make and model" /><br/>
<span id='carmakedescription_2'><i>Please enter Make and Model</i></span>

```html
<label for="car_2">Car</label>
<input type="text" id="car_2" aria-label="Car, please enter make and model" /><br/>
<span id="carmakedescription_2"><i>Please enter Make and Model</i></span>
```

**Screen Reader reads input as:** `Car, please enter make and model Edit text`
<hr>

##### With aria-labeledby pointing at `carmakedescription`

Reads only the `aria-labeledby` attribute and not the default label

<label for="car_3">Car</label>
<input type="text" id="car_3" aria-labeledby="carmakedescription_3" /><br/>
<span id='carmakedescription_3'><i>Please enter Make and Model</i></span>

```html
<label for="car_3">Car</label>
<input type="text" id="car_3" aria-labeledby="carmakedescription_3" /><br/>
<span id='carmakedescription_3'><i>Please enter Make and Model</i></span>
```

**Screen Reader reads input as:** `Please enter Make and Model Edit text`
<hr>

##### With aria-labeledby pointing at `car carmakedescription`

Reads both labels indicated by the `aria-labeledby` attribute

<label for="car_4">Car</label>
<input type="text" id="car_4" aria-labeledby="car_4 carmakedescription_4" /><br/>
<span id='carmakedescription_4'><i>Please enter Make and Model</i></span>

```html
<label for="car_4">Car</label>
<input type="text" id="car_4" aria-labeledby="car_4 carmakedescription_4" /><br/>
<span id='carmakedescription_4'><i>Please enter Make and Model</i></span>
```

**Screen Reader reads input as:** `Car Please enter Make and Model Edit text`
<hr>

##### With aria-describedby pointing at `carmakedescription`

Voiceover only reads the label, Jaws should read the description as well

<label for="car_5">Car</label>
<input type="text" id="car_5" aria-describedby="carmakedescription_5" /><br/>
<span id='carmakedescription_5'><i>Please enter Make and Model</i></span>

```html
<label for="car_5">Car</label>
<input type="text" id="car_5" aria-describedby="carmakedescription_5" /><br/>
<span id='carmakedescription_5'><i>Please enter Make and Model</i></span>
```

**Screen Reader reads input as:** `Car Edit text`
<hr>


