# Amazing Unicorn Startup

<p align="center">
    <!-- <a href="" target="_blank"> -->
        <img width="100%" src="https://github.com/varunswarup0/amazing-unicorn-startup/blob/master/amazingUnicornStartup.png" alt="Amazing Unicorn Startup">
    <!-- </a> -->
</p>

[![License: MPL 2.0](https://img.shields.io/badge/License-MPL%202.0-brightgreen.svg)](https://opensource.org/licenses/MPL-2.0)
[![Maintenance](https://img.shields.io/badge/Maintained%3F-yes-green.svg)](https://GitHub.com/Naereen/StrapDown.js/graphs/commit-activity)
[![Ask Me Anything !](https://img.shields.io/badge/Ask%20me-anything-1abc9c.svg)](https://GitHub.com/Naereen/ama)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square)](http://makeapullrequest.com)
[![Open Source Love svg1](https://badges.frapsoft.com/os/v1/open-source.svg?v=103)](https://github.com/ellerbrock/open-source-badges/)

In the base `master` branch, we have a mostly functional form. But, there are two problems.

1. Using individual state hooks for ever field is getting a bit cumbersome. It would be cool if we had something closer to `this.setState`.
2. There is no real form validation.

We're going to solve each of these in two different chapters of this workshop.

We've see that we can create our own custom hooks.

Could we create a hook that mostly works like `this.setState`.

**Spoiler alert**: Yes, we can—and we're going to do it using `useReducer`.

## Creating the Hook

```js
import { useReducer } from "react";

const reducer = (previousState = {}, updatedState = {}) => {
  return { ...previousState, ...updatedState };
};

const useSetState = (initialState = {}) => {
  const [state, dispatch] = useReducer(reducer, initialState);

  const setState = (updatedState) => dispatch(updatedState);

  return [state, setState];
};

export default useSetState;
```

We could take it a step further by using a more standard action patter, but I'm not going to.

## Using It in the Application

We just make an object for the initial state of the form.

```js
const initialState = {
  userName: "",
  email: "",
  password: "",
  passwordConfirmation: ""
};
```

We can add a dymanic function for changing the state based on the field name.

```js
const handleChange = (event) => {
  setState({
    [event.target.name]: event.target.value
  });
};
```

And now, each input field can be refactored somewhere along the lines of this first one.

```js
<input
  id="userName"
  name="userName"
  type="text"
  value={state.userName}
  required
  onChange={handleChange}
/>
```

## Without a Reducer

You could also implement this without a reducer, but I prefer the previous patter.

```js
import { useState, useCallback } from "react";

const useSetState = (initialState) => {
  const [state, set] = useState(initialState);

  const setState = useCallback(
    (updatedState) => {
      set((previousState) => ({ ...previousState, ...updatedState }));
    },
    [set]
  );

  return [state, setState];
};

export default useSetState;
```

[![ForTheBadge uses-js](http://ForTheBadge.com/images/badges/uses-js.svg)](http://ForTheBadge.com)
[![ForTheBadge built-by-developers](http://ForTheBadge.com/images/badges/built-by-developers.svg)](https://GitHub.com/Naereen/)
[![forthebadge cc-by-nd](http://ForTheBadge.com/images/badges/cc-by-nd.svg)](https://creativecommons.org/licenses/by-nd/4.0)
<a href="https://www.linkedin.com/in/varun-swarup/">
    <img src="https://img.shields.io/badge/Support-Recommed%2FEndorse%20me%20on%20Linkedin-yellow?style=for-the-badge&logo=linkedin" alt="Recommend me on LinkedIn" /></a>

