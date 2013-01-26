var newButton, openButton, saveButton;
var editor;
var menu;
var fileEntry;
var hasWriteAccess;

var gui = require("nw.gui");
var fs = require("fs");
var clipboard = gui.Clipboard.get();

function handleDocumentChange(title) {
  var mode;
  var modeName;
  if (title) {
    title = title.match(/[^/]+$/)[0];
    document.getElementById("title").innerHTML = title;
    if (title.match(/.scad$/)) {
      mode = "openscad";
      modeName = "OpenSCAD";
    } else if (title.match(/.js$/)) {
      mode = "javascript";
      modeName = "JavaScript";
    }
  } else {
    document.getElementById("title").innerHTML = "[no file loaded]";
  }
  editor.setOption("mode", mode);
  document.getElementById("mode").innerHTML = modeName;
}

function newFile() {
  fileEntry = null;
  hasWriteAccess = false;
  handleDocumentChange(null);
}

function setFile(theFileEntry, isWritable) {
  fileEntry = theFileEntry;
  hasWriteAccess = isWritable;
}

function readFileIntoEditor(theFileEntry) {
  fs.readFile(theFileEntry, function (err, data) {
    if (err) {
      console.log("Read failed: " + err);
    }

    handleDocumentChange(theFileEntry);
    editor.setValue(String(data));
  });
}

function writeEditorToFile(theFileEntry) {
  var str = editor.getValue();
  fs.writeFile(theFileEntry, editor.getValue(), function (err) {
    if (err) {
      console.log("Write failed: " + err);
      return;
    }

    handleDocumentChange(theFileEntry);
    console.log("Write completed.");
  });
}

var onChosenFileToOpen = function(theFileEntry) {
  setFile(theFileEntry, false);
  readFileIntoEditor(theFileEntry);
};

var onChosenFileToSave = function(theFileEntry) {
  setFile(theFileEntry, true);
  writeEditorToFile(theFileEntry);
};

function handleNewButton() {
  if (false) {
    newFile();
    editor.setValue("");
  } else {
    var x = window.screenX + 10;
    var y = window.screenY + 10;
    window.open('main.html', '_blank', 'screenX=' + x + ',screenY=' + y);
  }
}

function handleOpenButton() {
  $("#openFile").trigger("click");
}

function handleSaveButton() {
  if (fileEntry && hasWriteAccess) {
    writeEditorToFile(fileEntry);
  } else {
    $("#saveFile").trigger("click");
  }
}

function getSelectedRange() {
  return { from: editor.getCursor(true), to: editor.getCursor(false) };
}
      
function handleAutoformatButton() {
  var range = getSelectedRange();
  editor.autoFormatRange(range.from, range.to);
}

function handleCommentButton() {
  var range = getSelectedRange(), selStart = editor.getCursor("start");
  editor.commentRange(true, range.from, range.to);
  editor.setSelection(selStart, editor.getCursor("end"));
}

function handleUncommentButton() {
  var range = getSelectedRange(), selStart = editor.getCursor("start");
  editor.commentRange(false, range.from, range.to);
  editor.setSelection(selStart, editor.getCursor("end"));
}

function initContextMenu() {
  menu = new gui.Menu();
  menu.append(new gui.MenuItem({
    label: 'Copy',
    click: function() {
      clipboard.set(editor.getSelection());
    }
  }));
  menu.append(new gui.MenuItem({
    label: 'Cut',
    click: function() {
      clipboard.set(editor.getSelection());
      editor.replaceSelection('');
    }
  }));
  menu.append(new gui.MenuItem({
    label: 'Paste',
    click: function() {
      editor.replaceSelection(clipboard.get());
    }
  }));

  document.getElementById("editor").addEventListener('contextmenu', function(ev) { 
    ev.preventDefault();
    menu.popup(ev.x, ev.y);
    return false;
  });
}


onload = function() {
  initContextMenu();

  newButton = document.getElementById("new");
  openButton = document.getElementById("open");
  saveButton = document.getElementById("save");
  autoformatButton = document.getElementById("autoformat");
  commentButton = document.getElementById("comment");
  uncommentButton = document.getElementById("uncomment");

  newButton.addEventListener("click", handleNewButton);
  openButton.addEventListener("click", handleOpenButton);
  saveButton.addEventListener("click", handleSaveButton);
  autoformatButton.addEventListener("click", handleAutoformatButton);
  commentButton.addEventListener("click", handleCommentButton);
  uncommentButton.addEventListener("click", handleUncommentButton);

  $("#saveFile").change(function(evt) {
    onChosenFileToSave($(this).val());
  });
  $("#openFile").change(function(evt) {
    onChosenFileToOpen($(this).val());
  });
        
  editor = CodeMirror(document.getElementById("editor"), {
    fixedGutter: false,
    mode: "javascript",
    lineNumbers: true,
    lineWrapping: true,
    tabSize: 2,
    collapseRange: true,
    matchBrackets: true,
    theme: "lesser-dark",
    extraKeys: {
      "Cmd-S": function(instance) { handleSaveButton() },
      "Ctrl-S": function(instance) { handleSaveButton() },
      "F5": function(instance) { handleSaveButton() },
      "Ctrl-Space": "autocomplete",
  }
    });
    
  editor.on("cursorActivity", function() {
    editor.matchHighlight("CodeMirror-matchhighlight");
  });
  
  CodeMirror.commands.autocomplete = function(cm) {
    CodeMirror.showHint(cm, CodeMirror.javascriptHint);
  }
  
  newFile();
  onresize();

  gui.Window.get().show();
}

onresize = function() {
  var container = document.getElementById('editor');
  var containerWidth = container.offsetWidth;
  var containerHeight = container.offsetHeight;

  var scrollerElement = editor.getScrollerElement();
  scrollerElement.style.width = containerWidth + 'px';
  scrollerElement.style.height = containerHeight + 'px';

  editor.refresh();
}
