---
title: "{{ .File.ContentBaseName | replaceRE `^\d{4}-\d{2}-` "" | humanize }}"
summary: ""
date: {{ .Date | time.Format "2006-01-02" }}
tags: []
draft: true
---

## Introduction
