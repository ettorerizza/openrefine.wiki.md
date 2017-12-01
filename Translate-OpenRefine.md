# Internationalization / Localization

You can [https:_hosted.weblate.org/engage/openrefine/?utm\_source=widget_](help+translating+OpenRefine+into+your+language+with+Weblate).

![Translation status](https://hosted.weblate.org/widgets/openrefine/-/287x66-grey.png)

## Front End

The OpenRefine front end has been localized using [https:_github.com/newage/jquery-i18n_](jquery-i18n+plugin). The localized text is stored in a JSON dictionary on the server and retrieved with a new OpenRefine command.

### Adding a new string

There should be no hard-coded language strings in the HTML or JSON used for the front end. If you need a new string, first check the existing strings to make sure there isn't an equivalent that you can reuse. This cuts down on the amount of text which needs to be translated.

If there's no string you can reuse, allocate an available key in the appropriate translation dictionary and add the default string, e.g.

```javascript ..., "dictionaryname":{ "newkey":"new default string for this key", }, ... ```

and then set the text (or HTML) of your HTML element using i18n helper method. So given an HTML fragment like: ```html <label id="new-element-id">[untranslated text would have appeared here before]</label> ``` we could set it's text using: ```javascript $('#new-html-element-id').text($.i18n.\_('dictionaryname')["newkey"]); ``` or, if you need to embed HTML tags: ```javascript $('#new-html-element-id').html($.i18n.\_('dictionaryname')["newkey"]); ```

### Adding a new language

The language dictionaries are stored in the <tt>langs</tt> subdirectory for the module e.g.

- [https://github.com/OpenRefine/OpenRefine/tree/master/main/webapp/modules/langs/](https://github.com/OpenRefine/OpenRefine/tree/master/main/webapp/modules/langs/) for the main interface
- [https://github.com/OpenRefine/OpenRefine/tree/extensions/gdata/module/langs/](https://github.com/OpenRefine/OpenRefine/tree/extensions/gdata/module/langs/) for google spreadsheet connection

To add support for a new language, copy <tt>translation-en.json</tt> to <tt>translation-&lt;locale&gt;.json</tt> and have your translator translate all the value strings (ie right hand side). This is best done [https:_hosted.weblate.org/engage/openrefine/?utm\_source=widget_](with+Weblate).

By default, the system tries to load the language file corresponding to the currently in-use browser language. To override this setting a new menu item ("Language Settings") has been added at the index page. To support a new language file, the developer should add a corresponding entry to the dropdown menu in this file: <tt>/OpenRefine/main/webapp/modules/core/scripts/index/lang-settings-ui.html</tt>. The entry should look like: ```javascript <option value="<locale>">[Language Label]</option> ```

NOTE: Gettext .po files are better supported for translation, so in the future we may switch to those and use a PO to JSON converter at build time.

Currently supported languages include English, Spanish, Chinese, French, Hebrew, Italian and Japanese.

## Server / Back end

Additional work needs to be done on to localize the Refine server, so things like error messages, undo history, etc may appear in non-localized (ie English) form.

