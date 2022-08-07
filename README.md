# use-packages-examples

This repository collects community contributed `use-package` configuration examples, covering from
Emacs built-in packages to 3rt party packages.

## Built-in packages

### env

Set environment variables:

```el
(use-package env
  :ensure nil
  :config
  (setenv "PYTHONIOENCODING" "utf-8"))
```

### mule and mule-cmds

Set default input method and coding system:

```el
(use-package mule
  :ensure nil
  :init
  (setq default-input-method 'MacOSX)
  :config
  (set-terminal-coding-system 'utf-8)
  (set-keyboard-coding-system 'utf-8))

(use-package mule-cmds
  :preface (provide 'mule-cmds)
  :config
  (set-language-environment 'UTF-8))
```

### ns-win

Set Mac modifiers:

```el
(use-package ns-win
  :ensure nil
  :init
  (setq mac-command-modifier 'meta
        mac-option-modifier 'super))
```

### hippie-exp

Set keybinding to `M-/` and customize the list of expansion functions:

```el
(use-package hippie-exp
  :ensure nil
  :bind ("M-/" . hippie-expand)
  :init
  (setq hippie-expand-try-functions-list
        '(try-expand-dabbrev
          try-expand-dabbrev-visible
          try-expand-dabbrev-all-buffers
          try-expand-dabbrev-from-kill
          try-complete-file-name-partially
          try-complete-file-name
          try-expand-all-abbrevs
          try-expand-list
          try-expand-line
          try-complete-lisp-symbol-partially
          try-complete-lisp-symbol)))
```

### avoid

Move up mouse when cursor comes:

```el
(use-package avoid
  :ensure nil
  :config
  (mouse-avoidance-mode 'animate))
```

### tool-bar, scroll-bar, menu-bar

Disable all of them:

```el
(use-package tool-bar
  :ensure nil
  :config
  (tool-bar-mode -1))

(use-package scroll-bar
  :ensure nil
  :config
  (scroll-bar-mode -1))

(use-package menu-bar
  :unless (eq system-type 'darwin)
  :ensure nil
  :config
  (menu-bar-mode -1))
```

### time

```el
(use-package time
  :ensure nil
  :init
  (setq display-time-day-and-date t
        display-time-24hr-format t
        display-time-use-mail-icon t
        display-time-interval 10)
  :config
  (display-time))
```

### shell

Kill shell buffer when shell exits:

```el
(use-package shell
  :ensure nil
  :hook (shell-mode . my-shell-mode-hook-func)
  :config
  (defun my-shell-mode-hook-func ()
    (set-process-sentinel (get-buffer-process (current-buffer))
                          'my-shell-mode-kill-buffer-on-exit))
  (defun my-shell-mode-kill-buffer-on-exit (process state)
    (message "%s" state)
    (if (or
         (string-match "exited abnormally with code.*" state)
         (string-match "finished" state))
        (kill-buffer (current-buffer)))))
```

### paren

```el
(use-package paren
  :ensure nil
  :init
  (setq show-paren-style 'parentheses)
  :config
  (show-paren-mode t))
```

## 3rd party packages

### [exec-path-from-shell](https://github.com/purcell/exec-path-from-shell)

Copy specified environment variables:

```el
(use-package exec-path-from-shell
  :if (memq window-system '(mac ns x))
  :ensure t
  :init
  (setq exec-path-from-shell-variables '("PATH" "MANPATH" "GOPATH", "PYTHONPATH))
  :config
  (exec-path-from-shell-initialize))
```

### [doom-themes](https://github.com/hlissner/emacs-doom-themes)

```el
(use-package doom-themes
  :ensure t
  :init
  ;; Global settings (defaults)
  (setq doom-themes-enable-bold t    ; if nil, bold is universally disabled
        doom-themes-enable-italic t) ; if nil, italics is universally disabled
  :config
  (load-theme 'doom-one t)

  ;; Enable flashing mode-line on errors
  (doom-themes-visual-bell-config)

  (use-package doom-themes-ext-treemacs
    :init
    (setq doom-themes-treemacs-theme "doom-colors")) ; use the colorful treemacs theme)
  (doom-themes-treemacs-config)

  ;; Corrects (and improves) org-mode's native fontification.
  (doom-themes-org-config))
```

### [doom-modeline](https://github.com/seagle0128/doom-modeline)

```el
(use-package doom-modeline
  :ensure t
  :init
  (setq doom-modeline-minor-modes t
        doom-modeline-vcs-max-length 20)
  :config
  (doom-modeline-mode 1))
```

### [highlight-indent-guides](https://github.com/DarthFennec/highlight-indent-guides)

```el
(use-package highlight-indent-guides
  :ensure t
  :delight highlight-indent-guides-mode
  :init
  (setq highlight-indent-guides-method 'character
        ;; default is \x2502 but it is very slow on Mac
        highlight-indent-guides-character ?\xFFE8
        highlight-indent-guides-responsive 'top))
```

### [rainbow-delimiters](https://github.com/Fanael/rainbow-delimiters)

Hook to `prog-mode`:

```el
(use-package rainbow-delimiters
  :ensure t)

(use-package prog-mode
  :ensure nil
  :hook ((prog-mode . rainbow-delimiters-mode)))
```

### [projectile](https://github.com/bbatsov/projectile)

```el
(use-package projectile
  :ensure t
  :bind-keymap ("s-p" . projectile-command-map)
  :init
  (setq projectile-mode-line-function '(lambda () (format " [%s]" (projectile-project-name))))
  :config
  (projectile-mode +1))
```

### [company](https://github.com/company-mode/company-mode)

```el
(use-package company
  :ensure t
  :delight company-mode
  :demand t
  :init
  (setq company-idle-delay 0.1
        company-minimum-prefix-length 1)
  :bind (:map company-active-map
         ("C-n" . company-select-next)
         ("C-p". company-select-previous))
  :config
  (global-company-mode))
```
