+++
title = "让emacs具有mac下的按键绑定"
date = "2012-11-27T13:28:00+08:00"
comments = true
categories = ["programming", "tools"]
tags = ["linux", "emacs"]
description = ""
+++


```lisp
(setq *true-mac-cut-buffer* "")
(setq *true-mac-cut-buffer2* t)

(setq interprogram-cut-function
      '(lambda (str push)
         (setq *true-mac-cut-buffer* str)
         (setq *true-mac-cut-buffer2* push)))

(setq interprogram-paste-function
      '(lambda () nil))

(defun true-mac-cut-function () (interactive)
  (if mark-active
      (progn 
        (true-mac-copy-function)
        (kill-region (point) (mark)))
    (beep)))
        
(defun true-mac-copy-function () (interactive)
  (if mark-active
      (mac-cut-function 
       *true-mac-cut-buffer*
       *true-mac-cut-buffer2*)
    (beep)))

(defun true-mac-paste-function () (interactive)
  (if mark-active
      (kill-region (point) (mark)))
  (insert (mac-paste-function)))

(global-set-key [?\A-x] 'true-mac-cut-function)
(global-set-key "\S-c" 'true-mac-copy-function)
(global-set-key [?\A-v] 'true-mac-paste-function)

(global-set-key [?\A-a] 'mark-whole-buffer)
(global-set-key [?\A-s] 'save-buffer)
(global-set-key [?\A-S] 'write-file)
(global-set-key [?\A-p] 'ps-print-buffer)
(global-set-key [?\A-o] 'find-file)
(global-set-key [?\A-q] 'save-buffers-kill-emacs)
(global-set-key [?\A-w] 'kill-buffer-and-window)
(global-set-key [?\A-z] 'undo)
(global-set-key [?\A-f] 'isearch-forward)
(global-set-key [?\A-g] 'query-replace)
(global-set-key [?\A-l] 'goto-line)
(global-set-key [?\A-m] 'iconify-frame)
(global-set-key [?\A-n] 'new-frame)
```
<!--more-->