# FOOTNOTE-EXTRACTOR
# A JS SCRIPT FOR FOOTNOTE IN GOOGLE DOCS TO BE EXTRACTED AND ARRANGED IN A DIFFERENT DOC FILE AT THE LAST PAGE.

function extractFootnotes() {
  var doc = DocumentApp.getActiveDocument();
  var body = doc.getBody();
  var footnotes = [];
  
  // Loop through all paragraphs in the document
  var numChildren = body.getNumChildren();
  for (var i = 0; i < numChildren; i++) {
    var element = body.getChild(i);
    
    // Check if the element is a Paragraph
    if (element.getType() == DocumentApp.ElementType.PARAGRAPH) {
      
      // Check the paragraph for any footnote references
      var numElements = element.getNumChildren();
      for (var j = 0; j < numElements; j++) {
        var childElement = element.getChild(j);
        
        // Check if the child element is an inline footnote
        if (childElement.getType() == DocumentApp.ElementType.FOOTNOTE) {
          var footnoteText = childElement.asFootnote().getFootnoteContents().getText();
          footnotes.push(footnoteText);
        }
      }
    }
  }
  
  // Insert the extracted footnotes at the end of the document
  if (footnotes.length > 0) {
    body.appendParagraph("Extracted References from Footnotes:");
    for (var k = 0; k < footnotes.length; k++) {
      body.appendParagraph((k + 1) + ". " + footnotes[k]);
    }
  } else {
    body.appendParagraph("No footnotes found.");
  }
}

