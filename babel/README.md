# Babel multi-language support


## For Translators
You'll be working with a `messages.po` file for each specific language. It lists all the text that is displayed in Specter in English with a "msgstr"

Translators can use the free tool [Poedit](https://poedit.net/download) to add their translations to the `messages.po` file for each language. The translation files can be found organized by two-letter language code (e.g. Spanish = "es") under `src/cryptoadvance/specter/translations`.

Load the `messages.po` file for your target langage (e.g. Spanish: [src/cryptoadvance/specter/translations/es/LC_MESSAGES/messages.po](../src/cryptoadvance/specter/translations/es/LC_MESSAGES/messages.po)) in Poedit, add or update translations, and then save your changes.

The updated `messages.po` file should be submitted as a pull request (PR) to the [Specter-Desktop github repo](https://github.com/cryptoadvance/specter-desktop).

TODO: record tutorial screencast for how to do the github submission.



## For Developers:
### "Wrapping" text for translation
All you have to do in your code is wrap each piece of English text with the `ugettext` shorthand `_()`:
* Wrap jinja template text: `<p>Hello, world!</p>` becomes `<p>{{ _('Hello, world!') }}</p>`
* Wrap python strings: `error="No device was selected."` becomes `error=_("No device was selected.")`

There are more complex workarounds for strings that are dynamically constructed as well as locale-specific date and number formatting.

TODO: Link to resource


### Rescanning for text that needs translations
Re-generate the `messages.pot` file:
```
pybabel extract -F babel/babel.cfg -o babel/messages.pot .
```
This will rescan all wrapped text, picking up new strings as well as updating existings strings that have been edited.

Then run `update`:
```
pybabel update -i babel/messages.pot -d src/cryptoadvance/specter/translations
```

Any newly wrapped text strings will be added to each `messages.po` file. Altered strings will be flagged as needing review to see if the existing translations can still be used.

Once the next round of translations is complete, recompile the results:
```
pybabel compile -d src/cryptoadvance/specter/translations
```


### Adding support for another language
Assuming you have `extract`ed an updated `messages.pot`, all you have to do is generate the initial version of each new language file:
```
pybabel init -i babel/messages.pot -d src/cryptoadvance/specter/translations -l es
pybabel init -i babel/messages.pot -d src/cryptoadvance/specter/translations -l fr
pybabel init -i babel/messages.pot -d src/cryptoadvance/specter/translations -l de
```