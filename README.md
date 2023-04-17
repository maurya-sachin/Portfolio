# Portfolio
Welcome to my portfolio

#Live Server: https://maurya-sachin.github.io/Portfolio/

#Sources & Credits
1.  Logo & Background created at #Canva.
2.  Template followed from Youtube #Easy_tutorial.
3.  Fonts import from #Googlefonts.
4.  Icons fetched from fontawesome.com
5.  GoogleSheet -- form-to-google-sheets>> copied from https://github.com/jamiewilson/form-to-google-sheets.


Pasted following code to create a Google Apps Script
        
        
        var sheetName = 'Sheet1'
        var scriptProp = PropertiesService.getScriptProperties()

        function intialSetup () {
            var activeSpreadsheet = SpreadsheetApp.getActiveSpreadsheet()
            scriptProp.setProperty('key', activeSpreadsheet.getId())
        }

        function doPost (e) {
            var lock = LockService.getScriptLock()
            lock.tryLock(10000)
        
            try {
                var doc = SpreadsheetApp.openById(scriptProp.getProperty('key'))
                var sheet = doc.getSheetByName(sheetName)

                var headers = sheet.getRange(1, 1, 1, sheet.getLastColumn()).getValues()[0]
                var nextRow = sheet.getLastRow() + 1

                var newRow = headers.map(function(header) {
                return header === 'timestamp' ? new Date() : e.parameter[header]
                })

                sheet.getRange(nextRow, 1, 1, newRow.length).setValues([newRow])

                return ContentService
                .createTextOutput(JSON.stringify({ 'result': 'success', 'row': nextRow }))
                .setMimeType(ContentService.MimeType.JSON)
            }

            catch (e) {
                return ContentService
                .createTextOutput(JSON.stringify({ 'result': 'error', 'error': e }))
                .setMimeType(ContentService.MimeType.JSON)
            }

            finally {
                lock.releaseLock()
            }
        }

Input your web app URL

    Open the file named index.html. On line 12 replace <SCRIPT URL> with your script url:
        
        
        <form name="submit-to-google-sheet">
        <input name="email" type="email" placeholder="Email" required>
        <button type="submit">Send</button>
        </form>

        <script>
        const scriptURL = '<SCRIPT URL>'
        const form = document.forms['submit-to-google-sheet']

        form.addEventListener('submit', e => {
            e.preventDefault()
            fetch(scriptURL, { method: 'POST', body: new FormData(form)})
            .then(response => console.log('Success!', response))
            .catch(error => console.error('Error!', error.message))
        })
        </script>

#Challanges faced
1. style.css path error in index.html file resulting in server live preview issue.
2. due to changing template again & again the elements got mixed up thus after learning stated from beginning.
3. background responsiveness was an issue have to create another background for smaller devices.
4. javaScript functions were not working due to mispelling which consumed lot of time in irrevelant troubleshooting.
5. double class name usage in css was also point to remeber to not to add any space in between while styling.
