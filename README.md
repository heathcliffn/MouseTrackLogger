# MouseTrackLogger
Betav1
Step One
Buttons to save log data gathered so far
First, we need some buttons to push so that we can capture a snapshot of the users data captured so far, and one to view the data.



<a id="createLogFile">Create Log File</a>
<a id="downloadLogFile">Download Log File</a>
Step Two
Set up the logger.js module
// Anonymous function protects the namespace
(function () {
    var textFile = null,
    data = "", // The data we want to log. We'll populate this in Step Three

    makeTextFile = function (text) {
        var textData = new Blob([text], {type: 'text/plain'});

        // If we are replacing a previously generated file we need to
        // manually revoke the object URL to avoid memory leaks.
        if (textFile !== null) {
            window.URL.revokeObjectURL(textFile);
        }

        textFile = window.URL.createObjectURL(textData);
        return textFile;
    };
})();
The above code defines an anonymous function which is immediately called. Inside this function we define several things, textFile will contain a link to download the textfile.

Step Three
Recording the data
Inside the anonymous function, we'll define how to link the previous two steps.

// When the createLogFile link is clicked,
// we set downloadLogFile's href to the location of the data file.
$("#createLogFile").on("click", function() {
    var link = $('#downloadLogFile');
    link.attr("href", makeTextFile(data));
});

// Anytime mouse is moved on the page,
// append info about the action to our data variable
$(document).mousemove(function(e) {
    data += $.now() + " ";
    data += e.pageX + " ";
    data += e.pageY + " ";
    data += "mousemove\n";
});

// Record similar data for mouseclicks,
// but change the type of the entry
$(document).click(function(e) {
    data += $.now() + " ";
    data += e.pageX + " ";
    data += e.pageY + " ";
    data += "mouseclick\n";
});
This now populates our data variable with information about the user's action.

Conclusion
Putting this all together gives us the following basic structure:

<html>
    <body>
        <a id="createLogFile">Create Log File</a>
        <a id="downloadLogFile">Download Log File</a>

        <script>
            (function () {
                var textFile = null,
                data = "",
                makeTextFile = function (text) {
                    var textData = new Blob([text], {type: 'text/plain'});

                    if (textFile !== null) {
                        window.URL.revokeObjectURL(textFile);
                    }

                    textFile = window.URL.createObjectURL(textData);
                    return textFile;
                };

                $("#createLogFile").on("click", function() {
                    var link = $('#downloadLogFile');
                    link.attr("href", makeTextFile(data));
                });

                $(document).mousemove(function(e) {
                    data += $.now() + " ";
                    data += e.pageX + " ";
                    data += e.pageY + " ";
                    data += "mousemove\n";
                });

                $(document).click(function(e) {
                    data += $.now() + " ";
                    data += e.pageX + " ";
                    data += e.pageY + " ";
                    data += "mouseclick\n";
                });
            })();
        </script>
    </body>
</html>
