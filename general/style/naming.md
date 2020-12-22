# Naming things (variables, methods, classes, modules)

~ Philosophy of Software Design

- Create an image 
  - if user has no context of declaration, documentation, or surrounding code), can the user guess what the name refers to?

- Be precise! Do not use vague names (e.g. num, str)
  - Variables should convey WHAT they represent (nouns) and not verbs
  - If it's difficult to find a name, then maybe the variable is conveying too many things. A hint that you do not have a clean design.

- Consistency!
  - Use the same terminology so readers understand the same context
  - Reduces cognitive load
  - Always use the same common name for the common purpose!
  - Never use common name for anything else other than that common purpose!
  - Make sure both name and purpose and narrow enough so when it is mentioned, it conveys the same behavior!