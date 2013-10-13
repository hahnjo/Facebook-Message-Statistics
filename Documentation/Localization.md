Localize the App
================

To translate the App into a new language, do the following steps:<br />
1. Open the App in your Browser<br />
2. Open the console for Javascript<br />
3. Type ```$.newTranslation()``` and press Enter<br />
4. Copy the logged lines and save it to a file called `<FB-locale>.js` into the directory `l10n/` in your cloned repository [[1]](https://www.facebook.com/translations/FacebookLocales.xml)<br />
5. Adjust the value of `htmlLang` [[2]](http://en.wikipedia.org/wiki/List_of_ISO_639-1_codes)<br />
6. Translate the right values in `l10n`. The left ones MUST be untouched and remain English!<br />
7. Check the result!<br />
8. Commit the result, upload it to your Github and send me a Pull Request