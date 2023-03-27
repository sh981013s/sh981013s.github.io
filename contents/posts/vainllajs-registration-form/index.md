---
title: "회원가입 폼 with Vanilla JS"
description: "회원가입 폼 with Vanilla JS"
date: 2021-11-24
update: 2021-11-24
tags:
- VanillaJS
---

## Intro

리액트를 공부하거나, 프로젝트를 했으로때 주로 간단한 ui 적인 부분은 라이브러리를 사용하여 해결 했었지만, vanilla JS 를 사용하여 내가 직접 가능한 부분은 공부하여 만들어 보려고 하였다.

---

## Main

- [소스코드](https://github.com/sh981013s/Projects-with-VanillaJs/tree/main/Form-Validator)
- [liveDemo](https://hwani-vanillajs.netlify.app/form-validator/)


![](form_val.gif)

---

## HTML

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport"
        content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <link rel="stylesheet" href="./styles.css">
  <title>Form Validator</title>
</head>
<body>
<div class="container">
  <form action="" id="form" class="form">
    <h2>Register With Us</h2>
    <div class="form-control">
      <label for="username">Username</label>
      <input type="text" id="username" placeholder="Enter username">
      <small>Error message</small>
    </div>
    <div class="form-control">
      <label for="email">Email</label>
      <input type="text" id="email" placeholder="Enter email">
      <small>Error message</small>
    </div>
    <div class="form-control">
      <label for="password">Password</label>
      <input type="password" id="password" placeholder="Enter password">
      <small>Error message</small>
    </div>
    <div class="form-control">
      <label for="password">Confirm Password</label>
      <input type="password" id="password2" placeholder="Enter password again">
      <small>Error message</small>
    </div>
    <button>Submit</button>
  </form>
</div>
<script src="script.js"></script>
</body>
</html>
```

## JS

```javascript
const form = document.getElementById('form');
const username = document.getElementById('username');
const email = document.getElementById('email');
const password = document.getElementById('password');
const password2 = document.getElementById('password2');

// Show input error message
const showError = (input, message) => {
  const formControl = input.parentElement;
  formControl.className = 'form-control error';
  const small = formControl.querySelector('small');
  small.innerText = message;
}

// Show success outline
const showSuccess = (input) => {
  const formControl = input.parentElement;
  formControl.className = 'form-control success';
}

// Check email is valid
const isValidEmail = (email) => {
  const re = /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
  if (re.test(email.value)) {
    showSuccess(email)
  } else {
    showError(email, 'Email is not valid');
  }
}

// Get field name
const getFieldName = (input) => {
  return input.id.charAt(0).toUpperCase() + input.id.slice(1);
}

// Check required fields
const checkRequired = (inputArr) => {
  inputArr.forEach(input => {
    if (input.value.trim() === '') {
      showError(input, `${getFieldName(input)} is required`);
    } else {
      showSuccess(input);
    }
  })
}

// Check input length
const checkLength = (input, min, max) => {
  if (input.value.length < min) {
    showError(input, `${getFieldName(input)} must be at least ${min} characters`)
  } else if (input.value.length > max) {
    showError(input, `${getFieldName(input)} must be at less than ${max} characters`)
  }
}

// Check passwords match
const checkPasswordsMatch = (input1, input2) => {
  if(input1.value !== input2.value) {
    showError(input2, 'Password do not match');
  }
}

// Event listeners
form.addEventListener('submit', e => {
  e.preventDefault();

  checkRequired([username, email, password, password2]);
  checkLength(username, 8, 15);
  checkLength(password, 6, 25);
  isValidEmail(email);
  checkPasswordsMatch(password, password2);
});
```