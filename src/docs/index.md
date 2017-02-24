# Tempus Dominus - Core v5 Docs

<div class="alert alert-info">
    <strong>Note</strong>
    This is in Alpha and subject to a lot of change as I try to figure out how I want namespacing to be and how the picker can be improved and should be treated as unstable.
</div>

## Understanding "Core"
These documents represent the functions and options specific to the Core module. Each UI module should contain these docs and their own code examples and requirements.

Unless you are interested in developing your own UI module you should refer to the docs for the UI module you're using.

## Implementing Your Own Module

Let's pick a part the Bootstrap 3 module.

First we'll setup a constant variable with the name of our module e.g. `TempusDominusBootstrap3`. Notice that we're also creating a `JQUERY_NO_CONFLICT`. This is important if you want to support cases where `datetimepicker` might refer to your module or `jquery-ui` for instance.
```
const TempusDominusBootstrap3 = ($ => { // eslint-disable-line no-unused-vars
    const JQUERY_NO_CONFLICT = $.fn[DateTimePicker.NAME],
```

Next we will create a class with our module name that extends `DateTimePicker`. Extending the base class is how we will get the functions from Core into our module. In the constructor we're going to call the `super`'s constructor and initialize our module
```
    class TempusDominusBootstrap3 extends DateTimePicker {
        constructor(element, options) {
            super(element, options);
            this._init();
        }

        _init() {
            if (this._element.hasClass('input-group')) {
                // in case there is more then one 'input-group-addon' Issue #48
                const datepickerButton = this._element.find('.datepickerbutton');
                if (datepickerButton.length === 0) {
                    this.component = this._element.find('.input-group-addon');
                } else {
                    this.component = datepickerButton;
                }
            }
        }
```

The rest of the functions are basically all about the UI of the widget. How and where you will place the widget. Will  you use a modal? Will you use a drop down?
```
        _getDatePickerTemplate() {
            return '';
        }

        _getTimePickerMainTemplate() {
            return '';
        }

        _getTimePickerTemplate() {
            return '';
        }

        _getToolbar() {
            return '';
        }

        _getTemplate() {
            return '';
        }

        _place() {
            //place the widget on the screen near the textbox or a modal or whatever
        }

        _fillDow() {
            //populate widgets Day of the Week headers. E.g. Sunday, Monday, ....
        }

        _fillMonths() {
            //populate widgets Month View
        }

        _updateMonths() {
            //handle change to date
        }

        _updateYears() {
            //handle change to date
        }

        _updateDecades() {
            //handle change of data
        }

        _fillDate() {
            //fill days view and update Months, Years, Decades
        }

        _fillHours() {
            //fill hour view
        }

        _fillMinutes() {
            //fill minutes view
        }

        _fillSeconds() {
            //fill seconds
        }

        _fillTime() {
            //fill time selection screen and update Hours, Minutes, Seconds
        }

        _doAction(e, action) {
            //handle various element actions
        }

        //public
        hide() {
            //hide widget
        }

        show() {
            //show widget
        }

        destroy() {
            //destory widget, this is often the place to detach any bindings etc
        }

        disable() {
           //disable widget
        }

        enable() {
            //enable widget
        }

        toolbarPlacement(toolbarPlacement) {
            //handle changing the placement of the toolbar (where the Today, Clear, Select Time buttons are)
        }

        widgetPositioning(widgetPositioning) {
            //handle change of widget positioning
        }

        widgetParent(widgetParent) {
            //handle change of widget's parent element (e.g. a div)
        }
```

Lastly below are some basic jQuery functions for initializing the widget with options and calling functions.
```
        //static
        static _jQueryInterface(option, argument) {
            return this.each(function () {
                let data = $(this).data(DateTimePicker.DATA_KEY);
                if (typeof option === 'object') {
                    $.extend({}, DateTimePicker.Default, option);
                }

                if (!data) {
                    data = new TempusDominusBootstrap3($(this), option);
                    $(this).data(DateTimePicker.DATA_KEY, data);
                }

                if (typeof option === 'string') {
                    if (data[option] === undefined) {
                        throw new Error(`No method named "${option}"`);
                    }
                    data[option](argument);
                }
            });
        }
    }

    /**
    * ------------------------------------------------------------------------
    * jQuery
    * ------------------------------------------------------------------------
    */
    $(document).on(DateTimePicker.Event.CLICK_DATA_API, DateTimePicker.Selector.DATA_TOGGLE, function () {
        let $target = getSelectorFromElement($(this));
        if ($target.length === 0) {
            return;
        }
        TempusDominusBootstrap3._jQueryInterface.call($target, 'toggle');
    }).on(DateTimePicker.Event.CHANGE, `.${DateTimePicker.ClassName.INPUT}`, function (event) {
        let $target = getSelectorFromElement($(this));
        if ($target.length === 0) {
            return;
        }
        TempusDominusBootstrap3._jQueryInterface.call($target, '_change', event);
    }).on(DateTimePicker.Event.BLUR, `.${DateTimePicker.ClassName.INPUT}`, function (event) {
        let $target = getSelectorFromElement($(this));
        if ($target.length === 0) {
            return;
        }
        const config = $target.data(DateTimePicker.DATA_KEY);
        if (config._options.debug) {
            return;
        }
        TempusDominusBootstrap3._jQueryInterface.call($target, 'hide', event);
    }).on(DateTimePicker.Event.KEYDOWN, `.${DateTimePicker.ClassName.INPUT}`, function (event) {
        let $target = getSelectorFromElement($(this));
        if ($target.length === 0) {
            return;
        }
        TempusDominusBootstrap3._jQueryInterface.call($target, '_keydown', event);
    }).on(DateTimePicker.Event.KEYUP, `.${DateTimePicker.ClassName.INPUT}`, function (event) {
        let $target = getSelectorFromElement($(this));
        if ($target.length === 0) {
            return;
        }
        TempusDominusBootstrap3._jQueryInterface.call($target, '_keyup', event);
    }).on(DateTimePicker.Event.FOCUS, `.${DateTimePicker.ClassName.INPUT}`, function (event) {
        let $target = getSelectorFromElement($(this));
        if ($target.length === 0) {
            return;
        }
        const config = $target.data(DateTimePicker.DATA_KEY);
        if (!config._options.allowInputToggle) {
            return;
        }
        TempusDominusBootstrap3._jQueryInterface.call($target, config, event);
    });

    $.fn[DateTimePicker.NAME] = TempusDominusBootstrap3._jQueryInterface;
    $.fn[DateTimePicker.NAME].Constructor = TempusDominusBootstrap3;
    $.fn[DateTimePicker.NAME].noConflict = function () {
        $.fn[DateTimePicker.NAME] = JQUERY_NO_CONFLICT;
        return TempusDominusBootstrap3._jQueryInterface;
    };

    return TempusDominusBootstrap3;
})(jQuery);

```