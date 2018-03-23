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

Parameter|Type|Description
------------|------------|----
formName|TEXT|
tablePtr|POINTER|

### How does it work?

A designated method (``On Drop`` database method by default) is used as a launch pad to open the form editor. The original code is stored in an interprocess variable (why not, this is interpreted mode only) and then the code is replaced by the form name token (optionally prefixed with a table token) and ``METHOD OPEN PATH`` is called to open it. ``POST KEY`` is used to select all, goto definition and close the method editor.

Some tricks are used to coordinate the timing. First, the macro even ``on_load`` is used to trigger the method that posts the keyboard shortcuts. It is also used to differentiate the context, whether the method being used as launch pad or if it is being edited in a normal way. The macro ``on_close`` is also used to restore the original code that was replaced with the "launch code". 

Because editor windows take time to open, there is no guarantee that the launch pad window is open when the macro runs, or if the form editor is frontmost instead of the method editor. So a new process is started to wait until the launch pad method editor is available and ready to process the "go to definition" keyboard shortcut. This is important because the same keyboard shortcut opens the Explorer window unless a method editor window is the frontmost in design mode.

### More

If you don't like the delegate method opening, perhaps you could edit the form location json file so that it is displayed off screen.
