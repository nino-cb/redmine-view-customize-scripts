# Add a search function to the multiple selectbox

Add a search function to the multiple selectbox.  
複数セレクトボックスに検索機能を追加します。

In this example, the search function is added to the multiple selectbox for a user format custome field.  
この例では、複数選択可のセレクトボックスに検索機能を追加しています。

## Setting

### Path Pattern

None

### Insert Position

Bottom of issue form
<!-- 
Head of all pages
Bottom of issue form
Bottom of issue detail
Bottom of all pages
-->

### Code

JavaScript
<!--
JavaScript
CSS
HTML
-->

```javascript
$(function() {

  const addAutocompleteAfterMultiSelect = function(selectElement) {

    const $select = $(selectElement);
    if ($select.length == 0) {
      return;
    }
  
    const options = $select.find('option[value!=""]')
      .map(function() {
          const $option = $(this);
          return {
            label: $option.text(),
            optionValue: $option.val()
          };
        })
        .toArray();

    const $autocomplete = $('<input type="text" class="ui-autocomplete-input autocomplete" autocomplete="off">');
  
    const applyToSelect = function() {
      const inputValue = $autocomplete.val();
      const matchOption = $.grep(options, function(option) {
        return option.label == inputValue;
      })[0];
      
	  var selValue = $select.val();	  
      if (matchOption != null) {
		if(selValue == '') {
          $select.val(matchOption.optionValue);
		} else {
          var str = selValue + ',' + matchOption.optionValue;
          $select.val(str.split(","));
        } 
		$(selectElement + " option[value=" + matchOption.optionValue + "]").get(0).scrollIntoView({
		  behavior: "auto",
		  block: "nearest",
		  inline: "nearest"
	    });
      } else {
	    $autocomplate.val('');
	  }
    }

    $autocomplete
      .autocomplete({
        source: options,
        minLength: 0,
        select: function(event, ui) {
		  var selValue = $select.val();
		  if (selValue == '') {
            $select.val(ui.item.optionValue);
          } else {
            var str = selValue + "," + ui.item.optionValue;
			$select.val(str.split(","));
          }
		  setTimeout(function(){$(selectElement + " option[value=" + ui.item.optionValue + "]").get(0).scrollIntoView({
			  behavior: "auto",
		      block: "nearest",
		      inline: "nearest"
		  });},50);
        }
      })
      .on('blur ', applyToSelect)
      .on('keypress', function(event) {		 
        if (event.key == 'Enter') {
          return false;
        } 
      });
      $select.after($('<div></div>').append($autocomplete));      
  }

  addAutocompleteAfterMultiSelect('#issue_custom_field_values_1');
});
```

## Note

This can be applied not only to the user format custom field, but also to a list of custom fields, etc.  
ユーザー形式のカスタムフィールドだけでなく、リストのカスタムフィールドなどにも適用できます。

```javascript
addAutocompleteAfterMultiSelect('#issue_custom_field_values_1');
```

## Result

![result](./result.mp4)
