---
layout: post
title: "Linux Shell"
description: "Notes on Linux shell."
date: 2017-02-26
tags: [programming, Linux, notes]
comments: true
share: true
---

### 《Linux Shell Scripting Cookbook》

1. `echo` and `printf`

	Do not include ! within double quotes when use `echo`<br>
	`$ echo "hello!"`
	
	This command will return<br>
	`bash: !: event not found error`
	
	There are several equivalent ways to use `echo`:
	
	`$ echo "hello"; echo hello; echo 'hello'`.
	
	Usage of `printf` is the same as that in C.
	
	
