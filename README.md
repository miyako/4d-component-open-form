# 4d-component-open-form
Design mode component to open form editor by code

### Version

<img src="https://cloud.githubusercontent.com/assets/1725068/18940649/21945000-8645-11e6-86ed-4a0f800e5a73.png" width="32" height="32" /> 

* Interpreted mode only (``METHOD SET CODE``)  

* Uses ``Macros v2``

## Syntax

```
METHOD_OPEN_FORM (formName{;tablePtr})
```

limitations

does not work if caller method is paused by break point or ``TRACE``

Parameter|Type|Description
------------|------------|----
formName|TEXT|
tablePtr|POINTER|

### How does it work?

A designated method (``On Drop`` database method by default) is used as a launch pad to open the form editor. The original code is stored in an interprocess variable (why not, this is interpreted mode only) and then the code is replaced by the form name token (optionally prefixed with a table token) and ``METHOD OPEN PATH`` is called to open it. ``POST KEY`` is used to select all, goto definition and close the method editor.
