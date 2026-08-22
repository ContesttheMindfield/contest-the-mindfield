---
title: '{{ replace .File.ContentBaseName "-" " " | title }}'
date: {{ .Date }}
draft: true
type: 'post'
description: ''
# Use stable profile slugs from content/<language>/authors/.
authors: []
categories: []
tags: []
translationKey: ''
toc: true
# Optional intentional cover. Always provide alt text when a cover is set.
# cover: 'cover.webp'
# alt: 'Describe the cover image.'
# Add history entries only when there is a meaningful editorial change.
# history:
#   - date: {{ .Date }}
#     author: ''
#     note: 'Initial publication'
---
