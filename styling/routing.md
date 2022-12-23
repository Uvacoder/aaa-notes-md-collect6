# 🧹 Routing 

The de-facto standard library for routing React applications is
[React Router](https://reacttraining.com/react-router). It's terrific.

📜 **NOTE: We're using version 6 of the router which should be released soon.**

Most of the docs still apply:

https://reacttraining.com/react-router/web/guides/quick-start

but there are some differences, so as you're looking around at documentation,
reference this first:

https://github.com/ReactTraining/react-router/blob/v6.0.0-alpha.2/docs/installation/getting-started.md
https://github.com/ReactTraining/react-router/tree/v6.0.0-alpha.2/docs

The idea behind routing on the web is you have some API that informs you of
changes to the URL, then you react (no pun intended) to those changes by
rendering the correct user interface based on that URL route. 

In addition, you can change the URL when the user performs an action (like clicking a link or
submitting a form). This all happens client-side and does not reload the browser.
