---
layout: default
title: Field Specification
parent: Common
grand_parent: API
permalink: /api/common/field-specification
nav_order: 5
---
# Field Specification
## Resource Name
1-63 characters, support Chinese and English, numbers and - _ .

## Resource notes
0-255 characters, support Chinese and English, numbers and any characters

## Business group
1-63 characters, support Chinese and English, numbers and - _ .

## Password
### Common enquiries:
Support 8-30 characters cannot contain 

```
[A-Z], [a-z], [0-9] and [()`~!@#$%^&*-+=_|{}[]:;'<>,.?/]
``` 
illegal characters for:
- Linux-specific requirements:
  - Need to contain two or more at the same time: uppercase letters, lowercase letters, numbers, symbols
- Windows-specific requirements:
  - Need to contain three or more items at the same time: uppercase letters, lowercase letters, numbers, symbols
  - It cannot contain more than 2 consecutive characters in the username (`administrator`), such as adm/min, etc.