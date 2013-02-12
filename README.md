This plugin is forked from Amir's original textarea autocomplete plugin found here: http://www.amirharel.com/2011/03/07/implementing-autocomplete-jquery-plugin-for-textarea/

## Using the plugin
The plugin has one function areacomplete which takes one argument with the following attributes:

	wordCount {Number} the number of words the user want to for matching it with the dictionary
	mode {String} set "outter" for using an autocomplete that is being displayed in the outter layout of the textarea, as opposed to inner display
	highlight {Boolean} whether to highlight the searched string in the suggestion list
	triggers {optional Array} the list of characters that "trigger" completion when typed at the beginning of a word
							  triggers excludes wordCount
	on {Object} containing the followings:
		query {Function} will be called to query if there is any match for the user input
	 		The function will be called with parameters:
	 		- text {String} the word(s) that need to be completed
	 		- cb {Function} the callback that must be called to return results. Call it with either
	 		  - a list of strings
	 		  - a list of objects having attributes: display (what to show in the dropdown) and value (what to insert in the textarea)
	 		- trigger {optional String} the leading character that triggered this autocompletion request (if any)
		valueChanged {optional Function} will be called to notify when the content has changed
		selection {Function} takes 2 parameters - a string value of the selected text and the data attached to the selection. May return a string to replace the selection.

You can also style the list as you like by setting your own styles in the auto.css file.

## Styling the drop down menu
The plugin create a drop down menu to show the suggestion you have found for the current word that the user is typing. In order to style the to fits your needs, lets see the markup of the list:

```html
<ul class="auto-list" style="left: 91px; top: 113px; display: none;">
	<li data-value="daniel"><mark>d</mark>aniel</li>
	<li data-value="david"><mark>d</mark>avid</li>
</ul>
```

So as you can see from this example, i set the drop down list with a "auto-list" class name, so you can style it however you want.
Here is the CSS i applied for the purpose of the demos:

	ul.auto-list{
		display: none;
		position: absolute;
		top: 0px;
		left: 0px;
		border: 1px solid green;
		background-color: #A3DF99;
		padding: 0;
		margin:0;
		list-style:none;
	}
	ul.auto-list > li:hover,
	ul.auto-list > li[data-selected=true]{
		background-color: #236574;
	}

	ul.auto-list > li{
		border: 1px solid gray;
		cursor: default;
		padding: 2px;

	}

	mark{
		font-weight: bold;
	}


Here's an example: [fiddle](http://jsfiddle.net/EssPA/)

```javascript
var urls = [
	"facebook.com",
	"google.com",
	"twitter.com",
	"amirharel.com",
	"amazon.com",
	"microsoft.com",
	"quora.com",
	"walla.co.il",
	"ebay.com",
	"gowala.com",
	"myspace.com",
	"youtube.com"
];

var people = [
	"john",
	"jane",
	"bob"
];

function searchIn(text, list) {
    var res = [];
    for (var i=0; i<list.length; i++) {
        var listValue = list[i];
        if (listValue.toLowerCase().indexOf(text.toLowerCase()) == 0) {
            var resitem = {
                display: listValue.toUpperCase(),
                value: listValue
            };
            res.push(resitem);
        }
    }
    return res;
}

function initTextarea() {
    $("textarea").areacomplete({
        triggers: ['@','#'],
        mode: "outter",
        highlight: true,
        on: {
            query: function (text, cb, trigger) {
                if (trigger == '#') {
                    cb(searchIn(text, urls));
                } else if (trigger == '@') {
                    cb(searchIn(text, people));
                } else {
                    cb([]);
                }
            },
            valueChange: function (text) {
                console.log('Value has changed to: ' +  text);
            }
        }
    });
}

$(document).ready(function(){
	initTextarea();
});
```
