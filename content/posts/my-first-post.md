+++
title = "My first post"
author = ["mayapony"]
tags = ["tag1"]
categories = ["category1"]
draft = false
+++

This is my post body


## 使用use-package {#使用use-package}

```elisp
(use-package ox-hugo
  :ensure t   ;Auto-install the package from Melpa
  :pin melpa  ;`package-archives' should already have ("melpa" . "https://melpa.org/packages/")
  :after ox)
```
