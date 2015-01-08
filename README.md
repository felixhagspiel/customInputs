Browser's regular checkboxes and radios are often ugly or may just not fit into your design. In this turorial I show you how to apply your own styling for radios and checkboxes using CSS and [fontawesome icons](http://fontawesome) for the checkmark. Feel free to change the styling according to your needs (depending on your icon font and your default font you have to do it anyway). In addition I explain how create an on/off switch. I use CSS3, but I provide a fallback to render older browser's default checkbox and radio (IE8). For better code structure I am using [SCSS (SASS)](http://sass-lang.com/guide) in this tutorial, but you can find the compiled CSS at the end.

###[Checkout the Demo!](http://custom-inputs.felixhagspiel.de/)

##Checkboxes and radios
I use prefixes for my code so I don't have to worry about overriding third party code. I use `fh-` in this example, but of course you can use your own. As the styles for checkboxes and radios are pretty much the same, I put them together to avoid code duplication. For better readability I only show the code belonging to the current step and replace the rest of the code with `// ...`. You can find the complete code at the end. 

Let's begin! As styling of the default input elements is limited, we will just hide them completely and create our own styling depending on the state of the input.

Lets start with the markup. Please note that the order of the `input` and `label` elements is important!

    <!-- Checkbox -->
    <span class="fh-checkbox">
        <input type="checkbox" id="checkbox-id" name="mycheckbox">
        <label for="checkbox-id">My checkbox label</label>
    </span>

    <!-- Radio -->
    <span class="fh-radio">
        <input type="radio" id="radio-id1" name="myradiogroup">
        <label for="radio-id1">My radio label</label>
    </span>
    <span class="fh-radio">
        <input type="radio" id="radio-id2" name="myradiogroup">
        <label for="radio-id2">My second radio label</label>
    </span>

Now we create a SCSS-file and start with the actual styling. First we define color and margin variables at the top of the file to make changing the look of our elements easier:

    $color-default: #849FBB; // default color
    $color-default-light: #DDDDDD; // default color light
    $color-active: #57CB85; // color when active or checked
    $color-active-light: #9EFFC4; // active color light
    $color-focus: #6FB5FB; // color when focused

    $margin-el: 7px; // default margin for our custom inputs

To apply the custom styling only to browsers which support it, we use the CSS type selectors `[type="checkbox"]` and `[type="radio"]`. This will cause IE8 to ignore our code and to render the default checkbox and radio:
    
    .fh-checkbox > [type="checkbox"],
    .fh-radio > [type="radio"] {
       // here goes our styling
    }

One important thing we have to do is to hide the default input element because changing its style is limited. We do that by setting `opacity: 0; display: none;`, as only using `display: none` can cause the elements to still show in some browsers:

    .fh-checkbox > [type="checkbox"],
    .fh-radio > [type="radio"] {
        width: 0;
        height: 0;
        display: none;
        opacity: 0;
    }

Now we add some margins and paddings to the label-element to make room for the checkboxes and radios:

    .fh-checkbox > [type="checkbox"],
    .fh-radio > [type="radio"] {
        // ...
        & + label {
            display: inline-block;
            margin-right: $margin-el;
            margin-top: $margin-el;
            margin-bottom: $margin-el;
            padding-left: 22px;
            padding-top: 2px;
            position: relative;
            cursor: pointer;
        }
    }

To add the base styling for the new radios and checkboxes, we use the `:before`-pseudoelement of the label:

    .fh-checkbox > [type="checkbox"],
    .fh-radio > [type="radio"] {
        // ...
        & + label {
            // ...
            &:before {
                // position elements absolute to parent container
                position: absolute;
                display: inline-block;
                bottom: 1px;
                left: 0;
                width: 13px;
                height: 13px;
                border: 2px solid $color-default;
                color: $color-default;
            }
            &:hover:before {
                // add some hover styling
                border-color: $color-default;
            }
        }
    }

Next off we style the focus-state:

    .fh-checkbox > [type="checkbox"],
    .fh-radio > [type="radio"] {
        // ...
        &:focus + label:before {
            border-color: $color-focus;
            box-shadow: 0 0 6px 0 $color-focus;
        }
    }

Now we style the disabled-state by adding some opacity and a `not-allowed` cursor:

    .fh-checkbox > [type="checkbox"],
    .fh-radio > [type="radio"] {
        // ...
        &[disabled] + label {
            cursor: not-allowed;
            opacity: .4;
            &:before {
                opacity: .7;
            }
        }
    }

Now we start with the actual styling. Lets begin with the checkbox. By using the pseudoclasses `:selected` and `:not(:selected)` we can change our label styling depending on the checked state of our hidden input element (it's `:selected` state is still changed by clicking on our label). As we have to differ between `[type="checkbox"]` and `[type="radio"]` we put them in separate blocks outside of our base code:

    // styling for checkbox for both states
    .fh-checkbox > [type="checkbox"] + label:before {
        // set icon font
        font-family: "FontAwesome", sans-serif;
        font-size: 13px;
        text-align: center;
        // add some CSS3-animations
        -webkit-transition: border-color .2s ease-in, background-color .2s ease-in;
        -moz-transition: border-color .2s ease-in, background-color .2s ease-in;
        -o-transition: border-color .2s ease-in, background-color .2s ease-in;
        -ms-transition: border-color .2s ease-in, background-color .2s ease-in;
        transition: border-color .2s ease-in, background-color .2s ease-in;
    }
    // styling for checkbox when selected
    .fh-checkbox > [type="checkbox"]:checked + label:before {
        // set checkmark icon
        content: "\f00c";
        color: #FFF;
        background-color: $color-active;
        border-color: $color-active;
    }
    // styling for checkbox when not selected
    .fh-checkbox > [type="checkbox"]:not(:checked) + label:before {
        // remove checkmark icon
        content: "";
    } 

Now  we do the same for the radio:

    // styling for radio for both states
    .fh-radio > [type="radio"] + label:before {
        content: "";
        border-radius: 15px;    
        // add some CSS3-animations
        -webkit-transition: border-color .2s ease-in, box-shadow .1s ease-in, background-color .2s ease-in, box-shadow .2s ease-in;
        -moz-transition: border-color .2s ease-in, box-shadow .1s ease-in, background-color .2s ease-in;
        -o-transition: border-color .2s ease-in, box-shadow .1s ease-in, background-color .2s ease-in;
        -ms-transition: border-color .2s ease-in, box-shadow .1s ease-in, background-color .2s ease-in;
        transition: border-color .2s ease-in, box-shadow .1s ease-in, background-color .2s ease-in; 
    } 
    // styling for radio when selected
    .fh-radio > [type="radio"]:checked + label:before {
        color: $color-active;
        border-color: $color-active;
        background-color: $color-active;
        // use box-shadow to render circle
        box-shadow: inset 0 0 0 2px #fff;  
    }  
    // styling for radio when not selected
    .fh-radio > [type="radio"]:not(:checked) + label:before {
        box-shadow: inset 0 0 0 3px #fff; 
    }

And voilà, you got your custom styled radios and checkboxes!

##On/off switch
Now let's start with the on/off switch. Its styling is quite similar to the radios and checkboxes. We will use a regular checkbox inside a wrapper `span`. The wrapper contains another `span` element which represents our switch knob.

The markup:

    <span class="fh-switch">
       <input type="checkbox" id="switch-id" name="myswitch">
       <label for="switch-id">My switch label</label>
       <span class="fh-switch-knob"></span>
    </span>

To use absolute positioning we have to set the wrapper relative: 

    .fh-switch {
        position: relative;
    }

Now we hide the default checkbox and style our label, as we did before:

    .fh-switch > [type="checkbox"] {
        width: 0;
        height: 0;
        display: none;
        opacity: 0;
        & + label {
            cursor: pointer;
            display: inline-block;
            margin-right: $margin-el;
            margin-top: $margin-el;
            margin-bottom: $margin-el;
            // apply padding so the switch fits inside the label
            padding-right: 60px;
        }
    }

Next we style the background bar of the switch via the `:after` element of the label: 

    .fh-switch > [type="checkbox"] {
        //...
        & + label {
            //...
            &:after {
                content: "";
                top: 4px;
                right: 10px;
                width: 30px;
                height: 12px;
                // use absolute for better positioning
                position: absolute;
                border-radius: 30px;
            }
        }
    }

Now we add the base styling for the switch knob:

    .fh-switch > [type="checkbox"] {
        //...
        & + label {
            //...
            &+ .fh-switch-knob {
                top: 0;
                width: 20px;
                height: 20px;
                border-radius: 30px;
                display: inline-block;
                position: absolute;
                cursor: pointer;
                pointer-events: none;
                box-shadow: 1px 1px 1px $color-default-light;
                // add some CSS3-animations
                -webkit-transition: right .1s ease-in, background-color .1s ease-in;
                -moz-transition: right .1s ease-in, background-color .1s ease-in;
                -o-transition: right .1s ease-in, background-color .1s ease-in;
                -ms-transition: right .1s ease-in, background-color .1s ease-in;
            }
        }
    }

Here we define the appearence of the switch bar depending on the `checked` state:

    .fh-switch > [type="checkbox"] {
        //...
        &:checked + label:after {
            background-color: $color-active-light;
        }
        &:not(:checked) + label:after {
            background-color: $color-default-light;
        }
    }

And we do the same for the switch-knob:

    .fh-switch > [type="checkbox"] {
        //...
        &:checked + label + .fh-switch-knob {
            right:5px;
            background-color: $color-active;
        }
        &:not(:checked) + label + .fh-switch-knob {
            right: 25px;
            background-color: $color-default;
        }
    }

Now only the `:focus` and `disabled` states are missing:

    .fh-switch > [type="checkbox"] {
        //...
        &:focus + label:after,
        &:focus + label + .fh-switch-knob {
            box-shadow: 0 0 6px 0 $color-focus;
        }
        &[disabled] {
            & + label,
            & + label:after,
            & + label + .fh-switch-knob {
                cursor: not-allowed;
                opacity: 0.4;
            }
        }
    }

And hurray, we are finished! :) The whole file containing radios, checkboxes and the switch should look like this now:

    $color-default: #849FBB; // default color
    $color-default-light: #DDDDDD; // default color light
    $color-active: #57CB85; // color when active or checked
    $color-active-light: #9EFFC4; // active color light
    $color-focus: #6FB5FB; // color when focused

    $margin-el: 7px; // default margin for our custom inputs

    /**
     * Checkboxes & radios
     */
    .fh-checkbox > [type="checkbox"],
    .fh-radio > [type="radio"] {
        width: 0;
        height: 0;
        display: none;
        opacity: 0;
        & + label {
            display: inline-block;
            margin-right: $margin-el;
            margin-top: $margin-el;
            margin-bottom: $margin-el;
            padding-left: 22px;
            padding-top: 2px;
            position: relative;
            cursor: pointer;
            &:before {
                // position elements absolute to parent container
                position: absolute;
                display: inline-block;
                bottom: 1px;
                left: 0;
                width: 13px;
                height: 13px;
                border: 2px solid $color-default;
                color: $color-default;
            }
            &:hover:before {
                // add some hover styling
                background-color: $color-default;
            }
        }
        &:focus + label:before {
            border-color: $color-focus;
            box-shadow: 0 0 6px 0 $color-focus;
        }
        &[disabled] + label {
            cursor: not-allowed;
            opacity: .4;
            &:before {
                opacity: .7;
            }
        }
    }
    // styling for checkbox for both states
    .fh-checkbox > [type="checkbox"] + label:before {
        // set icon font
        font-family: "FontAwesome", sans-serif;
        font-size: 13px;
        text-align: center;
        // add some CSS3-animations
        -webkit-transition: border-color .2s ease-in, background-color .2s ease-in;
        -moz-transition: border-color .2s ease-in, background-color .2s ease-in;
        -o-transition: border-color .2s ease-in, background-color .2s ease-in;
        -ms-transition: border-color .2s ease-in, background-color .2s ease-in;
        transition: border-color .2s ease-in, background-color .2s ease-in;
    }
    // styling for checkbox when selected
    .fh-checkbox > [type="checkbox"]:checked + label:before {
        // set checkmark icon
        content: "\f00c";
        color: #FFF;
        background-color: $color-active;
        border-color: $color-active;
    }
    // styling for checkbox when not selected
    .fh-checkbox > [type="checkbox"]:not(:checked) + label:before {
        // remove checkmark icon
        content: "";
    }
    // styling for radio for both states
    .fh-radio > [type="radio"] + label:before {
        content: "";
        border-radius: 15px;
        // add some CSS3-animations
        -webkit-transition: border-color .2s ease-in, box-shadow .1s ease-in, background-color .2s ease-in, box-shadow .2s ease-in;
        -moz-transition: border-color .2s ease-in, box-shadow .1s ease-in, background-color .2s ease-in;
        -o-transition: border-color .2s ease-in, box-shadow .1s ease-in, background-color .2s ease-in;
        -ms-transition: border-color .2s ease-in, box-shadow .1s ease-in, background-color .2s ease-in;
        transition: border-color .2s ease-in, box-shadow .1s ease-in, background-color .2s ease-in;
    } 
    // styling for radio when selected
    .fh-radio > [type="radio"]:checked + label:before {
        color: $color-active;
        border-color: $color-active;
        background-color: $color-active;
        // use box-shadow to render circle
        box-shadow: inset 0 0 0 1px #fff;  
    }  
    // styling for radio when not selected
    .fh-radio > [type="radio"]:not(:checked) + label:before {
        box-shadow: inset 0 0 0 3px #fff; 
    }
    /**
     * On/off switch
     */
    .fh-switch {
        position: relative;
    }
    .fh-switch > [type="checkbox"] {
        width: 0;
        height: 0;
        display: none;
        opacity: 0;
        & + label {
            cursor: pointer;
            display: inline-block;
            margin-right: $margin-el;
            margin-top: $margin-el;
            margin-bottom: $margin-el;
            // apply padding so the switch fits inside the label
            padding-right: 60px;
            &:after {
                content: "";
                top: 4px;
                right: 10px;
                width: 30px;
                height: 12px;
                // use absolute for better positioning
                position: absolute;
                border-radius: 30px;
            }
            &+ .fh-switch-knob {
                top: 0;
                width: 20px;
                height: 20px;
                border-radius: 30px;
                display: inline-block;
                position: absolute;
                cursor: pointer;
                pointer-events: none;
                box-shadow: 1px 1px 1px $color-default-light;
                // add some CSS3-animations
                -webkit-transition: right .1s ease-in, background-color .1s ease-in;
                -moz-transition: right .1s ease-in, background-color .1s ease-in;
                -o-transition: right .1s ease-in, background-color .1s ease-in;
                -ms-transition: right .1s ease-in, background-color .1s ease-in;
            }
        }
        &:checked + label:after {
            background-color: #9EFFC4;
        }
        &:not(:checked) + label:after {
            background-color: $color-default-light;
        }
        &:checked + label + .fh-switch-knob {
            right:5px;
            background-color: $color-active;
        }
        &:not(:checked) + label + .fh-switch-knob {
            right: 25px;
            background-color: $color-default;
        }
        &:focus + label:after,
        &:focus + label + .fh-switch-knob {
            box-shadow: 0 0 6px 0 $color-focus;
        }
        &[disabled] {
            & + label,
            & + label:after,
            & + label + .fh-switch-knob {
                cursor: not-allowed;
                opacity: 0.4;
            }
        }
    }

Note that the code can be optimized, i.e. you could put the different `&:not(:checked) + label + ...` and the `&:checked + label + ...` of the on/off switch together, but I wrote it like this for better understanding. And of course there are still some other things left you have to do, for example increase the size of the inputs on mobile devices and so on. But this tutorial should give you a good start.

If you encounter any errors in this tutorial or if you think something is not understandable, or if you see something which could be done better otherwise, please create a pull-request on github or [contact me](http://felixhagspiel.de/contact)!

[View SCSS file](https://raw.githubusercontent.com/felixhagspiel/customInputs/master/custom-inputs.scss)

[View CSS file](https://raw.githubusercontent.com/felixhagspiel/customInputs/master/custom-inputs.css)

[Visit github](https://github.com/felixhagspiel/customInputs)